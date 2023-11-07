<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [文档注释](#%E6%96%87%E6%A1%A3%E6%B3%A8%E9%87%8A)
- [内存模型](#%E5%86%85%E5%AD%98%E6%A8%A1%E5%9E%8B)
  - [之前版本](#%E4%B9%8B%E5%89%8D%E7%89%88%E6%9C%AC)
  - [对软内存限制](#%E5%AF%B9%E8%BD%AF%E5%86%85%E5%AD%98%E9%99%90%E5%88%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

2022 年 8 月 2 日

Go 1.19 的泛型开发重点放在解决社区向我们报告的微妙问题和极端情况，以及重要的性能改进（某些泛型程序高达 20%）。

文档注释现在支持链接、列表和更清晰的标题语法。 此更改可帮助用户编写更清晰、更易于导航的文档注释，尤其是在具有大型 API 的包中。

Go 的内存模型现在明确定义了sync/atomic 包的行为。 发生之前关系的正式定义已被修改，以与 C、C++、Java、JavaScript、Rust 和 Swift 使用的内存模型保持一致。 现有程序不受影响。 随着内存模型的更新，sync/atomic 包中出现了新类型，例如atomic.Int64 和atomic.Pointer[T]，以便更轻松地使用原子值。

垃圾收集器添加了对软内存限制的支持，新的垃圾收集指南中详细讨论了这一点。 该限制对于优化 Go 程序特别有帮助，以便在具有专用内存量的容器中尽可能高效地运行。

出于安全原因，os/exec 包不再考虑 PATH 查找中的相对路径。 有关详细信息，请参阅包文档。

最后，Go 1.19 包括各种性能和实现改进，包括动态调整初始 Goroutine 堆栈的大小以减少堆栈复制、在大多数 Unix 系统上自动使用附加文件描述符、x86-64 和 ARM64 上大型 switch 语句的跳转表、 支持ARM64上的调试器注入函数调用，RISC-V上的寄存器ABI支持，以及对在龙芯64位架构LoongArch（GOARCH=loong64）上运行的Linux的实验性支持。

# 文档注释

用得时候查

# 内存模型

https://go.dev/ref/mem

新版内存模型定义：

Go的总体方法在C/C++和Java/Js之间，既不像C/C++那样将存在Data race的程序定义为违法的，让编译器以未定义行为处置它，即运行时表现出任意可能的行为；又不完全像Java/Js那样尽量明确Data Race情况下各种语义，将Data race带来的影响限制在最小，使程序更为可靠。

Go对于一些存在data Race的情况会输出race报告并终止程序，比如多goroutine在未使用同步手段下对map的并发读写。除此之外，Go对其他存数据竞争的场景有明确的语义，这让程序更可靠，也更容易调试。

**atomic.Int64 **

```go
// Go 1.18

var i uint64
atomic.AddUint64(&i, 1)
_ = atomic.LoadUint64(&i)

vs.

// Go 1.19
var i atomic.Uint64 // 默认值为0
i.Store(17) // 也可以通过Store设置初始值
i.Add(1)
_ = i.Load()
```

**atomic.Pointer**

```go
// $GOROOT/src/sync/atomic/type.go

// A Pointer is an atomic pointer of type *T. The zero value is a nil *T.
type Pointer[T any] struct {
    _ noCopy
    v unsafe.Pointer
}

// Load atomically loads and returns the value stored in x.
func (x *Pointer[T]) Load() *T { return (*T)(LoadPointer(&x.v)) }

// Store atomically stores val into x.
func (x *Pointer[T]) Store(val *T) { StorePointer(&x.v, unsafe.Pointer(val)) }

// Swap atomically stores new into x and returns the previous value.
func (x *Pointer[T]) Swap(new *T) (old *T) { return (*T)(SwapPointer(&x.v, unsafe.Pointer(new))) }

// CompareAndSwap executes the compare-and-swap operation for x.
func (x *Pointer[T]) CompareAndSwap(old, new *T) (swapped bool) {
    return CompareAndSwapPointer(&x.v, unsafe.Pointer(old), unsafe.Pointer(new))
}
```

## 之前版本

多处理器计算机在共享内存的情况下并发程序正确运行的条件，即多处理器要满足**顺序一致性**

内存模型指的是内存一致性模型。

- Go 内存模型指定了在一个 Goroutine 中读取变量时可以保证观察到在不同 Goroutine 中写入同一变量所产生的值的条件。

- 在一个 goroutine 中，读操作和写操作必须表现地就好像它们是按照程序中指定的顺序执行的。

`Happens Before`原则的定义是如果一个操作e1先于操作e2发生，那么我们就说e1 `happens before` e2，也可以描述成e2 `happens after` e2，此时e1操作的变量结果对e2都是可见的。如果e1操作既不先于e2发生又不晚于e2发生，我们说e1操作与e2操作并发发生。

Happens Before具有传导性：如果操作e1 `happens before` 操作e2，e3 `happends before` e1，那么e3一定也 `happends before` e2。

```bash
如果一个包p导入了包q，那么q的init函数的完成发生在p的任何函数的开始之前。
函数main.main的开始发生在所有init函数完成之后。
启动一个新的goroutine的go语句发生在goroutine的执行开始之前。
一个channel上的发送操作发生在该channel的对应接收操作完成之前。
一个channel的关闭发生在一个返回零值的接收之前(因为该channel已经关闭)。
一个无缓冲的channel的接收发生在该channel的发送操作完成之前。
一个容量为C的channel上的第k个接收操作发生在该channel第k+C个发送操作完成之前。
对于任何sync.Mutex或sync.RWMutex变量l，当n&lt;m时，第n次l.Unlock调用发生在第m次调用l.Lock()返回之前。
once.Do(f)中的f()调用发生在对once.Do(f)的任何一次调用返回之前。
```

## 对软内存限制

https://go.dev/doc/gc-guide#Memory_limit

Go 1.19版本之前，Go GC的调优参数仅有一个：**GOGC**(也可以通过runtime/debug.SetGCPercent调整)。

GOGC默认值为100，通过调整它的值，我们可以调整GC触发的时机。计算下一次触发GC的堆内存size的公式如下：

```go
// Go 1.18版本之前
目标堆大小 = (1+GOGC/100) * live heap // live heap为上一次GC标记后的堆上的live object的总size

// Go 1.18版本及之后
目标堆大小 = live heap + (live heap + GC roots) * GOGC / 100
```

不过pacer目前的算法是无法保证你的应用不被`OOM killed`,**而是Pacer算法导致了没能及时触发GC**。



**引入Soft memory limit **(防止oom)

这个方案在runtime/debug包中添加了一个名为SetMemoryLimit的函数以及GOMEMLIMIT环境变量，通过他们任意一个都可以设定Go应用的Memory limit。

一旦设定了Memory limit，当Go堆大小达到“Memory limit减去非堆内存后的值”时，一轮GC会被触发。即便你手动关闭了GC(GOGC=off)，GC亦是会被触发。

soft memory limit不保证不会出现oom-killed，这个也很好理解。如果live heap object到达limit了，说明你的应用内存资源真的不够了，是时候扩内存条资源了，这个是GC无论如何都无法解决的问题。

那么多大的值是合理的soft memory limit值呢？在Go服务独占容器资源时，一个好的经验法则是留下额外的5-10%的空间，以考虑Go运行时不知道的内存来源。uber在其博客中设定的limit为资源上限的70%，也是一个不错的经验值。