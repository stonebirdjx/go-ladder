# [sync](https://pkg.go.dev/sync)

> sync包里的东西禁止复制

## :point_right:type Locker interface 

Locker 代表可以锁定和解锁的对象。

```Go
// A Locker represents an object that can be locked and unlocked.
type Locker interface {
        Lock()
        Unlock()
}
```

- 合理使用锁，`Unlock()`一定要在`Lock()` 建议使用`defer`

## :point_right:type Mutex struct 

```Go
type Mutex struct {
        state int32
        sema  uint32
}
```

- Mutex 是互斥锁。
- 互斥体的零值是未锁定的互斥体。
- 第一次使用后不得复制互斥体。

Func:

```Go
func (*Mutex).Lock()
func (*Mutex).TryLock() bool
func (*Mutex).Unlock()
func (*Mutex).lockSlow()
func (*Mutex).unlockSlow(new int32)
```

Example:

```Go
package main

import (
    "fmt"
    "sync"
)

var counter int
var counterMutex sync.Mutex

func increment() {
    counterMutex.Lock()  // atomic.CompareAndSwapInt32
    defer counterMutex.Unlock() // new := atomic.AddInt32(&m.state, -mutexLocked) 设置state值为0
    counter++
}

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 1000; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            increment()
        }()
    }
    wg.Wait()
    fmt.Println("Final counter value:", counter)
}
```

## :point_right:type RWMutex struct

```Go
type RWMutex struct {
        w           Mutex        // held if there are pending writers
        writerSem   uint32       // semaphore for writers to wait for completing readers
        readerSem   uint32       // semaphore for readers to wait for completing writers
        readerCount atomic.Int32 // number of pending readers
        readerWait  atomic.Int32 // number of departing readers
}
```

- RWMutex 是读/写互斥锁。
- RWMutex 可以由任意数量的读取者或单个写入者持有。
- RWMutex 的零值是未锁定的互斥体。
- RWMutex 在首次使用后不得复制。

Func:

```Go
func (*RWMutex).Lock()
func (*RWMutex).RLock()
func (*RWMutex).RLocker() Locker
func (*RWMutex).RUnlock()
func (*RWMutex).TryLock() bool
func (*RWMutex).TryRLock() bool
func (*RWMutex).Unlock()
func (*RWMutex).rUnlockSlow(r int32)
```

Example:

```Go
package main

import (
        "fmt"
        "sync"
        "time"
)

var (
        data  map[string]string
        mutex sync.RWMutex
)

func main() {
        data = make(map[string]string)

        // 启动多个读取协程
        for i := 0; i < 3; i++ {
                go readData(i)
        }

        // 启动一个写入协程
        go writeData()

        // 等待一段时间，让读写操作执行
        time.Sleep(5 * time.Second)
}

func readData(id int) {
        for {
                mutex.RLock() // 获取读锁
                fmt.Printf("协程 %d 读取数据：%v\n", id, data)
                mutex.RUnlock() // 释放读锁

                time.Sleep(1 * time.Second)
        }
}

func writeData() {
        for {
                mutex.Lock()                                  // 获取写锁
                data["key"] = time.Now().Format(time.RFC3339) // 写入数据
                fmt.Println("写入数据：", data)
                mutex.Unlock() // 释放写锁

                time.Sleep(2 * time.Second)
        }
}
```

## :point_right:type WaitGroup struct 

```Go
type WaitGroup struct {
        noCopy noCopy

        state atomic.Uint64 // high 32 bits are counter, low 32 bits are waiter count.
        sema  uint32
}
```

- WaitGroup 等待 goroutine 集合完成。
- 主goroutine调用Add来设置数量
- 要等待的 goroutine。 然后每个 goroutine
- 运行并在完成时调用 Done。 同时，
- Wait 可用于阻塞，直到所有 goroutine 完成。
- 首次使用后不得复制 WaitGroup。

Func:

```Go
func (*WaitGroup).Add(delta int)
func (*WaitGroup).Done()
func (*WaitGroup).Wait()
```

Example：

```Go
package main

import (
        "fmt"
        "sync"
        "time"
)

func main() {
        var wg sync.WaitGroup

        // 启动多个 goroutine
        for i := 0; i < 5; i++ {
                wg.Add(1) // 增加等待计数

                go func(id int) {
                        defer wg.Done() // 减少等待计数

                        fmt.Printf("协程 %d 开始执行\n", id)
                        time.Sleep(2 * time.Second)
                        fmt.Printf("协程 %d 执行完成\n", id)
                }(i)
        }

        wg.Wait() // 等待所有协程完成

        fmt.Println("所有协程执行完成")
}
```

## :point_right:type Pool struct 

```Go
type Pool struct {
        noCopy noCopy

        local     unsafe.Pointer // local fixed-size per-P pool, actual type is [P]poolLocal
        localSize uintptr        // size of the local array

        victim     unsafe.Pointer // local from previous cycle
        victimSize uintptr        // size of victims array

        // New optionally specifies a function to generate
        // a value when Get would otherwise return nil.
        // It may not be changed concurrently with calls to Get.
        New func() any
}
```

