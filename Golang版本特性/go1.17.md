<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [Tools](#tools)
- [支持将切片转换为数组指针](#%E6%94%AF%E6%8C%81%E5%B0%86%E5%88%87%E7%89%87%E8%BD%AC%E6%8D%A2%E4%B8%BA%E6%95%B0%E7%BB%84%E6%8C%87%E9%92%88)
- [unsafe包新增了unsafe.Add函数 unsafe.Slice](#unsafe%E5%8C%85%E6%96%B0%E5%A2%9E%E4%BA%86unsafeadd%E5%87%BD%E6%95%B0-unsafeslice)
  - [unsafe.Add函数](#unsafeadd%E5%87%BD%E6%95%B0)
  - [unsafe.Slice函数](#unsafeslice%E5%87%BD%E6%95%B0)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

更新时间：2021 年 8 月 16 日

此版本为编译器带来了额外的改进，即传递函数参数和结果的新方法。 这一变化表明 Go 程序的性能提高了约 5%，AMD64 平台的二进制大小减少了约 2%。 未来版本将支持更多平台。

Go 1.17 还增加了对 Windows 上 64 位 ARM 架构的支持，让 gophers 在更多设备上原生运行 Go。

在 go.mod 文件中指定 go 1.17 或更高版本的模块的模块图将仅包含其他 Go 1.17 模块的直接依赖项，而不包含其完整的传递依赖项。

Go 1.17 对语言进行了三项小改动。 前两个是 unsafe 包中的新函数，使程序更容易遵守 unsafe.Pointer 规则：unsafe.Add 允许更安全的指针算术，而 unsafe.Slice 允许更安全地将指针转换为切片。 第三个更改是对语言类型转换规则的扩展，以允许从切片到数组指针的转换，前提是切片在运行时至少与数组一样大。

最后还有相当多的其他改进和错误修复，包括对 crypto/x509 的验证改进以及对 URL 查询解析的更改。

详情 : https://go.dev/doc/go1.17

# Tools

go 1.17 模块中新添加的间接依赖项需求将在与包含直接依赖项的块不同的独立 require 块中维护。

go mod tidy 子命令现在支持 -go 标志来设置或更改 go.mod 文件中的 go 版本。

```go
go mod tidy -go=1.17
```

-compat 标志允许覆盖该版本以支持较旧的（或仅较新的）版本，最高可达 go.mod 文件中 go 指令指定的版本。

```go
go mod tidy -compat=1.17
```

#  支持将切片转换为数组指针

```go
func slice2array() [3]int {
	var b = []int{11, 12, 13}
	var p = (*[3]int)(b)
	// var p = (*[4]int)(b)     // cannot convert slice with length 3 to pointer to array with length 4
	// var p = (*[0]int)(b)     // ok，*p = []
	// var p = (*[1]int)(b)     // ok，*p = [11]
	// var p = (*[2]int)(b)     // ok，*p = [11, 12]
	// var p = (*[3]int)(b)     // ok，*p = [11, 12, 13]
	// var p = (*[3]int)(b[:1]) // cannot convert slice with length 1 to pointer to array with length 3
	return *p
}
```

1.17之前的版本需要借助`unsafe`转换

```go
func slice2arrayptrWithHack() {
	var b = []int{11, 12, 13}
	var p = (*[3]int)(unsafe.Pointer(&b[0]))
	p[1] += 10
	fmt.Printf("%v\n", b) // [11 22 13]
}
```

# unsafe包新增了unsafe.Add函数 unsafe.Slice 

```go
// $GOROOT/src/unsafe.go
func Add(ptr Pointer, len IntegerType) Pointe
func Slice(ptr *ArbitraryType, len IntegerType) []ArbitraryType
```

## unsafe.Add函数

```go
package main

import (
	"fmt"
	"unsafe"
)

const intLen = unsafe.Sizeof(int(8))

// 1.17 版本之前
func foo() {
	var a = [5]int{11, 12, 13, 14, 15}
	for i := 0; i < 5; i++ {
		p := (*int)(unsafe.Pointer(uintptr(unsafe.Pointer(&a[0])) + uintptr(uintptr(i)*intLen)))
		*p = *p + 10
	}
	fmt.Println(a)
}

func bar() {
	var a = [5]int{11, 12, 13, 14, 15}
	for i := 0; i < 5; i++ {
		p := (*int)(unsafe.Add(unsafe.Pointer(&a[0]), uintptr(i)*intLen))
		*p = *p + 10
	}
	fmt.Println(a)
}

func main() {
	foo()
	bar()
}
```

本质

```go
// Should be a built-in for unsafe.Pointer?
//go:nosplit
func add(p unsafe.Pointer, x uintptr) unsafe.Pointer {
    return unsafe.Pointer(uintptr(p) + x)
}
```

## unsafe.Slice函数

函数 Slice 返回一个切片，其底层数组从 ptr 开始，长度和容量为 len。 Slice(ptr, len) 相当于

```go
(*[len]ArbitraryType)(unsafe.Pointer(ptr))[:]
```

但作为一种特殊情况，如果 ptr 为 nil 并且 len 为零，则 Slice 返回 nil。

len 参数必须是整数类型或无类型常量。 常量 len 参数必须是非负的并且可以用 int 类型的值表示； 如果它是无类型常量，则其类型为 int。 在运行时，如果 len 为负，或者 ptr 为 nil 并且 len 不为零，则会发生运行时恐慌。

```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	var a = [5]int{11, 12, 13, 14, 15}
	s1 := a[:]
	s2 := unsafe.Slice(&a[0], 5)

	fmt.Println(s1) // [11 12 13 14 15]
	fmt.Println(s2) // [11 12 13 14 15]
	fmt.Printf("the type of s2 is %T\n", s2)

	s2[2] += 10 // 底层数据共享
	fmt.Println(a)  // [11 12 23 14 15]
	fmt.Println(s1) // [11 12 23 14 15]
	fmt.Println(s2) // [11 12 23 14 15]
}

```

