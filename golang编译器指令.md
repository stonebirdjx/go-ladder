<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
  - [//go:linkname](#golinkname)
  - [//go:noescape](#gonoescape)
  - [//go:nosplit](#gonosplit)
  - [//go:norace](#gonorace)
  - [//go:noinline](#gonoinline)
  - [//go:notinheap](#gonotinheap)
  - [//go:nowritebarrierrec](#gonowritebarrierrec)
  - [//go:yeswritebarrierrec](#goyeswritebarrierrec)
  - [//go:uintptrkeepalive](#gouintptrkeepalive)
  - [//go:uintptrescapes](#gouintptrescapes)
  - [//go:embed](#goembed)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

编译指令的语法是一行特殊的注释，关键字//go开始(之间没有空格), 在cmd/compilex,很多还未列出如//go:build、Cgo、//line等需要详细看看

https://pkg.go.dev/cmd/compile

##  //go:linkname

该指令指示编译器使用 `importpath.name` 作为源代码中声明为 `localname` 的变量或函数的目标文件符号名称。但是由于这个伪指令，可以破坏类型系统和包模块化。因此只有引用了 unsafe 包才可以使用

**time/time.go**

```javascript
...
func now() (sec int64, nsec int32, mono int64)
```

**runtime/timestub.go**

```javascript
import _ "unsafe" // for go:linkname

//go:linkname time_now time.now
func time_now() (sec int64, nsec int32, mono int64) {
	sec, nsec = walltime()
	return sec, nsec, nanotime() - startNano
}
```

可以看到 `time.now`，它并没有具体的实现。如果你初看可能会懵逼。这时候建议你全局搜索一下源码，你就会发现其实现在 `runtime.time_now` 中

简单来讲，就是 `importpath.name` 是 `localname` 的符号别名，编译器实际上会调用 `localname` 。

## //go:noescape

该指令指定下一个有声明但没有主体（意味着实现有可能不是 Go）的函数，不允许编译器对其做逃逸分析。

一般情况下，该指令用于内存分配优化。因为编译器默认会进行逃逸分析，会通过规则判定一个变量是分配到堆上还是栈上。

但凡事有意外，一些函数虽然逃逸分析其是存放到堆上。但是对于我们来说，它是特别的。**我们就可以使用 `go:noescape` 指令强制要求编译器将其分配到函数栈上**。

这个指令告诉编译器 下面的func没有任何参数escape。编译器将会跳过对func参数的检查。

```go
// memmove copies n bytes from "from" to "to".
// in memmove_*.s
//go:noescape
func memmove(to, from unsafe.Pointer, n uintptr)
```

## //go:nosplit

该指令指定文件中声明的下一个函数不得包含堆栈溢出检查。简单来讲，就是这个**函数跳过堆栈溢出的检查**

```go
//go:nosplit
func key32(p *uintptr) *uint32 {
	return (*uint32)(unsafe.Pointer(p))
}
```

## //go:norace

该指令表示禁止进行竞态检测。而另外一种常见的形式就是在启动时执行 `go run -race`，能够检测应用程序中是否存在双向的数据竞争。非常有用

```go
var sum int

func main() {
    go add()
    go add()
}

//go:norace
func add() {
    sum++
}
```

## //go:noinline

该指令表示该函数禁止进行内联

```go
//go:noinline
func unexportedPanicForTesting(b []byte, i int) byte {
    return b[i]
}
```

我们观察一下这个案例，是直接通过索引取值，逻辑比较简单。如果不加上 `go:noinline` 的话，就会出现编译器对其进行内联优化

显然，内联有好有坏。该指令就是提供这一特殊处理

## //go:notinheap

该指令常用于类型声明，它表示这个类型不允许从 GC 堆上进行申请内存。在运行时中常用其来做较低层次的内部结构，避免调度器和内存分配中的写屏障。能够提高性能

## //go:nowritebarrierrec

该指令表示编译器遇到写屏障时就会产生一个错误，并且允许递归。也就是这个函数调用的其他函数如果有写屏障也会报错。简单来讲，就是针对写屏障的处理，防止其死循环

```go
//go:nowritebarrierrec
func gcFlushBgCredit(scanWork int64) {
    ...
}
```

## //go:yeswritebarrierrec

该指令与 `go:nowritebarrierrec` 相对，在标注 `go:nowritebarrierrec` 指令的函数上，遇到写屏障会产生错误。而当编译器遇到 `go:yeswritebarrierrec` 指令时将会停止

```go
//go:yeswritebarrierrec
func gchelper() {
	...
}
```

## //go:uintptrkeepalive

## //go:uintptrescapes

//go:uintptrescapes 指令后面必须跟一个函数声明。 它指定函数的 uintptr 参数可能是已转换为 uintptr 的指针值，并且必须在堆上并在调用期间保持活动状态，即使仅从类型来看，在调用期间似乎不再需要该对象 通话。 从指针到 uintptr 的转换必须出现在对该函数的任何调用的参数列表中。 该指令对于某些低级系统调用实现是必需的，否则应避免使用。

```go
//go:uintptrescapes
func F1(a uintptr) {} // ERROR "escaping uintptr"
```

## //go:embed 

指令在编译时使用从包目录或子目录中读取的文件内容来初始化字符串、[]byte 或 FS 类型的变量。

```go
import "embed"

//go:embed hello.txt
var f embed.FS
data, _ := f.ReadFile("hello.txt")
print(string(data))
```

