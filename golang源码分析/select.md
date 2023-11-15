<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [用法](#%E7%94%A8%E6%B3%95)
  - [无 case，永久阻塞](#%E6%97%A0-case%E6%B0%B8%E4%B9%85%E9%98%BB%E5%A1%9E)
  - [非阻塞式](#%E9%9D%9E%E9%98%BB%E5%A1%9E%E5%BC%8F)
  - [随机执行](#%E9%9A%8F%E6%9C%BA%E6%89%A7%E8%A1%8C)
- [case 数据结构](#case-%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

I/O 多路复用被用来处理同一个事件循环中的多个 I/O 事件。I/O 多路复用需要使用特定的系统调用，最常见的系统调用是 `select`，该函数可以同时监听最多 1024 个文件描述符的可读或者可写状态：

**几大特点**

- 可以实现两种收发操作，阻塞收发和非阻塞收发
- 当多个case ready的情况下会随机选择一个执行，不是顺序执行
- 没有ready的case时，有default语句，执行default语句;没有default语句，阻塞直到某个case ready
- select中的case必须是channel操作
- default语句总是可运行的

# 用法

## 无 case，永久阻塞

```go
select{}
```

## 非阻塞式

```go
func main() {
	ch := make(chan int)
	select {
	case i := <-ch:
		println(i)
	default:
		println("default")
	}
}
//default
```

## 随机执行

```go
package main

import "time"

func main() {
	ch := make(chan int)

	go func() {
		for range time.Tick(1 * time.Second) {
			ch <- 0
		}
	}()

	for {
		select {
		case <-ch:
			println("case1")
		case <-ch:
			println("case2")
		}
	}
}
```

```bash
case1
case2
case1
...
```

# case 数据结构

```go
type scase struct {
	c           *hchan         // chan
	elem        unsafe.Pointer // data element
	kind        uint16
	pc          uintptr // race pc (for race detector / msan)
	releasetime int64
}
```

