<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [前言](#%E5%89%8D%E8%A8%80)
- [sync.Mutex](#syncmutex)
  - [Mutex结构体](#mutex%E7%BB%93%E6%9E%84%E4%BD%93)
  - [Mutex方法](#mutex%E6%96%B9%E6%B3%95)
  - [加锁](#%E5%8A%A0%E9%94%81)
  - [解锁](#%E8%A7%A3%E9%94%81)
  - [自旋过程](#%E8%87%AA%E6%97%8B%E8%BF%87%E7%A8%8B)
    - [自旋条件](#%E8%87%AA%E6%97%8B%E6%9D%A1%E4%BB%B6)
    - [自旋的优势](#%E8%87%AA%E6%97%8B%E7%9A%84%E4%BC%98%E5%8A%BF)
    - [自旋的问题](#%E8%87%AA%E6%97%8B%E7%9A%84%E9%97%AE%E9%A2%98)
  - [Mutex模式](#mutex%E6%A8%A1%E5%BC%8F)
    - [normal模式](#normal%E6%A8%A1%E5%BC%8F)
    - [starvation模式](#starvation%E6%A8%A1%E5%BC%8F)
- [sync.RWMutex](#syncrwmutex)
  - [RWMutex数据结构](#rwmutex%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
  - [RWMutex方法](#rwmutex%E6%96%B9%E6%B3%95)
    - [Lock() 逻辑](#lock-%E9%80%BB%E8%BE%91)
    - [Unlock()实现逻辑](#unlock%E5%AE%9E%E7%8E%B0%E9%80%BB%E8%BE%91)
    - [RLock()实现逻辑](#rlock%E5%AE%9E%E7%8E%B0%E9%80%BB%E8%BE%91)
    - [RUnlock()实现逻辑](#runlock%E5%AE%9E%E7%8E%B0%E9%80%BB%E8%BE%91)
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
- [sync.Map](#syncmap)
  - [sync.Map数据结构](#syncmap%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
  - [sync.Map 常用的有以下方法：](#syncmap-%E5%B8%B8%E7%94%A8%E7%9A%84%E6%9C%89%E4%BB%A5%E4%B8%8B%E6%96%B9%E6%B3%95)
    - [Load](#load)
    - [Store](#store)
    - [Delete](#delete)
  - [总结](#%E6%80%BB%E7%BB%93)
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
- [context](#context)
  - [接口定义](#%E6%8E%A5%E5%8F%A3%E5%AE%9A%E4%B9%89)
  - [设计原理](#%E8%AE%BE%E8%AE%A1%E5%8E%9F%E7%90%86)
  - [默认上下文](#%E9%BB%98%E8%AE%A4%E4%B8%8A%E4%B8%8B%E6%96%87)
  - [取消信号](#%E5%8F%96%E6%B6%88%E4%BF%A1%E5%8F%B7)
  - [传值方法](#%E4%BC%A0%E5%80%BC%E6%96%B9%E6%B3%95)
- [系统监控](#%E7%B3%BB%E7%BB%9F%E7%9B%91%E6%8E%A7)
  - [监控循环](#%E7%9B%91%E6%8E%A7%E5%BE%AA%E7%8E%AF)
    - [检查死锁](#%E6%A3%80%E6%9F%A5%E6%AD%BB%E9%94%81)
    - [运行计时器](#%E8%BF%90%E8%A1%8C%E8%AE%A1%E6%97%B6%E5%99%A8)
    - [轮询网络 #](#%E8%BD%AE%E8%AF%A2%E7%BD%91%E7%BB%9C-)
    - [抢占处理器 #](#%E6%8A%A2%E5%8D%A0%E5%A4%84%E7%90%86%E5%99%A8-)
    - [垃圾回收](#%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6)
- [内存管理](#%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86)
  - [分配方法](#%E5%88%86%E9%85%8D%E6%96%B9%E6%B3%95)
  - [内存分配](#%E5%86%85%E5%AD%98%E5%88%86%E9%85%8D)
    - [span](#span)
    - [class](#class)
    - [总结](#%E6%80%BB%E7%BB%93-1)
  - [垃圾回收](#%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6-1)
    - [内存标记(Mark)](#%E5%86%85%E5%AD%98%E6%A0%87%E8%AE%B0mark)
    - [三色标记法](#%E4%B8%89%E8%89%B2%E6%A0%87%E8%AE%B0%E6%B3%95)
    - [Stop The World](#stop-the-world)
    - [垃圾回收优化](#%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E4%BC%98%E5%8C%96)
      - [写屏障(Write Barrier)](#%E5%86%99%E5%B1%8F%E9%9A%9Cwrite-barrier)
      - [辅助GC(Mutator Assist)](#%E8%BE%85%E5%8A%A9gcmutator-assist)
    - [垃圾回收触发时机](#%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E8%A7%A6%E5%8F%91%E6%97%B6%E6%9C%BA)
      - [内存分配量达到阀值触发GC](#%E5%86%85%E5%AD%98%E5%88%86%E9%85%8D%E9%87%8F%E8%BE%BE%E5%88%B0%E9%98%80%E5%80%BC%E8%A7%A6%E5%8F%91gc)
      - [定期触发GC](#%E5%AE%9A%E6%9C%9F%E8%A7%A6%E5%8F%91gc)
      - [手动触发](#%E6%89%8B%E5%8A%A8%E8%A7%A6%E5%8F%91)
    - [GC性能优化](#gc%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96)
  - [内存逃逸](#%E5%86%85%E5%AD%98%E9%80%83%E9%80%B8)
    - [指针逃逸](#%E6%8C%87%E9%92%88%E9%80%83%E9%80%B8)
    - [栈空间不足逃逸](#%E6%A0%88%E7%A9%BA%E9%97%B4%E4%B8%8D%E8%B6%B3%E9%80%83%E9%80%B8)
    - [动态类型逃逸](#%E5%8A%A8%E6%80%81%E7%B1%BB%E5%9E%8B%E9%80%83%E9%80%B8)
    - [闭包引用对象逃逸](#%E9%97%AD%E5%8C%85%E5%BC%95%E7%94%A8%E5%AF%B9%E8%B1%A1%E9%80%83%E9%80%B8)
    - [总结](#%E6%80%BB%E7%BB%93-2)
- [代码生成](#%E4%BB%A3%E7%A0%81%E7%94%9F%E6%88%90)
- [泛型原理](#%E6%B3%9B%E5%9E%8B%E5%8E%9F%E7%90%86)
  - [性能问题：](#%E6%80%A7%E8%83%BD%E9%97%AE%E9%A2%98)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

![Golang](E:\google\Download\Golang.png)

<div STYLE="page-break-after: always;"></div>

# sync.Mutex

## Mutex结构体

源码包`src/sync/mutex.go:Mutex`定义了互斥锁的数据结构：

```go
type Mutex struct {
    state int32
    sema  uint32
}
```

- Mutex.state表示互斥锁的状态，比如是否被锁定等。
- Mutex.sema表示信号量，协程阻塞等待该信号量，解锁的协程释放信号量从而唤醒等待信号量的协程。

我们看到Mutex.state是32位的整型变量，内部实现时把该变量分成四份，用于记录Mutex的四种状态。

下图展示Mutex的内存布局：

state: 29bit（Waiter）、1bit（Starving）、1bit（Woken）、1bit（Locked）

- Locked: 表示该Mutex是否已被锁定，0：没有锁定 1：已被锁定。
- Woken: 表示是否有协程已被唤醒，0：没有协程唤醒 1：已有协程唤醒，正在加锁过程中。
- Starving：表示该Mutex是否处理饥饿状态， 0：没有饥饿 1：饥饿状态，说明有协程阻塞了超过1ms。
- Waiter: 表示阻塞等待锁的协程个数，协程解锁时根据此值来判断是否需要释放信号量。

协程之间抢锁实际上是抢给Locked赋值的权利，能给Locked域置1，就说明抢锁成功。抢不到的话就阻塞等待Mutex.sema信号量，一旦持有锁的协程解锁，等待的协程会依次被唤醒。

Woken和Starving主要用于控制协程间的抢锁过程，后面再进行了解。

## Mutex方法

Mutext对外提供两个方法，实际上也只有这两个方法：

- Lock() : 加锁方法
- Unlock(): 解锁方法

下面我们分析一下加锁和解锁的过程，加锁分成功和失败两种情况，成功的话直接获取锁，失败后当前协程被阻塞，同样，解锁时跟据是否有阻塞协程也有两种处理。

## 加锁

假定当前只有一个协程在加锁，没有其他协程干扰，加锁过程会去判断Locked标志位是否为0，如果是0则把Locked位置1，代表加锁成功。从上图可见，加锁成功后，只是Locked位置1，其他状态位没发生变化。

假定加锁时，锁已被其他协程占用了，当协程B对一个已被占用的锁再次加锁时，Waiter计数器增加了1，此时协程B将被阻塞，直到Locked值变为0后才会被唤醒。

## 解锁

假定解锁时，没有其他协程阻塞，由于没有其他协程阻塞等待加锁，所以此时解锁时只需要把Locked位置为0即可，不需要释放信号量。

假定解锁时，有1个或多个协程阻塞，协程A解锁过程分为两个步骤，一是把Locked位置0，二是查看到Waiter>0，所以释放一个信号量，唤醒一个阻塞的协程，被唤醒的协程B把Locked位置1，于是协程B获得锁。

## 自旋过程

加锁时，如果当前Locked位为1，说明该锁当前由其他协程持有，尝试加锁的协程并不是马上转入阻塞，而是会持续的探测Locked位是否变为0，这个过程即为自旋过程。

自旋的好处是，当加锁失败时不必立即转入阻塞，有一定机会获取到锁，这样可以避免协程的切换。

自旋对应于CPU的"PAUSE"指令，CPU对该指令什么都不做，相当于CPU空转，对程序而言相当于sleep了一小段时间，时间非常短，当前实现是30个时钟周期。

自旋过程中会持续探测Locked是否变为0，连续两次探测间隔就是执行这些PAUSE指令，它不同于sleep，不需要将协程转为睡眠状态。

### 自旋条件

加锁时程序会自动判断是否可以自旋，无限制的自旋将会给CPU带来巨大压力，所以判断是否可以自旋就很重要了。

自旋必须满足以下所有条件：

- 自旋次数要足够小，通常为4，即自旋最多4次
- CPU核数要大于1，否则自旋没有意义，因为此时不可能有其他协程释放锁
- 协程调度机制中的Process数量要大于1，比如使用GOMAXPROCS()将处理器设置为1就不能启用自旋
- 协程调度机制中的可运行队列必须为空，否则会延迟协程调度

可见，自旋的条件是很苛刻的，总而言之就是不忙的时候才会启用自旋。

### 自旋的优势

自旋的优势是更充分的利用CPU，尽量避免协程切换。因为当前申请加锁的协程拥有CPU，如果经过短时间的自旋可以获得锁，当前协程可以继续运行，不必进入阻塞状态。

### 自旋的问题

如果自旋过程中获得锁，那么之前被阻塞的协程将无法获得锁，如果加锁的协程特别多，每次都通过自旋获得锁，那么之前被阻塞的进程将很难获得锁，从而进入饥饿状态。

为了避免协程长时间无法获取锁，自1.8版本以来增加了一个状态，即Mutex的Starving状态。这个状态下不会自旋，一旦有协程释放锁，那么一定会唤醒一个协程并成功加锁。

## Mutex模式

前面分析加锁和解锁过程中只关注了Waiter和Locked位的变化，现在我们看一下Starving位的作用。

每个Mutex都有两个模式，称为Normal和Starving。下面分别说明这两个模式。

### normal模式

默认情况下，Mutex的模式为normal。

该模式下，协程如果加锁不成功不会立即转入阻塞排队，而是判断是否满足自旋的条件，如果满足则会启动自旋过程，尝试抢锁。

### starvation模式

自旋过程中能抢到锁，一定意味着同一时刻有协程释放了锁，我们知道释放锁时如果发现有阻塞等待的协程，还会释放一个信号量来唤醒一个等待协程，被唤醒的协程得到CPU后开始运行，此时发现锁已被抢占了，自己只好再次阻塞，不过阻塞前会判断自上次阻塞到本次阻塞经过了多长时间，如果超过1ms的话，会将Mutex标记为"饥饿"模式，然后再阻塞。

处于饥饿模式下，不会启动自旋过程，也即一旦有协程释放了锁，那么一定会唤醒协程，被唤醒的协程将会成功获取锁，同时也会把等待计数减1。

<div STYLE="page-break-after: always;"></div>

# sync.RWMutex

实现读写锁需要解决如下几个问题：

1. 写锁需要阻塞写锁：一个协程拥有写锁时，其他协程写锁定需要阻塞
2. 写锁需要阻塞读锁：一个协程拥有写锁时，其他协程读锁定需要阻塞
3. 读锁需要阻塞写锁：一个协程拥有读锁时，其他协程写锁定需要阻塞
4. 读锁不能阻塞读锁：一个协程拥有读锁时，其他协程也可以拥有读锁

## RWMutex数据结构

源码包`src/sync/rwmutex.go:RWMutex`定义了读写锁数据结构：

```go
type RWMutex struct {
    w           Mutex  //用于控制多个写锁，获得写锁首先要获取该锁，如果有一个写锁在进行，那么再到来的写锁将会阻塞于此
    writerSem   uint32 //写阻塞等待的信号量，最后一个读者释放锁时会释放信号量
    readerSem   uint32 //读阻塞的协程等待的信号量，持有写锁的协程释放锁后会释放信号量
    readerCount int32  //记录读者个数
    readerWait  int32  //记录写阻塞时读者个数
}
```

## RWMutex方法

RWMutex提供4个简单的接口来提供服务：

- RLock()：读锁定
- RUnlock()：解除读锁定
- Lock(): 写锁定，与Mutex完全一致
- Unlock()：解除写锁定，与Mutex完全一致

### Lock() 逻辑

写锁定操作需要做两件事：

- 获取互斥锁
- 阻塞等待所有读操作结束（如果有的话）

### Unlock()实现逻辑

解除写锁定要做两件事：

- 唤醒因读锁定而被阻塞的协程（如果有的话）
- 解除互斥锁

### RLock()实现逻辑

读锁定需要做两件事：

- 增加读操作计数，即readerCount++
- 阻塞等待写操作结束(如果有的话)

### RUnlock()实现逻辑

解除读锁定需要做两件事：

- 减少读操作计数，即readerCount--
- 唤醒等待写操作的协程（如果有的话）

<div STYLE="page-break-after: always;"></div>

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

<div STYLE="page-break-after: always;"></div>

# sync.Map

Go 的内建 `map` 是不支持并发写操作的，原因是 `map` 写操作不是并发安全的，当你尝试多个 Goroutine 操作同一个 `map`，会产生报错：`fatal error: concurrent map writes`。

`sync.Map` 的实现原理可概括为：

- 通过 read 和 dirty 两个字段将读写分离，读的数据存在只读字段 read 上，将最新写入的数据则存在 dirty 字段上
- 读取时会先查询 read，不存在再查询 dirty，写入时则只写入 dirty
- 读取 read 并不需要加锁，而读或写 dirty 都需要加锁
- 另外有 misses 字段来统计 read 被穿透的次数（被穿透指需要读 dirty 的情况），超过一定次数则将 dirty 数据同步到 read 上
- 对于删除数据则直接通过标记来延迟删除

## sync.Map数据结构

```go
type Map struct {
    // 加锁作用，保护 dirty 字段
    mu Mutex
    // 只读的数据，实际数据类型为 readOnly
    read atomic.Value
    // 最新写入的数据
    dirty map[interface{}]*entry
    // 计数器，每次需要读 dirty 则 +1
    misses int
}
```

**readOnly** 的数据结构

```go
type readOnly struct {
    // 内建 map
    m  map[interface{}]*entry
    // 表示 dirty 里存在 read 里没有的 key，通过该字段决定是否加锁读 dirty
    amended bool
}
```

**entry** 数据结构则用于存储值的指针

```go
type entry struct {
    p unsafe.Pointer  // 等同于 *interface{}
}
```

## sync.Map 常用的有以下方法：

- `Load`：读取指定 key 返回 value
- `Store`： 存储（增或改）key-value
- `Delete`： 删除指定 key
- `Range`：遍历所有键值对，参数是回调函数
- `LoadOrStore`：读取数据，若不存在则保存再读取

### Load

```go
func (m *Map) Load(key interface{}) (value interface{}, ok bool) {
    // 首先尝试从 read 中读取 readOnly 对象
    read, _ := m.read.Load().(readOnly)
    e, ok := read.m[key]

    // 如果不存在则尝试从 dirty 中获取
    if !ok && read.amended {
        m.mu.Lock()
        // 由于上面 read 获取没有加锁，为了安全再检查一次
        read, _ = m.read.Load().(readOnly)
        e, ok = read.m[key]

        // 确实不存在则从 dirty 获取
        if !ok && read.amended {
            e, ok = m.dirty[key]
            // 调用 miss 的逻辑
            m.missLocked()
        }
        m.mu.Unlock()
    }

    if !ok {
        return nil, false
    }
    // 从 entry.p 读取值
    return e.load()
}

func (m *Map) missLocked() {
    m.misses++
    if m.misses < len(m.dirty) {
        return
    }
    // 当 miss 积累过多，会将 dirty 存入 read，然后 将 amended = false，且 m.dirty = nil
    m.read.Store(readOnly{m: m.dirty})
    m.dirty = nil
    m.misses = 0
}
```

### Store

```go
func (m *Map) Store(key, value interface{}) {
    read, _ := m.read.Load().(readOnly)
    // 如果 read 里存在，则尝试存到 entry 里
    if e, ok := read.m[key]; ok && e.tryStore(&value) {
        return
    }

    // 如果上一步没执行成功，则要分情况处理
    m.mu.Lock()
    read, _ = m.read.Load().(readOnly)
    // 和 Load 一样，重新从 read 获取一次
    if e, ok := read.m[key]; ok {
        // 情况 1：read 里存在
        if e.unexpungeLocked() {
            // 如果 p == expunged，则需要先将 entry 赋值给 dirty（因为 expunged 数据不会留在 dirty）
            m.dirty[key] = e
        }
        // 用值更新 entry
        e.storeLocked(&value)
    } else if e, ok := m.dirty[key]; ok {
        // 情况 2：read 里不存在，但 dirty 里存在，则用值更新 entry
        e.storeLocked(&value)
    } else {
        // 情况 3：read 和 dirty 里都不存在
        if !read.amended {
            // 如果 amended == false，则调用 dirtyLocked 将 read 拷贝到 dirty（除了被标记删除的数据）
            m.dirtyLocked()
            // 然后将 amended 改为 true
            m.read.Store(readOnly{m: read.m, amended: true})
        }
        // 将新的键值存入 dirty
        m.dirty[key] = newEntry(value)
    }
    m.mu.Unlock()
}

func (e *entry) tryStore(i *interface{}) bool {
    for {
        p := atomic.LoadPointer(&e.p)
        if p == expunged {
            return false
        }
        if atomic.CompareAndSwapPointer(&e.p, p, unsafe.Pointer(i)) {
            return true
        }
    }
}

func (e *entry) unexpungeLocked() (wasExpunged bool) {
    return atomic.CompareAndSwapPointer(&e.p, expunged, nil)
}

func (e *entry) storeLocked(i *interface{}) {
    atomic.StorePointer(&e.p, unsafe.Pointer(i))
}

func (m *Map) dirtyLocked() {
    if m.dirty != nil {
        return
    }

    read, _ := m.read.Load().(readOnly)
    m.dirty = make(map[interface{}]*entry, len(read.m))
    for k, e := range read.m {
        // 判断 entry 是否被删除，否则就存到 dirty 中
        if !e.tryExpungeLocked() {
            m.dirty[k] = e
        }
    }
}

func (e *entry) tryExpungeLocked() (isExpunged bool) {
    p := atomic.LoadPointer(&e.p)
    for p == nil {
        // 如果有 p == nil（即键值对被 delete），则会在这个时机被置为 expunged
        if atomic.CompareAndSwapPointer(&e.p, nil, expunged) {
            return true
        }
        p = atomic.LoadPointer(&e.p)
    }
    return p == expunged
}
```

### Delete

```go
func (m *Map) Delete(key interface{}) {
    m.LoadAndDelete(key)
}

// LoadAndDelete 作用等同于 Delete，并且会返回值与是否存在
func (m *Map) LoadAndDelete(key interface{}) (value interface{}, loaded bool) {
    // 获取逻辑和 Load 类似，read 不存在则查询 dirty
    read, _ := m.read.Load().(readOnly)
    e, ok := read.m[key]
    if !ok && read.amended {
        m.mu.Lock()
        read, _ = m.read.Load().(readOnly)
        e, ok = read.m[key]
        if !ok && read.amended {
            e, ok = m.dirty[key]
            m.missLocked()
        }
        m.mu.Unlock()
    }
    // 查询到 entry 后执行删除
    if ok {
        // 将 entry.p 标记为 nil，数据并没有实际删除
        // 真正删除数据并被被置为 expunged，是在 Store 的 tryExpungeLocked 中
        return e.delete()
    }
    return nil, false
}
```

## 总结

可见，通过这种读写分离的设计，解决了并发情况的写入安全，又使读取速度在大部分情况可以接近内建 `map`，非常适合读多写少的情况。

<div STYLE="page-break-after: always;"></div>

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



# context

Go 语言中用来设置截止日期、同步信号，传递请求相关值的结构体。上下文与 Goroutine 有比较密切的关系，是 Go 语言中独特的设计，在其他编程语言中我们很少见到类似的概念。

## 接口定义

源码包中src/context/context.go:Context 定义了该接口：

```go
type Context interface {
    Deadline() (deadline time.Time, ok bool)
    Done() <-chan struct{}
    Err() error
    Value(key interface{}) interface{}
}

```

[`context.Context`](https://draveness.me/golang/tree/context.Context) 是 Go 语言在 1.7 版本中引入标准库的接口[1](https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-context/#fn:1)，该接口定义了四个需要实现的方法，其中包括：

1. `Deadline` — 返回 [`context.Context`](https://draveness.me/golang/tree/context.Context) 被取消的时间，也就是完成工作的截止日期；
2. `Done` — 返回一个 Channel，这个 Channel 会在当前工作完成或者上下文被取消后关闭，多次调用 `Done` 方法会返回同一个 Channel；
3. Err — 返回`context.Context`结束的原因，它只会在Done方法对应的 Channel 关闭时返回非空的值；
   1. 如果 [`context.Context`](https://draveness.me/golang/tree/context.Context) 被取消，会返回 `Canceled` 错误；
   2. 如果 [`context.Context`](https://draveness.me/golang/tree/context.Context) 超时，会返回 `DeadlineExceeded` 错误；
4. `Value` — 从 [`context.Context`](https://draveness.me/golang/tree/context.Context) 中获取键对应的值，对于同一个上下文来说，多次调用 `Value` 并传入相同的 `Key` 会返回相同的结果，该方法可以用来传递请求特定的数据；

## 设计原理

在 Goroutine 构成的树形结构中对信号进行同步以减少计算资源的浪费是 [`context.Context`](https://draveness.me/golang/tree/context.Context) 的最大作用。每一个 [`context.Context`](https://draveness.me/golang/tree/context.Context) 都会从最顶层的 Goroutine 一层一层传递到最下层。

我们可以通过一个代码片段了解 [`context.Context`](https://draveness.me/golang/tree/context.Context) 是如何对信号进行同步的。在这段代码中，我们创建了一个过期时间为 1s 的上下文，并向上下文传入 `handle` 函数，该方法会使用 500ms 的时间处理传入的请求：

```go
func main() {
	ctx, cancel := context.WithTimeout(context.Background(), 1*time.Second)
	defer cancel()

	go handle(ctx, 500*time.Millisecond)
	select {
	case <-ctx.Done():
		fmt.Println("main", ctx.Err())
	}
}

func handle(ctx context.Context, duration time.Duration) {
	select {
	case <-ctx.Done():
		fmt.Println("handle", ctx.Err())
	case <-time.After(duration):
		fmt.Println("process request with", duration)
	}
}
```

## 默认上下文

[`context`](https://github.com/golang/go/tree/master/src/context) 包中最常用的方法还是 [`context.Background`](https://draveness.me/golang/tree/context.Background)、[`context.TODO`](https://draveness.me/golang/tree/context.TODO)，这两个方法都会返回预先初始化好的私有变量 `background` 和 `todo`，它们会在同一个 Go 程序中被复用：

```go
func Background() Context {
	return background
}

func TODO() Context {
	return todo
}
```

这两个私有变量都是通过 `new(emptyCtx)` 语句初始化的，它们是指向私有结构体 [`context.emptyCtx`](https://draveness.me/golang/tree/context.emptyCtx) 的指针，这是最简单、最常用的上下文类型：

```go
type emptyCtx int

func (*emptyCtx) Deadline() (deadline time.Time, ok bool) {
	return
}

func (*emptyCtx) Done() <-chan struct{} {
	return nil
}

func (*emptyCtx) Err() error {
	return nil
}

func (*emptyCtx) Value(key interface{}) interface{} {
	return nil
}
```

从源代码来看，[`context.Background`](https://draveness.me/golang/tree/context.Background) 和 [`context.TODO`](https://draveness.me/golang/tree/context.TODO) 也只是互为别名，没有太大的差别，只是在使用和语义上稍有不同：

- [`context.Background`](https://draveness.me/golang/tree/context.Background) 是上下文的默认值，所有其他的上下文都应该从它衍生出来；
- [`context.TODO`](https://draveness.me/golang/tree/context.TODO) 应该仅在不确定应该使用哪种上下文时使用；

## 取消信号

[`context.WithCancel`](https://draveness.me/golang/tree/context.WithCancel) 函数能够从 [`context.Context`](https://draveness.me/golang/tree/context.Context) 中衍生出一个新的子上下文并返回用于取消该上下文的函数。一旦我们执行返回的取消函数，当前上下文以及它的子上下文都会被取消，所有的 Goroutine 都会同步收到这一取消信号。

```go
func WithCancel(parent Context) (ctx Context, cancel CancelFunc) {
	c := newCancelCtx(parent)
	propagateCancel(parent, &c)
	return &c, func() { c.cancel(true, Canceled) }
}
```

- [`context.newCancelCtx`](https://draveness.me/golang/tree/context.newCancelCtx) 将传入的上下文包装成私有结构体 [`context.cancelCtx`](https://draveness.me/golang/tree/context.cancelCtx)；
- [`context.propagateCancel`](https://draveness.me/golang/tree/context.propagateCancel) 会构建父子上下文之间的关联，当父上下文被取消时，子上下文也会被取消：

```go
func propagateCancel(parent Context, child canceler) {
	done := parent.Done()
	if done == nil {
		return // 父上下文不会触发取消信号
	}
	select {
	case <-done:
		child.cancel(false, parent.Err()) // 父上下文已经被取消
		return
	default:
	}

	if p, ok := parentCancelCtx(parent); ok {
		p.mu.Lock()
		if p.err != nil {
			child.cancel(false, p.err)
		} else {
			p.children[child] = struct{}{}
		}
		p.mu.Unlock()
	} else {
		go func() {
			select {
			case <-parent.Done():
				child.cancel(false, parent.Err())
			case <-child.Done():
			}
		}()
	}
}
```

[`context.cancelCtx`](https://draveness.me/golang/tree/context.cancelCtx) 实现的几个接口方法也没有太多值得分析的地方，该结构体最重要的方法是 [`context.cancelCtx.cancel`](https://draveness.me/golang/tree/context.cancelCtx.cancel)，该方法会关闭上下文中的 Channel 并向所有的子上下文同步取消信号：

```go
func (c *cancelCtx) cancel(removeFromParent bool, err error) {
	c.mu.Lock()
	if c.err != nil {
		c.mu.Unlock()
		return
	}
	c.err = err
	if c.done == nil {
		c.done = closedchan
	} else {
		close(c.done)
	}
	for child := range c.children {
		child.cancel(false, err)
	}
	c.children = nil
	c.mu.Unlock()

	if removeFromParent {
		removeChild(c.Context, c)
	}
}
```

除了 [`context.WithCancel`](https://draveness.me/golang/tree/context.WithCancel) 之外，[`context`](https://github.com/golang/go/tree/master/src/context) 包中的另外两个函数 [`context.WithDeadline`](https://draveness.me/golang/tree/context.WithDeadline) 和 [`context.WithTimeout`](https://draveness.me/golang/tree/context.WithTimeout) 也都能创建可以被取消的计时器上下文 [`context.timerCtx`](https://draveness.me/golang/tree/context.timerCtx)：

```go
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc) {
	return WithDeadline(parent, time.Now().Add(timeout))
}

func WithDeadline(parent Context, d time.Time) (Context, CancelFunc) {
	if cur, ok := parent.Deadline(); ok && cur.Before(d) {
		return WithCancel(parent)
	}
	c := &timerCtx{
		cancelCtx: newCancelCtx(parent),
		deadline:  d,
	}
	propagateCancel(parent, c)
	dur := time.Until(d)
	if dur <= 0 {
		c.cancel(true, DeadlineExceeded) // 已经过了截止日期
		return c, func() { c.cancel(false, Canceled) }
	}
	c.mu.Lock()
	defer c.mu.Unlock()
	if c.err == nil {
		c.timer = time.AfterFunc(dur, func() {
			c.cancel(true, DeadlineExceeded)
		})
	}
	return c, func() { c.cancel(true, Canceled) }
}
```

[`context.WithDeadline`](https://draveness.me/golang/tree/context.WithDeadline) 在创建 [`context.timerCtx`](https://draveness.me/golang/tree/context.timerCtx) 的过程中判断了父上下文的截止日期与当前日期，并通过 [`time.AfterFunc`](https://draveness.me/golang/tree/time.AfterFunc) 创建定时器，当时间超过了截止日期后会调用 [`context.timerCtx.cancel`](https://draveness.me/golang/tree/context.timerCtx.cancel) 同步取消信号。

[`context.timerCtx`](https://draveness.me/golang/tree/context.timerCtx) 内部不仅通过嵌入 [`context.cancelCtx`](https://draveness.me/golang/tree/context.cancelCtx) 结构体继承了相关的变量和方法，还通过持有的定时器 `timer` 和截止时间 `deadline` 实现了定时取消的功能：

```go
type timerCtx struct {
	cancelCtx
	timer *time.Timer // Under cancelCtx.mu.

	deadline time.Time
}

func (c *timerCtx) Deadline() (deadline time.Time, ok bool) {
	return c.deadline, true
}

func (c *timerCtx) cancel(removeFromParent bool, err error) {
	c.cancelCtx.cancel(false, err)
	if removeFromParent {
		removeChild(c.cancelCtx.Context, c)
	}
	c.mu.Lock()
	if c.timer != nil {
		c.timer.Stop()
		c.timer = nil
	}
	c.mu.Unlock()
}
```

[`context.timerCtx.cancel`](https://draveness.me/golang/tree/context.timerCtx.cancel) 方法不仅调用了 [`context.cancelCtx.cancel`](https://draveness.me/golang/tree/context.cancelCtx.cancel)，还会停止持有的定时器减少不必要的资源浪费。

##  传值方法

，[`context`](https://github.com/golang/go/tree/master/src/context) 包中的 [`context.WithValue`](https://draveness.me/golang/tree/context.WithValue) 能从父上下文中创建一个子上下文，传值的子上下文使用 [`context.valueCtx`](https://draveness.me/golang/tree/context.valueCtx) 类型：

```go
func WithValue(parent Context, key, val interface{}) Context {
	if key == nil {
		panic("nil key")
	}
	if !reflectlite.TypeOf(key).Comparable() {
		panic("key is not comparable")
	}
	return &valueCtx{parent, key, val}
}
```

Go

[`context.valueCtx`](https://draveness.me/golang/tree/context.valueCtx) 结构体会将除了 `Value` 之外的 `Err`、`Deadline` 等方法代理到父上下文中，它只会响应 [`context.valueCtx.Value`](https://draveness.me/golang/tree/context.valueCtx.Value) 方法，该方法的实现也很简单：

```go
type valueCtx struct {
	Context
	key, val interface{}
}

func (c *valueCtx) Value(key interface{}) interface{} {
	if c.key == key {
		return c.val
	}
	return c.Context.Value(key)
}
```

如果 [`context.valueCtx`](https://draveness.me/golang/tree/context.valueCtx) 中存储的键值对与 [`context.valueCtx.Value`](https://draveness.me/golang/tree/context.valueCtx.Value) 方法中传入的参数不匹配，就会从父上下文中查找该键对应的值直到某个父上下文中返回 `nil` 或者查找到对应的值。



<div STYLE="page-break-after: always;"></div>

# 系统监控

很多系统中都有守护进程，它们能够在后台监控系统的运行状态，在出现意外情况时及时响应。系统监控是 Go 语言运行时的重要组成部分，它会每隔一段时间检查 Go 语言运行时，确保程序没有进入异常状态。

Go 语言的系统监控也起到了很重要的作用，它在内部启动了一个不会中止的循环，在循环的内部会轮询网络、抢占长期运行或者处于系统调用的 Goroutine 以及触发垃圾回收，通过这些行为，它能够让系统的运行状态变得更健康。

## 监控循环

当 Go 语言程序启动时，运行时会在第一个 Goroutine 中调用 [`runtime.main`](https://draveness.me/golang/tree/runtime.main) 启动主程序，该函数会在系统栈中创建新的线程：

```go
func main() {
	...
	if GOARCH != "wasm" {
		systemstack(func() {
			newm(sysmon, nil)
		})
	}
	...
}
```

[`runtime.newm`](https://draveness.me/golang/tree/runtime.newm) 会创建一个存储待执行函数和处理器的新结构体 [`runtime.m`](https://draveness.me/golang/tree/runtime.m)。运行时执行系统监控不需要处理器，系统监控的 Goroutine 会直接在创建的线程上运行：

```go
func newm(fn func(), _p_ *p) {
	mp := allocm(_p_, fn)
	mp.nextp.set(_p_)
	mp.sigmask = initSigmask
	...
	newm1(mp)
}
```

[`runtime.newm1`](https://draveness.me/golang/tree/runtime.newm1) 会调用特定平台的 [`runtime.newosproc`](https://draveness.me/golang/tree/runtime.newosproc) 通过系统调用 `clone` 创建一个新的线程并在新的线程中执行 [`runtime.mstart`](https://draveness.me/golang/tree/runtime.mstart)：

```go
func newosproc(mp *m) {
	stk := unsafe.Pointer(mp.g0.stack.hi)
	var oset sigset
	sigprocmask(_SIG_SETMASK, &sigset_all, &oset)
	ret := clone(cloneFlags, stk, unsafe.Pointer(mp), unsafe.Pointer(mp.g0), unsafe.Pointer(funcPC(mstart)))
	sigprocmask(_SIG_SETMASK, &oset, nil)
	...
}
```

在新创建的线程中，我们会执行存储在 [`runtime.m`](https://draveness.me/golang/tree/runtime.m) 中的 [`runtime.sysmon`](https://draveness.me/golang/tree/runtime.sysmon) 启动系统监控：

```go
func sysmon() {
	sched.nmsys++
	checkdead()

	lasttrace := int64(0)
	idle := 0
	delay := uint32(0)
	for {
		if idle == 0 {
			delay = 20
		} else if idle > 50 {
			delay *= 2
		}
		if delay > 10*1000 {
			delay = 10 * 1000
		}
		usleep(delay)
		...
	}
}
```

当运行时刚刚调用上述函数时，会先通过 [`runtime.checkdead`](https://draveness.me/golang/tree/runtime.checkdead) 检查是否存在死锁，然后进入核心的监控循环；系统监控在每次循环开始时都会通过 `usleep` 挂起当前线程，该函数的参数是微秒，运行时会遵循以下的规则决定休眠时间：

- 初始的休眠时间是 20μs；
- 最长的休眠时间是 10ms；
- 当系统监控在 50 个循环中都没有唤醒 Goroutine 时，休眠时间在每个循环都会倍增；

当程序趋于稳定之后，系统监控的触发时间就会稳定在 10ms。它除了会检查死锁之外，还会在循环中完成以下的工作：

- 运行计时器 — 获取下一个需要被触发的计时器；
- 轮询网络 — 获取需要处理的到期文件描述符；
- 抢占处理器 — 抢占运行时间较长的或者处于系统调用的 Goroutine；
- 垃圾回收 — 在满足条件时触发垃圾收集回收内存；

我们在这一节中会依次介绍系统监控是如何完成上述几种不同工作的。

### 检查死锁

系统监控通过 [`runtime.checkdead`](https://draveness.me/golang/tree/runtime.checkdead) 检查运行时是否发生了死锁，我们可以将检查死锁的过程分成以下三个步骤：

1. 检查是否存在正在运行的线程；
2. 检查是否存在正在运行的 Goroutine；
3. 检查处理器上是否存在计时器；

该函数首先会检查 Go 语言运行时中正在运行的线程数量，我们通过调度器中的多个字段计算该值的结果：

```go
func checkdead() {
	var run0 int32
	run := mcount() - sched.nmidle - sched.nmidlelocked - sched.nmsys
	if run > run0 {
		return
	}
	if run < 0 {
		print("runtime: checkdead: nmidle=", sched.nmidle, " nmidlelocked=", sched.nmidlelocked, " mcount=", mcount(), " nmsys=", sched.nmsys, "\n")
		throw("checkdead: inconsistent counts")
	}
	...
}
```

1. [`runtime.mcount`](https://draveness.me/golang/tree/runtime.mcount) 根据下一个待创建的线程 id 和释放的线程数得到系统中存在的线程数；
2. `nmidle` 是处于空闲状态的线程数量；
3. `nmidlelocked` 是处于锁定状态的线程数量；
4. `nmsys` 是处于系统调用的线程数量；

利用上述几个线程相关数据，我们可以得到正在运行的线程数，如果线程数量大于 0，说明当前程序不存在死锁；如果线程数小于 0，说明当前程序的状态不一致；如果线程数等于 0，我们需要进一步检查程序的运行状态：

```go
func checkdead() {
	...
	grunning := 0
	for i := 0; i < len(allgs); i++ {
		gp := allgs[i]
		if isSystemGoroutine(gp, false) {
			continue
		}
		s := readgstatus(gp)
		switch s &^ _Gscan {
		case _Gwaiting, _Gpreempted:
			grunning++
		case _Grunnable, _Grunning, _Gsyscall:
			print("runtime: checkdead: find g ", gp.goid, " in status ", s, "\n")
			throw("checkdead: runnable g")
		}
	}
	unlock(&allglock)
	if grunning == 0 {
		throw("no goroutines (main called runtime.Goexit) - deadlock!")
	}
	...
}
```

1. 当存在 Goroutine 处于 `_Grunnable`、`_Grunning` 和 `_Gsyscall` 状态时，意味着程序发生了死锁；
2. 当所有的 Goroutine 都处于 `_Gidle`、`_Gdead` 和 `_Gcopystack` 状态时，意味着主程序调用了 [`runtime.goexit`](https://draveness.me/golang/tree/runtime.goexit)；

当运行时存在等待的 Goroutine 并且不存在正在运行的 Goroutine 时，我们会检查处理器中存在的计时器[：

```go
func checkdead() {
	...
	for _, _p_ := range allp {
		if len(_p_.timers) > 0 {
			return
		}
	}

	throw("all goroutines are asleep - deadlock!")
}
```

如果处理器中存在等待的计时器，那么所有的 Goroutine 陷入休眠状态是合理的，不过如果不存在等待的计时器，运行时会直接报错并退出程序。

### 运行计时器

系统监控的循环中，我们通过 [`runtime.nanotime`](https://draveness.me/golang/tree/runtime.nanotime) 和 [`runtime.timeSleepUntil`](https://draveness.me/golang/tree/runtime.timeSleepUntil) 获取当前时间和计时器下一次需要唤醒的时间；当前调度器需要执行垃圾回收或者所有处理器都处于闲置状态时，如果没有需要触发的计时器，那么系统监控可以暂时陷入休眠：

```go
func sysmon() {
	...
	for {
		...
		now := nanotime()
		next, _ := timeSleepUntil()
		if debug.schedtrace <= 0 && (sched.gcwaiting != 0 || atomic.Load(&sched.npidle) == uint32(gomaxprocs)) {
			lock(&sched.lock)
			if atomic.Load(&sched.gcwaiting) != 0 || atomic.Load(&sched.npidle) == uint32(gomaxprocs) {
				if next > now {
					atomic.Store(&sched.sysmonwait, 1)
					unlock(&sched.lock)
					sleep := forcegcperiod / 2
					if next-now < sleep {
						sleep = next - now
					}
					...
					notetsleep(&sched.sysmonnote, sleep)
					...
					now = nanotime()
					next, _ = timeSleepUntil()
					lock(&sched.lock)
					atomic.Store(&sched.sysmonwait, 0)
					noteclear(&sched.sysmonnote)
				}
				idle = 0
				delay = 20
			}
			unlock(&sched.lock)
		}
		...
		if next < now {
			startm(nil, false)
		}
	}
}
```

休眠的时间会依据强制 GC 的周期 `forcegcperiod` 和计时器下次触发的时间确定，[`runtime.notesleep`](https://draveness.me/golang/tree/runtime.notesleep) 会使用信号量同步系统监控即将进入休眠的状态。当系统监控被唤醒之后，我们会重新计算当前时间和下一个计时器需要触发的时间、调用 [`runtime.noteclear`](https://draveness.me/golang/tree/runtime.noteclear) 通知系统监控被唤醒并重置休眠的间隔。

如果在这之后，我们发现下一个计时器需要触发的时间小于当前时间，这也说明所有的线程可能正在忙于运行 Goroutine，系统监控会启动新的线程来触发计时器，避免计时器的到期时间有较大的偏差。

### 轮询网络 [#](https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-sysmon/#轮询网络)

如果上一次轮询网络已经过去了 10ms，那么系统监控还会在循环中轮询网络，检查是否有待执行的文件描述符：

```go
func sysmon() {
	...
	for {
		...
		lastpoll := int64(atomic.Load64(&sched.lastpoll))
		if netpollinited() && lastpoll != 0 && lastpoll+10*1000*1000 < now {
			atomic.Cas64(&sched.lastpoll, uint64(lastpoll), uint64(now))
			list := netpoll(0)
			if !list.empty() {
				incidlelocked(-1)
				injectglist(&list)
				incidlelocked(1)
			}
		}
		...
	}
}
```

上述函数会非阻塞地调用 [`runtime.netpoll`](https://draveness.me/golang/tree/runtime.netpoll) 检查待执行的文件描述符并通过 [`runtime.injectglist`](https://draveness.me/golang/tree/runtime.injectglist) 将所有处于就绪状态的 Goroutine 加入全局运行队列中：

```go
func injectglist(glist *gList) {
	if glist.empty() {
		return
	}
	lock(&sched.lock)
	var n int
	for n = 0; !glist.empty(); n++ {
		gp := glist.pop()
		casgstatus(gp, _Gwaiting, _Grunnable)
		globrunqput(gp)
	}
	unlock(&sched.lock)
	for ; n != 0 && sched.npidle != 0; n-- {
		startm(nil, false)
	}
	*glist = gList{}
}
```

该函数会将所有 Goroutine 的状态从 `_Gwaiting` 切换至 `_Grunnable` 并加入全局运行队列等待运行，如果当前程序中存在空闲的处理器，会通过 [`runtime.startm`](https://draveness.me/golang/tree/runtime.startm) 启动线程来执行这些任务。

### 抢占处理器 [#](https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-sysmon/#抢占处理器)

系统监控会在循环中调用 [`runtime.retake`](https://draveness.me/golang/tree/runtime.retake) 抢占处于运行或者系统调用中的处理器，该函数会遍历运行时的全局处理器，每个处理器都存储了一个 [`runtime.sysmontick`](https://draveness.me/golang/tree/runtime.sysmontick)：

```go
type sysmontick struct {
	schedtick   uint32
	schedwhen   int64
	syscalltick uint32
	syscallwhen int64
}
```

该结构体中的四个字段分别存储了处理器的调度次数、处理器上次调度时间、系统调用的次数以及系统调用的时间。[`runtime.retake`](https://draveness.me/golang/tree/runtime.retake) 的循环包含了两种不同的抢占逻辑：

```go
func retake(now int64) uint32 {
	n := 0
	for i := 0; i < len(allp); i++ {
		_p_ := allp[i]
		pd := &_p_.sysmontick
		s := _p_.status
		if s == _Prunning || s == _Psyscall {
			t := int64(_p_.schedtick)
			if pd.schedwhen+forcePreemptNS <= now {
				preemptone(_p_)
			}
		}

		if s == _Psyscall {
			if runqempty(_p_) && atomic.Load(&sched.nmspinning)+atomic.Load(&sched.npidle) > 0 && pd.syscallwhen+10*1000*1000 > now {
				continue
			}
			if atomic.Cas(&_p_.status, s, _Pidle) {
				n++
				_p_.syscalltick++
				handoffp(_p_)
			}
		}
	}
	return uint32(n)
}
```

1. 当处理器处于 `_Prunning` 或者 `_Psyscall` 状态时，如果上一次触发调度的时间已经过去了 10ms，我们会通过 [`runtime.preemptone`](https://draveness.me/golang/tree/runtime.preemptone) 抢占当前处理器；
2. 当处理器处于_Psyscall状态时，在满足以下两种情况下会调用`runtime.handoffp`,让出处理器的使用权：
   1. 当处理器的运行队列不为空或者不存在空闲处理器时；
   2. 当系统调用时间超过了 10ms 时；

系统监控通过在循环中抢占处理器来避免同一个 Goroutine 占用线程太长时间造成饥饿问题。

### 垃圾回收

在最后，系统监控还会决定是否需要触发强制垃圾回收，[`runtime.sysmon`](https://draveness.me/golang/tree/runtime.sysmon) 会构建 [`runtime.gcTrigger`](https://draveness.me/golang/tree/runtime.gcTrigger) 并调用 [`runtime.gcTrigger.test`](https://draveness.me/golang/tree/runtime.gcTrigger.test) 方法判断是否需要触发垃圾回收：

```go
func sysmon() {
	...
	for {
		...
		if t := (gcTrigger{kind: gcTriggerTime, now: now}); t.test() && atomic.Load(&forcegc.idle) != 0 {
			lock(&forcegc.lock)
			forcegc.idle = 0
			var list gList
			list.push(forcegc.g)
			injectglist(&list)
			unlock(&forcegc.lock)
		}
		...
	}
}
```

如果需要触发垃圾回收，我们会将用于垃圾回收的 Goroutine 加入全局队列，让调度器选择合适的处理器去执行。

# 代码生成

> 可以把实现的的步骤放在别处
>
> 或者执行一些generate 代码

Go 语言的代码生成机制会读取包含预编译指令的注释，然后执行注释中的命令读取包中的文件，它们将文件解析成抽象语法树并根据语法树生成新的 Go 语言代码和文件，生成的代码会在项目的编译期间与其他代码一起编译和运行。

```go
//go:generate command argument...
```

`go generate` 不会被 `go build` 等命令自动执行，该命令需要显式的触发，手动执行该命令时会在文件中扫描上述形式的注释并执行后面的执行命令

```go
package main

import "fmt"

//go:generate echo hello
//go:generate go run main.go
//go:generate  echo file=$GOFILE pkg=$GOPACKAGE
func main() {
    fmt.Println("main func")
}

// go generate
// hello
// main func
// file=main.go pkg=main
```

我们可以用go generate来实现String()

```go
//go:generate stringer -type=Pill
package painkiller

type Pill int

const (
    Placebo Pill = iota
    Aspirin
    Ibuprofen
    Paracetamol
    Acetaminophen = Paracetamol
)
```

**在运行"go generate"命令前，我们需要安装stringer工具**，命令如下：



```shell
$ go get golang.org/x/tools/cmd/stringer
```

- 然后，在painkiller.go所在的目录下面运行"go generate"命令：



```shell
$ go generate
```

我们会发现当前目录下面生成一个pill_string.go文件，里面实现了我们需要的String()方法，文件内容如下：



```go
// Code generated by "stringer -type=Pill"; DO NOT EDIT.

package painkiller

import "fmt"

const _Pill_name = "PlaceboAspirinIbuprofenParacetamol"

var _Pill_index = [...]uint8{0, 7, 14, 23, 34}

func (i Pill) String() string {
    if i < 0 || i >= Pill(len(_Pill_index)-1) {
        return fmt.Sprintf("Pill(%d)", i)
    }
    return _Pill_name[_Pill_index[i]:_Pill_index[i+1]]
}
```

<div STYLE="page-break-after: always;"></div>