- Pool 是一组可以单独保存和使用的临时对象
- 检索到存储在池中的任何项目都可以随时自动删除，无需通知。 如果发生这种情况时 Pool 持有唯一的引用，则项目可能会被释放。
- 多个 Goroutine 同时使用 Pool 是安全的。
- 池的目的是缓存已分配但未使用的项目以供以后重用，减轻垃圾收集器的压力。 也就是说，它可以很容易地构建高效、线程安全的空闲列表。
- Pool 的适当用途是管理一组临时项在并发独立之间静默共享并可能被并发独立重用包的客户端。 池提供了一种分摊分配开销的方法
- 很好使用 Pool 的例子，它维护了一个动态大小的临时输出缓冲区存储。
- 另一方面，作为短期对象的一部分维护的空闲列表是不适合使用 Pool，因为开销不能很好地摊销
- 首次使用后不得复制池。

> sync.Pool 可以用于存储临时对象，以避免频繁地分配和释放内存，从而减少垃圾回收的负担。

Func:

```Go
func (*Pool).Get() any
func (*Pool).Put(x any)
func (*Pool).getSlow(pid int) any
func (*Pool).pin() (*poolLocal, int)
func (*Pool).pinSlow() (*poolLocal, int)
```

Example：

```Go
package main

import (
        "fmt"
        "sync"
)

type Object struct {
        ID int
}

func main() {
        pool := sync.Pool{
                New: func() interface{} {
                        fmt.Println("Creating new object")
                        return &Object{}
                },
        }

        // 从池中获取对象
        obj1 := pool.Get().(*Object)
        obj1.ID = 1
        fmt.Println("Object 1:", obj1)

        // 将对象放回池中
        pool.Put(obj1)

        // 再次从池中获取对象
        obj2 := pool.Get().(*Object)
        fmt.Println("Object 2:", obj2)
}
```

## :point_right:type Once struct 

```Go
type Once struct {
        // done indicates whether the action has been performed.
        // It is first in the struct because it is used in the hot path.
        // The hot path is inlined at every call site.
        // Placing done first allows more compact instructions on some architectures (amd64/386),
        // and fewer instructions (to calculate offset) on other architectures.
        done uint32
        m    Mutex
}
```

- Once 是一个只执行一个操作的对象。
- 首次使用后不得复制 Once。

Func:

```Go
func (*Once).Do(f func())
func (*Once).doSlow(f func())
```

Example:

```Go
package main

import (
        "fmt"
        "sync"
)

func main() {
        var once sync.Once

        for i := 0; i < 3; i++ {
                go func() {
                        once.Do(func() {
                                fmt.Println("只执行一次的操作")
                        })
                        fmt.Println("其他操作")
                }()
        }

        // 等待一段时间，以便观察输出
        fmt.Scanln()
}
```

## :point_right:type Map struct 

```Go
type Map struct {
        mu Mutex

        // read contains the portion of the map's contents that are safe for
        // concurrent access (with or without mu held).
        //
        // The read field itself is always safe to load, but must only be stored with
        // mu held.
        //
        // Entries stored in read may be updated concurrently without mu, but updating
        // a previously-expunged entry requires that the entry be copied to the dirty
        // map and unexpunged with mu held.
        read atomic.Pointer[readOnly]

        // dirty contains the portion of the map's contents that require mu to be
        // held. To ensure that the dirty map can be promoted to the read map quickly,
        // it also includes all of the non-expunged entries in the read map.
        //
        // Expunged entries are not stored in the dirty map. An expunged entry in the
        // clean map must be unexpunged and added to the dirty map before a new value
        // can be stored to it.
        //
        // If the dirty map is nil, the next write to the map will initialize it by
        // making a shallow copy of the clean map, omitting stale entries.
        dirty map[any]*entry

        // misses counts the number of loads since the read map was last updated that
        // needed to lock mu to determine whether the key was present.
        //
        // Once enough misses have occurred to cover the cost of copying the dirty
        // map, the dirty map will be promoted to the read map (in the unamended
        // state) and the next store to the map will make a new dirty copy.
        misses int
}
```

- Map 类似于 Go map[interface{}]interface{} 但可以安全地并发使用由多个 goroutine 执行，无需额外的锁定或协调。加载、存储和删除在摊余常量时间内运行。
- 读取操作：Load, LoadAndDelete, LoadOrStore, Swap, CompareAndSwap, and CompareAndDelete
- 写入操作: Delete, LoadAndDelete, Store, and Swap

Func:

