<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
- [具名返回](#%E5%85%B7%E5%90%8D%E8%BF%94%E5%9B%9E)
- [函数返回过程](#%E5%87%BD%E6%95%B0%E8%BF%94%E5%9B%9E%E8%BF%87%E7%A8%8B)
- [defer总结](#defer%E6%80%BB%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

defer类似于栈结构，多个defer的执行顺序为`后进先出LIFO`

# 数据结构

定义文件：`src/runtime/runtime2.go`

```Go
type _defer struct {
        heap      bool
        rangefunc bool    // true for rangefunc list
        sp        uintptr // sp at time of defer
        pc        uintptr // pc at time of defer
        fn        func()  // can be nil for open-coded defers
        link      *_defer // next defer on G; can point to either heap or stack!

        // If rangefunc is true, *head is the head of the atomic linked list
        // during a range-over-func execution.
        head *atomic.Pointer[_defer]
}
```

- sp 函数栈指针
- pc 程序计数器
- fn 函数地址
- link 指向自身结构的指针，用于链接多个 defer

# 具名返回

也就是说是闭包或者匿名函数，并且调用 defer的函数时一个有名返回值(named result parameters)的函数，那么 defer 可以直接访问有名返回值并进行修改的。

# 函数返回过程

设置返回值—>执行 defer—>ret。

# defer总结

- derfer保存的当前副本，注意指针，或者地址变动对值的影响
- 申请资源后立即使用defer关闭资源是好习惯