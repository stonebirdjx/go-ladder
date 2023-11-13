<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [sync.Cond](#synccond)
  - [Cond数据结构](#cond%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
  - [Cond 方法](#cond-%E6%96%B9%E6%B3%95)
  - [Wait 操作](#wait-%E6%93%8D%E4%BD%9C)
  - [Signal：唤醒最早 Wait 的 goroutine](#signal%E5%94%A4%E9%86%92%E6%9C%80%E6%97%A9-wait-%E7%9A%84-goroutine)
  - [Cond 的惯用法及使用注意事项](#cond-%E7%9A%84%E6%83%AF%E7%94%A8%E6%B3%95%E5%8F%8A%E4%BD%BF%E7%94%A8%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)
- [sync.Once](#synconce)
  - [Once 数据结构](#once-%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
  - [once 方法](#once-%E6%96%B9%E6%B3%95)
- [sync.Pool](#syncpool)
  - [Pool 的数据结构](#pool-%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
  - [poolChain 的实现](#poolchain-%E7%9A%84%E5%AE%9E%E7%8E%B0)
  - [Put 的实现](#put-%E7%9A%84%E5%AE%9E%E7%8E%B0)
  - [Get 的实现](#get-%E7%9A%84%E5%AE%9E%E7%8E%B0)
  - [对象的清理](#%E5%AF%B9%E8%B1%A1%E7%9A%84%E6%B8%85%E7%90%86)
  - [性能之道](#%E6%80%A7%E8%83%BD%E4%B9%8B%E9%81%93)
- [sync.WaitGroup](#syncwaitgroup)
  - [WaitGroup 数据结构](#waitgroup-%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
  - [WaitGroup方法](#waitgroup%E6%96%B9%E6%B3%95)
    - [Add(delta int)](#adddelta-int)
    - [Done](#done)
    - [Wait](#wait)
- [Select](#select)
  - [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
  - [初始化](#%E5%88%9D%E5%A7%8B%E5%8C%96)
  - [轮询事件](#%E8%BD%AE%E8%AF%A2%E4%BA%8B%E4%BB%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# sync.Cond

cond 使用场景：单生产者多消费者

## Cond数据结构

`sync.Cond` 的 struct 定义如下：

```go
type Cond struct {
	noCopy noCopy

	// L is held while observing or changing the condition
	L Locker

	notify  notifyList
	checker copyChecker
}
```

其中最核心的就是 `notifyList` 这个数据结构, 其源码在 runtime/sema.go

```go
type notifyList struct {
    wait uint32
	notify uint32

	// List of parked waiters.
	lock mutex
	head *sudog
	tail *sudog
}
```

以上代码中，notifyList 包含两类属性：

1. `wait` 和 `notify`。这两个都是ticket值，每次调 Wait 时，ticket 都会递增，作为 goroutine 本次 Wait 的唯一标识，便于下次恢复。 wait 表示下次 `sync.Cond` Wait 的 ticket 值，notify 表示下次要唤醒的 goroutine 的 ticket 的值。这两个值都只增不减的。利用 wait 和 notify 可以实现 goroutine FIFO式的唤醒，具体见下文。
2. `head` 和 `tail`。等待在这个 `sync.Cond` 上的 goroutine 链表，head -> *sudog -> *sudog -> *sudog ... -> tail

## Cond 方法

我们看下 `sync.Cond` 接口的用法:

1. `sync.NewCond(l Locker)`: 新建一个 sync.Cond 变量。注意该函数需要一个 Locker 作为必填参数，这是因为在 `cond.Wait()` 中底层会涉及到 Locker 的锁操作。
2. `cond.Wait()`: 等待被唤醒。唤醒期间会解锁并切走 goroutine。
3. `cond.Signal()`: 只唤醒一个最先 Wait 的 goroutine。对应的另外一个唤醒函数是 `Broadcast`，区别是 `Signa`l 一次只会唤醒一个 goroutine，而 `Broadcast` 会将全部 Wait 的 goroutine 都唤醒。

## Wait 操作

```go
func (c *Cond) Wait() {
	c.checker.check()
	// 获取ticket
	t := runtime_notifyListAdd(&c.notify)
	// 注意这里，必须先解锁，因为 runtime_notifyListWait 要切走 goroutine
	// 所以这里要解锁，要不然其他 goroutine 没法获取到锁了
	c.L.Unlock()
	// 将当前 goroutine 加入到 notifyList 里面，然后切走 goroutine
	runtime_notifyListWait(&c.notify, t)
	// 这里已经唤醒了，因此需要再度锁上
	c.L.Lock()
}
```

Wait 函数虽然短短几行代码，但里面蕴含了很多重要的逻辑。整个逻辑可以拆分为 4 步：

第一步：调用 `runtime_notifyListAdd` 获取 ticket。ticket 是一次 Wait 操作的唯一标识，可以用来防止重复唤醒以及保证 FIFO 式的唤醒。

第二步：`c.L.Unlock()` 先把用户传进来的 locker 解锁。因为在 `runtime_notifyListWait` 中会调用 `gopark` 切走 goroutine。因此在切走之前，必须先把 Locker 解锁了。要不然其他 goroutine 获取不到这个锁，将会造成死锁问题。

第三步：`runtime_notifyListWait` 将当前 goroutine 加入到 notifyList 里面，然后切走goroutine。

## Signal：唤醒最早 Wait 的 goroutine

```go
func (c *Cond) Signal() {
	runtime_notifyListNotifyOne(&c.notify)
}

func notifyListNotifyOne(l *notifyList) {
	// 如果二者相等，说明没有需要唤醒的 goroutine
	if atomic.Load(&l.wait) == atomic.Load(&l.notify) {
		return
	}

	lock(&l.lock)

	t := l.notify
	if t == atomic.Load(&l.wait) {
		unlock(&l.lock)
		return
	}

	// Update the next notify ticket number.
	atomic.Store(&l.notify, t+1)

	for p, s := (*sudog)(nil), l.head; s != nil; p, s = s, s.next {
		if s.ticket == t {
			n := s.next
			if p != nil {
				p.next = n
			} else {
				l.head = n
			}
			if n == nil {
				l.tail = p
			}
			unlock(&l.lock)
			s.next = nil

			// 唤醒 goroutine
			readyWithTime(s, 4)
			return
		}
	}
	unlock(&l.lock)
}
```

那么，每次唤醒的时候，也会对应一个 `notify` 属性。例如当前 `notify` 属性等于 1，则去逐个检查 `notifyList` 链表中 元素，找到 `ticket` 等于 1 的 goroutine 并唤醒，同时将 `notify` 属性进行原子递增。

## Cond 的惯用法及使用注意事项

sync.Cond 在使用时还是有一些需要注意的地方，否则使用不当将造成代码错误。

1. sync.Cond不能拷贝，否则将会造成`panic("sync.Cond is copied")`错误
2. Wait 的调用一定要放在 Lock 和 UnLock 中间，否则将会造成 `panic("sync: unlock of unlocked mutex")` 错误。代码如下:

```go
c.L.Lock()
for !condition() {
       c.Wait()
}
... make use of condition ...
c.L.Unlock()
```

1. Wait 调用的条件检查一定要放在 for 循环中，代码如上。这是因为当 Boardcast 唤醒时，有可能其他 goroutine 先于当前 goroutine 唤醒并抢到锁，导致轮到当前 goroutine 抢到锁的时候，条件又不再满足了。因此，需要将条件检查放在 for 循环中。
2. Signal 和 Boardcast 两个唤醒操作不需要加锁。

<div STYLE="page-break-after: always;"></div>

# sync.Once

`sync.Once` 是 Go 标准库提供的使函数只执行一次的实现，常应用于单例模式，例如初始化配置、保持数据库连接等

## Once 数据结构

```go
// 一个 Once 实例在使用之后不能被拷贝继续使用
type Once struct {
   done uint32 // done 表明了动作是否已经执行
   m    Mutex
}
```

## once 方法

Once 对外仅暴露了唯一的方法 `Do(f func())`，f 为需要执行的函数。

```go
func (o *Once) Do(f func()) {
    if atomic.LoadUint32(&o.done) == 0 {
      o.doSlow(f)
   }
}
```

- 不要拷贝一个 sync.Once 使用或作为参数传递，然后去执行 `Do`，值传递时 `done` 会归0，无法起到限制一次的效果。
- 不要在 `Do` 的 `f` 中嵌套调用 `Do`

<div STYLE="page-break-after: always;"></div>

# sync.Pool 

sync.Pool 是 Golang 内置的对象池技术，可用于缓存临时对象，避免因频繁建立临时对象所带来的消耗以及对 GC 造成的压力。

在许多知名的开源库中，都可以看到 sync.Pool 的大量使用。例如，HTTP 框架 Gin 用 sync.Pool 来复用每个请求都会创建的 `gin.Context` 对象。

> sync.Pool 不仅是并发安全的，而且实现了 lock free

## Pool 的数据结构

sync.Pool 的 代码定义如下sync/pool.go

```go
type Pool struct {
	noCopy noCopy

	local     unsafe.Pointer // local fixed-size per-P pool, actual type is [P]poolLocal
	localSize uintptr        // size of the local array

	victim     unsafe.Pointer // local from previous cycle
	victimSize uintptr        // size of victims array

	New func() interface{}
}
```

在 GMP 调度模型中，M 代表了系统线程，而同一时间一个 M 上只能同时运行一个 P。那么也就意味着，从线程维度来看，在 P 上的逻辑都是单线程执行的。

sync.Pool 就是充分利用了 GMP 这一特点。对于同一个 sync.Pool ，每个 P 都有一个自己的本地对象池 `poolLocal`。

其中，我们需要着重关注以下三个字段：

- `local` 是个数组，长度为 P 的个数。其元素类型是 `poolLocal`。这里面存储着各个 P 对应的本地对象池。可以近似的看做 `[P]poolLocal`。
- `localSize`。代表 local 数组的长度。因为 P 可以在运行时通过调用 runtime.GOMAXPROCS 进行修改, 因此我们还是得通过 `localSize` 来对应 `local` 数组的长度。
- `New` 就是用户提供的创建对象的函数。这个选项也不是必需。当不填的时候，Get 有可能返回 nil。

我们再看下本地对象池 `poolLocal` 的定义，如下：

```go
// 每个 P 都会有一个 poolLocal 的本地
type poolLocal struct {
	poolLocalInternal

	pad [128 - unsafe.Sizeof(poolLocalInternal{})%128]byte
}

type poolLocalInternal struct {
	private interface{}
	shared  poolChain
}
```

`pad` 变量的作用在下文会讲到，这里暂时不展开讨论。我们可以直接看 poolLocalInternal 的定义，其中每个本地对象池，都会包含两项：

- `private` 私有变量。Get 和 Put 操作都会优先存取 private 变量，如果 private 变量可以满足情况，则不再深入进行其他的复杂操作。
- `shared`。其类型为 poolChain，从名字不难看出这个是链表结构，这个就是 P 的本地对象池了。

## poolChain 的实现

poolChain 是个链表结构，其链表头 HEAD 指向最新分配的元素项。链表中的每一项是一个 poolDequeue 对象。poolDequeue 本质上是一个 ring buffer 结构。其对应的代码定义如下：

```go
type poolChain struct {
	head *poolChainElt
	tail *poolChainElt
}

type poolChainElt struct {
	poolDequeue
	next, prev *poolChainElt
}

type poolDequeue struct {
	headTail uint64
	vals []eface
}
```

为什么 poolChain 是这么一个链表 + ring buffer 的复杂结构呢？简单的每个链表项为单一元素不行吗？

使用 ring buffer 是因为它有以下优点：

1. 预先分配好内存，且分配的内存项可不断复用。
2. 由于ring buffer 本质上是个数组，是连续内存结构，非常利于 CPU Cache。在访问poolDequeue 某一项时，其附近的数据项都有可能加载到统一 Cache Line 中，访问速度更快。

## Put 的实现

```go
func (p *Pool) Put(x interface{}) {
	if x == nil {
		return
	}

	l, _ := p.pin()

	if l.private == nil {
		l.private = x
		x = nil
	}

	if x != nil {
		l.shared.pushHead(x)
	}

	runtime_procUnpin()
}
```

在 Put 函数中首先调用了 `pin()`。`pin` 函数非常重要，它有三个作用：

1. **初始化或者重新创建local数组。** 当 local 数组为空，或者和当前的 `runtime.GOMAXPROCS` 不一致时，将触发重新创建 local 数组，以和 P 的个数保持一致。
2. 取当前 P 对应的本地缓存池 poolLocal。其实代码逻辑很简单，就是从 local 数组中根据索引取元素。
3. **防止当前 P 被抢占。** 这点非常重要。

## Get 的实现

```go
func (p *Pool) Get() interface{} {
	l, pid := p.pin()

	x := l.private
	l.private = nil

	if x == nil {
		x, _ = l.shared.popHead()

		if x == nil {
			x = p.getSlow(pid)
		}
	}
	runtime_procUnpin()

	if x == nil && p.New != nil {
		x = p.New()
	}
	return x
}
```

其中 `pin()` 的作用和 `private` 对象的作用，和 PUT 操作中的一致

**从当前 P 的 poolChain 中取数据时，是从链表头部开始取数据。** 具体来说，先取位于链表头的 poolDequeue，然后从 poolDequeue 的头部开始取数据。

## 对象的清理

sync.Pool 没有对外开放对象清理策略和清理接口,当窃取其他 P 的对象时，会逐步淘汰已经为空的 poolDequeue。但除此之外，sync.Pool 一定也还有其他的对象清理机制.

Golang 对 sync.Pool 的清理逻辑非常简单粗暴。首先每个被使用的 sync.Pool，都会在初始化阶段被添加到全局变量 `allPools []*Pool` 对象中。Golang 的 runtime 将会在 **每轮 GC 前**，触发调用 poolCleanup 函数，清理 allPools。

```go
func poolCleanup() {
	for _, p := range oldPools {
		p.victim = nil
		p.victimSize = 0
	}

	for _, p := range allPools {
		p.victim = p.local
		p.victimSize = p.localSize
		p.local = nil
		p.localSize = 0
	}

	oldPools, allPools = allPools, nil
}
```

## 性能之道

回顾下 sync.Pool 的实现细节，总结来说，sync.Pool 利用以下手段将程序性能做到了极致：

1. 利用 GMP 的特性，为每个 P 创建了一个本地对象池 poolLocal，尽量减少并发冲突。
2. 每个 poolLocal 都有一个 private 对象，优先存取 private 对象，可以避免进入复杂逻辑。
3. 在 Get 和 Put 期间，利用 `pin` 锁定当前 P，防止 goroutine 被抢占，造成程序混乱。
4. 在获取对象期间，利用对象窃取的机制，从其他 P 的本地对象池以及 victim 中获取对象。
5. 充分利用 CPU Cache 特性，提升程序性能。

# sync.WaitGroup

## WaitGroup 数据结构

```go
type WaitGroup struct {
    // 避免复制使用的一个技巧，可以告诉vet工具违反了复制使用的规则
    noCopy noCopy
    // 64bit(8bytes)的值分成两段，高32bit是计数值，低32bit是waiter的计数
    // 另外32bit是用作信号量的
    // 因为64bit值的原子操作需要64bit对齐，但是32bit编译器不支持，所以数组中的元素在不同的架构中不一样，具体处理看下面的方法
    // 总之，会找到对齐的那64bit作为state，其余的32bit做信号量
    state1 [3]uint32
}
```

`state1` 长度为 3 的 uint32 数组，Golang 内存对齐的概念

*当 `state1` 是 32 位对齐：`state1` 数组的第一位是 sema，第二位是 counter，第三位是 waiter。*

当 `state1` 是 64 位对齐：`state1` 数组的第一位是 counter，第二位是 waiter，第三位是 sema。

- *`counter` 代表目前尚未完成的个数。`WaitGroup.Add(n)` 将会导致 `counter += n`, 而 `WaitGroup.Done()` 将导致 `counter--`。*
- `waiter` 代表目前已调用 `WaitGroup.Wait` 的 goroutine 的个数。
- `sema` 对应于 golang 中 runtime 内部的信号量的实现。

## WaitGroup方法

- Add(int)
- Done()
- Wait()

### Add(delta int)

```go
func (wg *WaitGroup) Add(delta int) {
  // statep表示wait数和计数值
  // 低32位表示wait数，高32位表示计数值
   statep, semap := wg.state()
   // uint64(delta)<<32 将delta左移32位
    // 因为高32位表示计数值，所以将delta左移32，增加到技术上
   state := atomic.AddUint64(statep, uint64(delta)<<32)
   // 当前计数值
   v := int32(state >> 32)
   // 阻塞在检查点的wait数
   w := uint32(state)
   if v > 0 || w == 0 {
      return
   }

   // 如果计数值v为0并且waiter的数量w不为0，那么state的值就是waiter的数量
    // 将waiter的数量设置为0，因为计数值v也是0,所以它们俩的组合*statep直接设置为0即可。此时需要并唤醒所有的waiter
   *statep = 0
   for ; w != 0; w-- {
      runtime_Semrelease(semap, false, 0)
   }
}
```

WaitGroup里还对使用逻辑进行了严格的检查，比如Wait()一旦开始不能Add().

### Done

```go
// Done decrements the WaitGroup counter by one.
func (wg *WaitGroup) Done() {
    wg.Add(-1)
}
```

### Wait

```go
// Wait blocks until the WaitGroup counter is zero.
func (wg *WaitGroup) Wait() {
    statep := wg.state()
        for {
        state := atomic.LoadUint64(statep)
        v := int32(state >> 32)
        w := uint32(state)
        if v == 0 {
            // Counter is 0, no need to wait.
            if race.Enabled {
                race.Enable()
                race.Acquire(unsafe.Pointer(wg))
            }
            return
        }
        // Increment waiters count.
        // 如果statep和state相等，则增加等待计数，同时进入if等待信号量
        // 此处做CAS，主要是防止多个goroutine里进行Wait()操作，每有一个goroutine进行了wait，等待计数就加1
        // 如果这里不相等，说明statep，在 从读出来 到 CAS比较 的这个时间区间内，被别的goroutine改写了，那么不进入if，回去再读一次，这样写避免用锁，更高效些
        if atomic.CompareAndSwapUint64(statep, state, state+1) {
            if race.Enabled && w == 0 {
                // Wait must be synchronized with the first Add.
                // Need to model this is as a write to race with the read in Add.
                // As a consequence, can do the write only for the first waiter,
                // otherwise concurrent Waits will race with each other.
                race.Write(unsafe.Pointer(&wg.sema))
            }
            // 等待信号量
            runtime_Semacquire(&wg.sema)
            // 信号量来了，代表所有Add都已经Done
            if *statep != 0 {
                // 走到这里，说明在所有Add都已经Done后，触发信号量后，又被执行了Add
                panic("sync: WaitGroup is reused before previous Wait has returned")
            }
            return
        }
    }
}
```

<div STYLE="page-break-after: always;"></div>



<div STYLE="page-break-after: always;"></div>

# Select

I/O 多路复用被用来处理同一个事件循环中的多个 I/O 事件。I/O 多路复用需要使用特定的系统调用，最常见的系统调用是 [`select`](https://github.com/torvalds/linux/blob/f757165705e92db62f85a1ad287e9251d1f2cd82/fs/select.c#L722)，该函数可以同时监听最多 1024 个文件描述符的可读或者可写状态：

**几大特点**

- 可以实现两种收发操作，阻塞收发和非阻塞收发
- 当多个case ready的情况下会随机选择一个执行，不是顺序执行
- 没有ready的case时，有default语句，执行default语句;没有default语句，阻塞直到某个case ready
- select中的case必须是channel操作
- default语句总是可运行的

**几大用处**

- 超时处理，一旦超时就返回，不再等待
- 生产，消费者通信、
- 非阻塞读写

## 数据结构

select 中的case用runtime.scase结构体来表示，具体如下

```go
type scase struct {
	c           *hchan         // 除了default其他都是channel操作，所以需要一个channel变量存储通道信息
	elem        unsafe.Pointer // data element
	kind        uint16//case类型 default语句是caseDefault类型，接收通道是caseRecv，发送通道是caseSend
	pc          uintptr // race pc (for race detector / msan)
	releasetime int64
}
```

通道类型具体代码定义如下

```js
const (
	caseNil = iota
	caseRecv
	caseSend
	caseDefault
)
```

编译器在中间代码生成期间会根据 select 中 case 的不同对控制语句进行优化，这一过程都发在 cmd/compile/internal/gc.walkselectcases 函数中，常规编译之后，调用方法runtime.selectgo，具体操作都在这个方法里面，传入的参数是scase数组，传出的参数是随机选择的ready的scase下标，如下

```go
func selectgo(cas0 *scase, order0 *uint16, ncases int) (int, bool) {
	if debugSelect {
		print("select: cas0=", cas0, "\n")
	}
...
```

## 初始化

因为文件 I/O、网络 I/O 以及计时器都依赖网络轮询器，所以 Go 语言会通过以下两条不同路径初始化网络轮询器：

1. [`internal/poll.pollDesc.init`](https://draveness.me/golang/tree/internal/poll.pollDesc.init) — 通过 [`net.netFD.init`](https://draveness.me/golang/tree/net.netFD.init) 和 [`os.newFile`](https://draveness.me/golang/tree/os.newFile) 初始化网络 I/O 和文件 I/O 的轮询信息时；
2. [`runtime.doaddtimer`](https://draveness.me/golang/tree/runtime.doaddtimer) — 向处理器中增加新的计时器时；

网络轮询器的初始化会使用 [`runtime.poll_runtime_pollServerInit`](https://draveness.me/golang/tree/runtime.poll_runtime_pollServerInit) 和 [`runtime.netpollGenericInit`](https://draveness.me/golang/tree/runtime.netpollGenericInit) 两个函数：

```go
func poll_runtime_pollServerInit() {
	netpollGenericInit()
}

func netpollGenericInit() {
	if atomic.Load(&netpollInited) == 0 {
		lock(&netpollInitLock)
		if netpollInited == 0 {
			netpollinit()
			atomic.Store(&netpollInited, 1)
		}
		unlock(&netpollInitLock)
	}
}
```

[`runtime.netpollGenericInit`](https://draveness.me/golang/tree/runtime.netpollGenericInit) 会调用平台上特定实现的 [`runtime.netpollinit`](https://draveness.me/golang/tree/runtime.netpollinit)，即 Linux 上的 `epoll`，它主要做了以下几件事情：

1. 是调用 `epollcreate1` 创建一个新的 `epoll` 文件描述符，这个文件描述符会在整个程序的生命周期中使用；
2. 通过 [`runtime.nonblockingPipe`](https://draveness.me/golang/tree/runtime.nonblockingPipe) 创建一个用于通信的管道；
3. 使用 `epollctl` 将用于读取数据的文件描述符打包成 `epollevent` 事件加入监听；

```go
var (
	epfd int32 = -1
	netpollBreakRd, netpollBreakWr uintptr
)

func netpollinit() {
	epfd = epollcreate1(_EPOLL_CLOEXEC)
	r, w, _ := nonblockingPipe()
	ev := epollevent{
		events: _EPOLLIN,
	}
	*(**uintptr)(unsafe.Pointer(&ev.data)) = &netpollBreakRd
	epollctl(epfd, _EPOLL_CTL_ADD, r, &ev)
	netpollBreakRd = uintptr(r)
	netpollBreakWr = uintptr(w)
}
```

初始化的管道为我们提供了中断多路复用等待文件描述符中事件的方法，[`runtime.netpollBreak`](https://draveness.me/golang/tree/runtime.netpollBreak) 会向管道中写入数据唤醒 `epoll`：

```go
func netpollBreak() {
	for {
		var b byte
		n := write(netpollBreakWr, unsafe.Pointer(&b), 1)
		if n == 1 {
			break
		}
		if n == -_EINTR {
			continue
		}
		if n == -_EAGAIN {
			return
		}
	}
}
```

因为目前的计时器由网络轮询器管理和触发，它能够让网络轮询器立刻返回并让运行时检查是否有需要触发的计时器。

## 轮询事件

调用 [`internal/poll.pollDesc.init`](https://draveness.me/golang/tree/internal/poll.pollDesc.init) 初始化文件描述符时不止会初始化网络轮询器，还会通过 [`runtime.poll_runtime_pollOpen`](https://draveness.me/golang/tree/runtime.poll_runtime_pollOpen) 重置轮询信息 [`runtime.pollDesc`](https://draveness.me/golang/tree/runtime.pollDesc) 并调用 [`runtime.netpollopen`](https://draveness.me/golang/tree/runtime.netpollopen) 初始化轮询事件：

```go
func poll_runtime_pollOpen(fd uintptr) (*pollDesc, int) {
	pd := pollcache.alloc()
	lock(&pd.lock)
	if pd.wg != 0 && pd.wg != pdReady {
		throw("runtime: blocked write on free polldesc")
	}
	...
	pd.fd = fd
	pd.closing = false
	pd.everr = false
	...
	pd.wseq++
	pd.wg = 0
	pd.wd = 0
	unlock(&pd.lock)

	var errno int32
	errno = netpollopen(fd, pd)
	return pd, int(errno)
}
```

[`runtime.netpollopen`](https://draveness.me/golang/tree/runtime.netpollopen) 的实现非常简单，它会调用 `epollctl` 向全局的轮询文件描述符 `epfd` 中加入新的轮询事件监听文件描述符的可读和可写状态：

```go
func netpollopen(fd uintptr, pd *pollDesc) int32 {
	var ev epollevent
	ev.events = _EPOLLIN | _EPOLLOUT | _EPOLLRDHUP | _EPOLLET
	*(**pollDesc)(unsafe.Pointer(&ev.data)) = pd
	return -epollctl(epfd, _EPOLL_CTL_ADD, int32(fd), &ev)
}
```

从全局的 `epfd` 中删除待监听的文件描述符可以使用 [`runtime.netpollclose`](https://draveness.me/golang/tree/runtime.netpollclose)，因为该函数的实现与 [`runtime.netpollopen`](https://draveness.me/golang/tree/runtime.netpollopen) 比较相似，所以这里不展开分析了。