```Go
func (*Map).CompareAndDelete(key any, old any) (deleted bool) //如果 key 的值等于 old，CompareAndDelete 将删除该 key 的条目。 旧值必须是可比较类型。如果映射中没有 key 的当前值，CompareAndDelete 将返回 false（即使旧值是 nil 接口值）。

func (*Map).CompareAndSwap(key any, old any, new any) bool // 如果存储在映射中的值等于旧值，CompareAndSwap 就会交换键的新旧值。 旧值必须是可比较类型。

func (*Map).Delete(key any)
func (*Map).Load(key any) (value any, ok bool)
func (*Map).LoadAndDelete(key any) (value any, loaded bool)
func (*Map).LoadOrStore(key any, value any) (actual any, loaded bool) //LoadOrStore 返回键的现有值（如果存在）。 否则，它存储并返回给定值。

func (*Map).Range(f func(key any, value any) bool)
func (*Map).Store(key any, value any)
func (*Map).Swap(key any, value any) (previous any, loaded bool) // Swap 将值交换为键，并返回之前的值（如果有）。 loaded 报告密钥是否存在。
func (*Map).dirtyLocked()
func (*Map).loadReadOnly() readOnly
func (*Map).missLocked()
```

Example：

```Go
package main

import (
        "fmt"
        "sync"
)

func main() {
        var m sync.Map

        // 存储键值对
        m.Store("key1", "value1")
        m.Store("key2", "value2")

        // 从映射中加载值
        value1, ok1 := m.Load("key1")
        value2, ok2 := m.Load("key2")
        value3, ok3 := m.Load("key3")

        fmt.Println("Value for key1:", value1, ", Exists:", ok1)
        fmt.Println("Value for key2:", value2, ", Exists:", ok2)
        fmt.Println("Value for key3:", value3, ", Exists:", ok3)

        // 删除键值对
        m.Delete("key2")

        // 遍历映射中的所有键值对
        m.Range(func(key, value interface{}) bool {
                fmt.Println("Key:", key, "Value:", value)
                return true
        })
}
```

## :point_right:type Cond struct 

```Go
type Cond struct {
        noCopy noCopy

        // L is held while observing or changing the condition
        L Locker

        notify  notifyList
        checker copyChecker
}
```

- Cond 实现一个条件变量，一个集合点用于等待或宣布发生的 goroutine一个事件。
- 每个Cond都有一个关联的Locker L（通常是*Mutex或*RWMutex），
- 改变条件时必须保存 当调用Wait方法时。
- 首次使用后不得复制 Cond。

> 一般用于微小广播或信号唤醒

Func:

```Go
func (*Cond).Broadcast() //广播唤醒所有在 c 上等待的 goroutine。允许但不要求呼叫者在通话过程中按住 c.L。
func (*Cond).Signal() // 信号会唤醒一个在 c 上等待的 goroutine（如果有的话）。
func (*Cond).Wait() // Wait 以原子方式解锁 c.L 并暂停调用 goroutine 的执行。 稍后恢复执行后，Wait 在返回之前锁定 c.L。
```

Example：

```Go
package main

import (
        "fmt"
        "sync"
        "time"
)

var sharedRsc = false

func main() {
        var wg sync.WaitGroup
        wg.Add(2)
        m := sync.Mutex{}
        c := sync.NewCond(&m)
        go func() {
                defer wg.Done()
                c.L.Lock()
                defer c.L.Unlock()
                for !sharedRsc {
                        fmt.Println("goroutine1 wait")
                        c.Wait()
                }
                fmt.Println("goroutine1", sharedRsc)
        }()

        go func() {
                defer wg.Done()
                c.L.Lock()
                defer c.L.Unlock()
                for !sharedRsc {
                        fmt.Println("goroutine2 wait")
                        c.Wait()
                }
                fmt.Println("goroutine2", sharedRsc)
        }()

        time.Sleep(2 * time.Second)
        c.L.Lock()
        fmt.Println("main goroutine ready")
        sharedRsc = true
        c.Broadcast()
        fmt.Println("main goroutine broadcast")
        c.L.Unlock()
        wg.Wait()
}
```

## func OnceFunc

```Go
func OnceFunc(f func()) func() {}
```

- OnceFunc 返回一个仅调用 f 一次的函数。 返回的函数可以被并发调用。
- 如果 f 发生panic，则返回的函数将在每次调用时以相同的值发生panic。

Example:

```Go
package main

import (
        "fmt"
        "sync"
)

func exec() {
        fmt.Println("haha")
}

func main() {
        f := sync.OnceFunc(exec)
        f()
        f()
}
```

## func OnceValue

```Go
func OnceValue[T any](f func() T) func() T
```

- OnceValue 返回一个仅调用 f 一次的函数，并返回 f 返回的值。 返回的函数可以被并发调用。
- 如果 f 发生恐慌，则返回的函数将在每次调用时以相同的值发生恐慌。

Example:

```Go
package main

import (
        "fmt"
        "sync"
)

func exec() string {
        return "haha"
}

func main() {
        f := sync.OnceValue[string](exec)
        fmt.Println(f())
        fmt.Println(f())
}
// haha
// haha
```

## func OnceValues

```Go
func OnceValues[T1, T2 any](f func() (T1, T2)) func() (T1, T2)
```

- OnceValue 用法一致

