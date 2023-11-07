<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [创建interface](#%E5%88%9B%E5%BB%BAinterface)
  - [空接口 - eface](#%E7%A9%BA%E6%8E%A5%E5%8F%A3---eface)
  - [带方法的接口 - iface](#%E5%B8%A6%E6%96%B9%E6%B3%95%E7%9A%84%E6%8E%A5%E5%8F%A3---iface)
- [方式实现](#%E6%96%B9%E5%BC%8F%E5%AE%9E%E7%8E%B0)
- [类型断言](#%E7%B1%BB%E5%9E%8B%E6%96%AD%E8%A8%80)
- [编译器自动检测类型是否实现接口](#%E7%BC%96%E8%AF%91%E5%99%A8%E8%87%AA%E5%8A%A8%E6%A3%80%E6%B5%8B%E7%B1%BB%E5%9E%8B%E6%98%AF%E5%90%A6%E5%AE%9E%E7%8E%B0%E6%8E%A5%E5%8F%A3)
- [interface零值](#interface%E9%9B%B6%E5%80%BC)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

接口是一组抽象方法的签名，使用接口能够让我们写出易于测试的代码

不关心怎么实现，我只在乎结果是否符合预期

接口的本质是引入一个新的中间层，调用方可以通过接口与具体实现分离，解除上下游的耦合，上层的模块不再需要依赖下层的具体模块，只需要依赖一个约定好的接口。

代码必须能够被人阅读，只是机器恰好可以执行

> 在 Go 中：实现接口的所有方法就隐式地实现了接口

# 创建interface

## 空接口 - eface

不包含任何方法的接口 interface{}

定义文件：`src/runtime/runtime2.go`

```go
var i interface{}

// 底层结构体  empty？从源代码和汇编指令层面介绍接口的底层数据结构。
type eface struct {
        _type *_type
        data  unsafe.Pointer
}

type _type struct {
        size       uintptr //占用的字节大小
        ptrdata uintptr //指针数据 size of memory prefix holding all pointers
        hash       uint32 //计算的hash
        tflag      tflag //额外的标记信息
        align      uint8 //内存对齐系数
        fieldAlign uint8 //字段内存对齐系数
        kind uint8 //用于标记数据类型
        // function for comparing objects of this type
        // (ptr to object A, ptr to object B) -> ==?
        equal func(unsafe.Pointer, unsafe.Pointer) bool//用于判断当前类型多个对象是否相等
        str       nameOff //名字偏移量
        ptrToThis typeOff //指针的偏移量
}
```

## 带方法的接口 - iface

定义文件：`src/runtime/runtime2.go`

```go
type Person interface {
    GetName() string
    GetAge()  int
}

// 底层结构体
type iface struct {
    tab  *itab
    data unsafe.Pointer
}

type itab struct {
     inter *interfacetype //接口类型的表示
     _type *_type
     hash  uint32 // copy of _type.hash. Used for type switches.
     _     [4]byte
     fun   [1]uintptr // variable sized. fun[0]==0 means _type does not implement inter.
}
```

# 方式实现

用interface调用方法的情况下, `*T` 可以调用`T`方法，反之不行。

使用值类型实现接口带来的开销会大于使用指针实现，而动态派发在结构体上的表现非常差，这也提醒我们应当尽量避免使用值类型类型实现接口。

> 只是结构体方法的话，`T`可以调用`*T`.

```go
package main

type Duck interface {
        Quack()
}

type Cat struct {
        Name string
}

//go:noinline
func (c *Cat) Quack() {
        println(c.Name + " meow")
}

//go:noinline
func (c Cat) Quack() {
        println(c.Name + " meow")
}

func main() {
        var c Duck = &Cat{Name: "draven"}
        c.Quack()
}
```

# 类型断言

前面说过，因为空接口 interface{} 没有定义任何函数，因此 Go 中所有类型都实现了空接口。

```go
t,ok := i.(type) // i 位interface ， type为固定关键字

// 带方法的接口
func main() {
        var c Duck = &Cat{Name: "draven"}
        switch c.(type) {
        case *Cat:
                cat := c.(*Cat)
                cat.Quack()
        }
}
```

# 编译器自动检测类型是否实现接口

```go
// 编译器会由此检查 *myWriter 类型是否实现了 io.Writer 接口。
// 必须要用 _ inter 格式。
var _ io.Writer = (*myWriter)(nil)
var _ io.Writer = myWriter{}

// 例子
type myWriter struct {
}

func (w myWriter) Write(p []byte) (n int, err error) {
        return
}

func main() {
        // 检查 *myWriter 类型是否实现了 io.Writer 接口
        var _ io.Writer = (*myWriter)(nil)

        // 检查 myWriter 类型是否实现了 io.Writer 接口
        var _ io.Writer = myWriter{}
}
```

在转换的过程中，编译器会检测等号右边的类型是否实现了等号左边接口所规定的函数。

# interface零值

接口值的零值是指动态类型和动态值都为 nil。当仅且当这两部分的值都为 nil 的情况下，这个接口值就才会被认为 接口值 == nil。

```go
var i interface{} = nil // i == nil

var s []int
var i interface{} = s // i != nil
```

