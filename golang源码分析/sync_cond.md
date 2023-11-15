<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [Cond用法](#cond%E7%94%A8%E6%B3%95)
- [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
- [copyChecker 源码](#copychecker-%E6%BA%90%E7%A0%81)
- [notifyList 源码](#notifylist-%E6%BA%90%E7%A0%81)
- [Wait](#wait)
- [Signal](#signal)
- [Broadcast](#broadcast)
- [Cond 的惯用法及使用注意事项](#cond-%E7%9A%84%E6%83%AF%E7%94%A8%E6%B3%95%E5%8F%8A%E4%BD%BF%E7%94%A8%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

cond 使用场景：单生产者多消费者

# Cond用法

sync.Cond 接口的用法:

- sync.NewCond(l Locker): 新建一个 sync.Cond 变量。注意该函数需要一个 Locker 作为必填参数，这是因为在 cond.Wait() 中底层会涉及到 Locker 的锁操作。
- Wait(): 等待被唤醒。唤醒期间会解锁并切走 goroutine。
- Signal(): 只唤醒一个最先 Wait 的 goroutine。
- Broadcast(): 会将全部 Wait 的 goroutine 都唤醒。区别是 Signal 一次只会唤醒一个 goroutine

```go
package main

import (
        "log"
        "sync"
        "time"
)

var done = false

func read(name string, c *sync.Cond) {
        c.L.Lock()
        for !done {
                c.Wait()
        }
        log.Println(name, "starts reading")
        c.L.Unlock()
}

func write(name string, c *sync.Cond) {
        log.Println(name, "starts writing")
        time.Sleep(time.Second)
        c.L.Lock()
        done = true
        c.L.Unlock()
        log.Println(name, "wakes all")
        c.Broadcast()
}

func main() {
        cond := sync.NewCond(&sync.Mutex{})

        go read("reader1", cond)
        go read("reader2", cond)
        go read("reader3", cond)
        write("writer", cond)

        time.Sleep(time.Second * 3)
}
```

# 数据结构

```go
type Cond struct {
        noCopy noCopy

        // L is held while observing or changing the condition
        L Locker //locker 是mutex

        notify  notifyList
        checker copyChecker
}

// NewCond returns a new Cond with Locker l.
func NewCond(l Locker) *Cond {
        return &Cond{L: l}
}
```

- nocopy ：是在go vet工具检验时，表明 WaitGroup 结构体的具体实现是不可被复制的。
- checker：双重检查(Double check)防止运行时拷贝
- L：可以传入一个读写锁或互斥锁，当修改条件或者调用wait方法时需要加锁
- notify：通知链表，调用wait()方法的Goroutine会放到这个链表中，唤醒从这里取。

# copyChecker 源码

copyChecker 再次判断有没有copy

```go
// copyChecker holds back pointer to itself to detect object copying.
type copyChecker uintptr

func (c *copyChecker) check() {
        if uintptr(*c) != uintptr(unsafe.Pointer(c)) &&
                !atomic.CompareAndSwapUintptr((*uintptr)(c), 0, uintptr(unsafe.Pointer(c))) &&
                uintptr(*c) != uintptr(unsafe.Pointer(c)) {
                panic("sync.Cond is copied")
        }
}
```

# notifyList 源码

将调用wait()方法的Goroutine会放到这个链表中，唤醒从这里取。

```go
type notifyList struct {
    wait   uint32
    notify uint32
    lock   uintptr // key field of the mutex
    head   unsafe.Pointer
    tail   unsafe.Pointer
}
```

wait：下一个等待唤醒Goroutine的索引，他是在锁外自动递增的.

notify：下一个要通知的Goroutine的索引，他可以在锁外读取，但是只能在锁持有的情况下写入.

head：指向链表的头部

tail：指向链表的尾部

# Wait

```Go
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

// runtime.notifyListAdd
//go:linkname notifyListAdd sync.runtime_notifyListAdd
func notifyListAdd(l *notifyList) uint32 {
        // This may be called concurrently, for example, when called from
        // sync.Cond.Wait while holding a RWMutex in read mode.
        return l.wait.Add(1) - 1
}
```

- 执行运行期间拷贝检查，如果发生了拷贝，则直接panic程序
- 调用runtime_notifyListAdd将等待计数器加一并解锁. 获取ticket；
- 调用runtime_notifyListWait等待其他 Goroutine 的唤醒并加锁

# Signal

Signal()唤醒等待队列头部的goroutine

```Go
func (c *Cond) Signal() {
 c.checker.check()
 runtime_notifyListNotifyOne(&c.notify)
}
```

# Broadcast

Broadcast()唤醒等待队列所有的的goroutine

```Go
func (c *Cond) Broadcast() {
 c.checker.check()
 runtime_notifyListNotifyAll(&c.notify)
}
```

# Cond 的惯用法及使用注意事项

- 调用Wait方法之前必须调用cond.L.Lock方法。在Wait方法的实现中，会首先调用Unlock方法，如果之前没有调用Lock方法，会造成panic。
- Wait方法必须在Signal或Broadcast方法之前调用，否则可能会造成死锁。
- cond.Wait应该在一个循环中调用
  - cond.L会在Wait方法中被Unlock从而失去锁，无法确定资源状态是否被改变了，因此在Wait函数返回时无法确定条件是否达成，所以需要在一个循环中调用cond.Wait，当调用结束后再次判断条件是否达成，如果条件未达成，则循环一直执行。

```Go
c.L.Lock()
for !condition() {
     c.Wait()
}
// ... make use of condition ...
c.L.Unlock()
```