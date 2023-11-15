<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
- [Do](#do)
- [问题](#%E9%97%AE%E9%A2%98)
  - [为什么不使用乐观锁 CAS](#%E4%B8%BA%E4%BB%80%E4%B9%88%E4%B8%8D%E4%BD%BF%E7%94%A8%E4%B9%90%E8%A7%82%E9%94%81-cas)
  - [为什么读取 done 值的方式没有统一](#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%AF%BB%E5%8F%96-done-%E5%80%BC%E7%9A%84%E6%96%B9%E5%BC%8F%E6%B2%A1%E6%9C%89%E7%BB%9F%E4%B8%80)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

sync.Once 是 Go 标准库提供的使函数只执行一次的实现，常应用于单例模式，例如初始化配置、保持数据库连接等

# 数据结构

```Go
type Once struct {
    done uint32
    m    Mutex  // 初始值为0表示还未执行过，1表示已经执行过
}
```

# Do

```Go
func (o *Once) Do(f func()) {
        if atomic.LoadUint32(&o.done) == 0 {
                // Outlined slow-path to allow inlining of the fast-path.
                o.doSlow(f)
        }
}

func (o *Once) doSlow(f func()) {
        o.m.Lock()
        defer o.m.Unlock()
        if o.done == 0 {
                defer atomic.StoreUint32(&o.done, 1)
                f() // 存在阻塞问题
        }
}
```

需要注意的是执行f函数是同步进行的，也就是说可能存在阻塞问题。

# 问题

## 为什么不使用乐观锁 CAS

简单的来说就是 f() 的执行结果最终可能是不成功的，所以你会看到现在采用的是双重检测锁机制来实现，同时需要等 f() 执行完成才修改 done 值

## 为什么读取 done 值的方式没有统一

为什么有的地方用的是 atomic.LoadUint32，有的地方用的却是 o.done？

主要原因是 atomic.LoadUint32 可以保证原子读取到 done 值，是并发安全的，而在 doSlow 中，已经加锁了，那么临界区就是并发安全的，使用 o.done 就可以来读取值就可以了