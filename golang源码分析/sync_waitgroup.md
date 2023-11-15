<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [WaitGroup 的使用](#waitgroup-%E7%9A%84%E4%BD%BF%E7%94%A8)
- [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
- [Add](#add)
- [Done](#done)
- [Wait](#wait)
- [总结](#%E6%80%BB%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

`sync.WaitGroup` 是 Golang 中常用的并发措施，我们可以用它来等待一批 Goroutine 结束。

# WaitGroup 的使用

```Go
func main() {
        var wg sync.WaitGroup
        for i := 0; i < 5; i++ {
                wg.Add(1)
                go func() {
                        defer wg.Done()
                        println("hello")
                }()
        }

        wg.Wait()
}
```

# 数据结构

```Go
type WaitGroup struct {
        noCopy noCopy

        state atomic.Uint64 // high 32 bits are counter, low 32 bits are waiter count.
        sema  uint32
}
```

- state 是 WaitGroup 的核心，它是一个无符号的 64 位整型，基于atomic.Uint64 它使用了 CompareAndSwap(CAS) 操作，而这个操作依赖于 CPU 提供的原子性指令，是 CPU 级的原子操作
- sema 信号量

# Add

```Go
func (wg *WaitGroup) Add(delta int) {
    // 获取到wg.state1数组中元素组成的二进制对应的十进制的值
        statep := wg.state()
        // 高32位是计数器
        state := atomic.AddUint64(statep, uint64(delta)<<32)
        // 获取计数器
        v := int32(state >> 32)
        w := uint32(state)
        // 计数器为负数，报panic
        if v < 0 {
                panic("sync: negative WaitGroup counter")
        }
        // 添加与等待并发调用，报panic
        if w != 0 && delta > 0 && v == int32(delta) {
                panic("sync: WaitGroup misuse: Add called concurrently with Wait")
        }
        // 计数器添加成功
        if v > 0 || w == 0 {
                return
        }

        // 当等待计数器> 0时，而goroutine设置为0。
        // 此时不可能有同时发生的状态突变:
        // - 增加不能与等待同时发生，
        // - 如果计数器counter == 0，不再增加等待计数器
        if *statep != state {
                panic("sync: WaitGroup misuse: Add called concurrently with Wait")
        }
        // Reset waiters count to 0.
        *statep = 0
        for ; w != 0; w-- {
                // 目的是作为一个简单的wakeup原语，以供同步使用。true为唤醒排在等待队列的第一个goroutine
                runtime_Semrelease(&wg.sema, false)
        }
}
```

# Done

```Go
// Done decrements the WaitGroup counter by one.
func (wg *WaitGroup) Done() {
        wg.Add(-1)
}
```

# Wait

```Go
func (wg *WaitGroup) Wait() {
        // 获取到wg.state1数组中元素组成的二进制对应的十进制的值
        statep := wg.state()
        // cas算法
        for {
                state := atomic.LoadUint64(statep)
                // 高32位是计数器
                v := int32(state >> 32)
                w := uint32(state)
                // 计数器为0，结束等待
                if v == 0 {
                        // Counter is 0, no need to wait.
                        return
                }
                // 增加等待goroutine计数，对低32位加1，不需要移位
                if atomic.CompareAndSwapUint64(statep, state, state+1) {
                        // 目的是作为一个简单的sleep原语，以供同步使用
                        runtime_Semacquire(&wg.sema)
                        if *statep != 0 {
                                panic("sync: WaitGroup is reused before previous Wait has returned")
                        }
                        return
                }
        }
}
```

# 总结

- Wait() 后不能使用Add()
- 加减后数量不能小于0
- 禁止拷贝