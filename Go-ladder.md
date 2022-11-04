<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [前言](#%E5%89%8D%E8%A8%80)
- [String](#string)
  - [string数据结构](#string%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
  - [[]byte转string](#byte%E8%BD%ACstring)
  - [string转[]byte](#string%E8%BD%ACbyte)
  - [string和[]byte如何取舍](#string%E5%92%8Cbyte%E5%A6%82%E4%BD%95%E5%8F%96%E8%88%8D)
  - [字符串拼接性能](#%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%8B%BC%E6%8E%A5%E6%80%A7%E8%83%BD)
  - [比较 strings.Builder 和 bytes.Buffer](#%E6%AF%94%E8%BE%83-stringsbuilder-%E5%92%8C-bytesbuffer)
- [数组](#%E6%95%B0%E7%BB%84)
  - [初始化](#%E5%88%9D%E5%A7%8B%E5%8C%96)
  - [存放位置](#%E5%AD%98%E6%94%BE%E4%BD%8D%E7%BD%AE)
  - [访问和赋值](#%E8%AE%BF%E9%97%AE%E5%92%8C%E8%B5%8B%E5%80%BC)
- [切片](#%E5%88%87%E7%89%87)
  - [slice数据结构](#slice%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
  - [初始化](#%E5%88%9D%E5%A7%8B%E5%8C%96-1)
    - [使用下标](#%E4%BD%BF%E7%94%A8%E4%B8%8B%E6%A0%87)
    - [字面量（块儿）](#%E5%AD%97%E9%9D%A2%E9%87%8F%E5%9D%97%E5%84%BF)
    - [make](#make)
  - [访问元素](#%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
  - [追加和扩容](#%E8%BF%BD%E5%8A%A0%E5%92%8C%E6%89%A9%E5%AE%B9)
  - [拷贝](#%E6%8B%B7%E8%B4%9D)
- [SSA代码](#ssa%E4%BB%A3%E7%A0%81)
- [go编译器指令](#go%E7%BC%96%E8%AF%91%E5%99%A8%E6%8C%87%E4%BB%A4)
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
- [Delve、GDB、LLDB调试（了解）](#delvegdblldb%E8%B0%83%E8%AF%95%E4%BA%86%E8%A7%A3)
- [编译原理（了解）](#%E7%BC%96%E8%AF%91%E5%8E%9F%E7%90%86%E4%BA%86%E8%A7%A3)
  - [go build 过程](#go-build-%E8%BF%87%E7%A8%8B)
  - [流程图](#%E6%B5%81%E7%A8%8B%E5%9B%BE)
  - [语法分析](#%E8%AF%AD%E6%B3%95%E5%88%86%E6%9E%90)
  - [语法分析（抽象语法树）](#%E8%AF%AD%E6%B3%95%E5%88%86%E6%9E%90%E6%8A%BD%E8%B1%A1%E8%AF%AD%E6%B3%95%E6%A0%91)
  - [语义分析（类型检查）](#%E8%AF%AD%E4%B9%89%E5%88%86%E6%9E%90%E7%B1%BB%E5%9E%8B%E6%A3%80%E6%9F%A5)
  - [中间码生成（适应平台，重用代码）](#%E4%B8%AD%E9%97%B4%E7%A0%81%E7%94%9F%E6%88%90%E9%80%82%E5%BA%94%E5%B9%B3%E5%8F%B0%E9%87%8D%E7%94%A8%E4%BB%A3%E7%A0%81)
  - [代码优化](#%E4%BB%A3%E7%A0%81%E4%BC%98%E5%8C%96)
  - [机器码生成](#%E6%9C%BA%E5%99%A8%E7%A0%81%E7%94%9F%E6%88%90)
- [Go汇编（了解）](#go%E6%B1%87%E7%BC%96%E4%BA%86%E8%A7%A3)
  - [CPU、内存与寄存器](#cpu%E5%86%85%E5%AD%98%E4%B8%8E%E5%AF%84%E5%AD%98%E5%99%A8)
  - [通用寄存器](#%E9%80%9A%E7%94%A8%E5%AF%84%E5%AD%98%E5%99%A8)
  - [伪寄存器](#%E4%BC%AA%E5%AF%84%E5%AD%98%E5%99%A8)
  - [汇编程序指令](#%E6%B1%87%E7%BC%96%E7%A8%8B%E5%BA%8F%E6%8C%87%E4%BB%A4)
    - [宏](#%E5%AE%8F)
    - [汇编常量](#%E6%B1%87%E7%BC%96%E5%B8%B8%E9%87%8F)
    - [DATA - 初始化变量](#data---%E5%88%9D%E5%A7%8B%E5%8C%96%E5%8F%98%E9%87%8F)
    - [GLOBL - 全局变量](#globl---%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F)
    - [TEXT - 函数](#text---%E5%87%BD%E6%95%B0)
  - [常见操作指令](#%E5%B8%B8%E8%A7%81%E6%93%8D%E4%BD%9C%E6%8C%87%E4%BB%A4)
    - [MOVQ - 搬运长度](#movq---%E6%90%AC%E8%BF%90%E9%95%BF%E5%BA%A6)
    - [LEAQ - 将有效地址加载到指定的地址寄存器中](#leaq---%E5%B0%86%E6%9C%89%E6%95%88%E5%9C%B0%E5%9D%80%E5%8A%A0%E8%BD%BD%E5%88%B0%E6%8C%87%E5%AE%9A%E7%9A%84%E5%9C%B0%E5%9D%80%E5%AF%84%E5%AD%98%E5%99%A8%E4%B8%AD)
    - [ADDQ - 减法](#addq---%E5%87%8F%E6%B3%95)
    - [SUBQ - 减法](#subq---%E5%87%8F%E6%B3%95)
    - [IMULQ - 无符号乘法](#imulq---%E6%97%A0%E7%AC%A6%E5%8F%B7%E4%B9%98%E6%B3%95)
    - [IDIVQ - 无符号除法](#idivq---%E6%97%A0%E7%AC%A6%E5%8F%B7%E9%99%A4%E6%B3%95)
    - [CMP - 比较](#cmp---%E6%AF%94%E8%BE%83)
    - [AND，OR，XOR - 位运算](#andorxor---%E4%BD%8D%E8%BF%90%E7%AE%97)
    - [JMP - 无条件跳转](#jmp---%E6%97%A0%E6%9D%A1%E4%BB%B6%E8%B7%B3%E8%BD%AC)
    - [JZ，JLS - 有条件跳转](#jzjls---%E6%9C%89%E6%9D%A1%E4%BB%B6%E8%B7%B3%E8%BD%AC)
    - [CALL - 调用函数](#call---%E8%B0%83%E7%94%A8%E5%87%BD%E6%95%B0)
    - [RET - 返回](#ret---%E8%BF%94%E5%9B%9E)
    - [leave - 恢复调用者者栈指针](#leave---%E6%81%A2%E5%A4%8D%E8%B0%83%E7%94%A8%E8%80%85%E8%80%85%E6%A0%88%E6%8C%87%E9%92%88)
  - [寻址模式](#%E5%AF%BB%E5%9D%80%E6%A8%A1%E5%BC%8F)
  - [标志位](#%E6%A0%87%E5%BF%97%E4%BD%8D)
- [Go常用命令](#go%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
  - [go build](#go-build)
- [事件说明](#%E4%BA%8B%E4%BB%B6%E8%AF%B4%E6%98%8E)
- [参考](#%E5%8F%82%E8%80%83)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

![Golang](E:\google\Download\Golang.png)

<div STYLE="page-break-after: always;"></div>

# 前言

阅读本文需要有一定的golang基础，如果你是初学者建议先学习golang基础，这里有一个良好的[教程](https://github.com/stonebirdjx/go-ladder)

> 本文仅提供参考，资料均为个人学习和工作收集，仅供学习交流，不涉及商业用途。
>
> 如果你对相关内容感兴趣，请尊重创作者，遵守相关协议，非商业行为下传播。

<div STYLE="page-break-after: always;"></div>

# String

源代码位于`src/builtin/builtin.go`，其中关于string的描述如下:

```go
// string is the set of all strings of 8-bit bytes, conventionally but not
// necessarily representing UTF-8-encoded text. A string may be empty, but
// not nil. Values of string type are immutable.
type string string
```

所以string是8比特字节的集合，通常但并不一定是UTF-8编码的文本。

另外，还提到了两点，非常重要：

- string可以为空（长度为0），但不会是nil；
- string对象不可以修改。

## string数据结构

源码包`src/runtime/string.go:stringStruct`定义了string的数据结构：

```go
type stringStruct struct {
    str unsafe.Pointer  // str 地址
    len int  // 长度
}
```

string 声明

字符串构建过程是先跟据字符串构建stringStruct，再转换成string。转换的源码如下：

```go
var str = "hello, world"

func gostringnocopy(str *byte) string { // 跟据字符串地址构建string
    ss := stringStruct{str: unsafe.Pointer(str), len: findnull(str)} // 先构造stringStruct
    s := *(*string)(unsafe.Pointer(&ss))                             // 再将stringStruct转换成string
    return s
}
```

## []byte转string

```go
func GetStringBySlice(s []byte) string {
    return string(s)
}
```

string() 这种转换需要一次内存拷贝。

转换过程如下：

1. 跟据切片的长度申请内存空间，假设内存地址为p，切片长度为len(b)；
2. 构建string（string.str = p；string.len = len；）
3. 拷贝数据(切片中数据拷贝到新申请的内存空间)

> []rune 同理

## string转[]byte

```go
func GetSliceByString(str string) []byte {
    return []byte(str)
}
```

string转换成byte切片，也需要一次内存拷贝，其过程如下：

- 申请切片内存空间
- 将string拷贝到切片

## string和[]byte如何取舍

string和[]byte都可以表示字符串，但因数据结构不同，其衍生出来的方法也不同，要跟据实际应用场景来选择。

string 擅长的场景：

- 需要字符串比较的场景；
- 不需要nil字符串的场景；

[]byte擅长的场景：

- 修改字符串的场景，尤其是修改粒度为1个字节；
- 函数返回值，需要用nil表示含义的场景；
- 需要切片操作的场景；

虽然看起来string适用的场景不如[]byte多，但因为string直观，在实际应用中还是大量存在，在偏底层的实现中[]byte使用更多

## 字符串拼接性能

常见的字符串拼接方式有如下 5 种：

- 使用 `+`

- 使用 `fmt.Sprintf`

- 使用 `strings.Builder`

```go
func builderConcat(n int, str string) string {
	var builder strings.Builder
	for i := 0; i < n; i++ {
		builder.WriteString(str)
	}
	return builder.String()
}
```

- 使用 `bytes.Buffer`

```go
func bufferConcat(n int, s string) string {
	buf := new(bytes.Buffer)
	for i := 0; i < n; i++ {
		buf.WriteString(s)
	}
	return buf.String()
}
```

- 使用 `[]byte`

```go
func byteConcat(n int, str string) string {
	buf := make([]byte, 0)
	for i := 0; i < n; i++ {
		buf = append(buf, str...)
	}
	return string(buf)
}
```

- prebyte

如果长度是可预知的，那么创建 `[]byte` 时，我们还可以预分配切片的容量(cap)。

```go
func preByteConcat(n int, str string) string {
	buf := make([]byte, 0, n*len(str))
	for i := 0; i < n; i++ {
		buf = append(buf, str...)
	}
	return string(buf)
}
```

基准测试

```go
$ go test -bench="Concat$" -benchmem .
goos: darwin
goarch: amd64
pkg: example
BenchmarkPlusConcat-8         19      56 ms/op   530 MB/op   10026 allocs/op
BenchmarkSprintfConcat-8      10     112 ms/op   835 MB/op   37435 allocs/op
BenchmarkBuilderConcat-8    8901    0.13 ms/op   0.5 MB/op      23 allocs/op
BenchmarkBufferConcat-8     8130    0.14 ms/op   0.4 MB/op      13 allocs/op
BenchmarkByteConcat-8       8984    0.12 ms/op   0.6 MB/op      24 allocs/op
BenchmarkPreByteConcat-8   17379    0.07 ms/op   0.2 MB/op       2 allocs/op
PASS
ok      example 8.627s
```

使用 `+` 和 `fmt.Sprintf` 的效率是最低的，和其余的方式相比，性能相差约 1000 倍，而且消耗了超过 1000 倍的内存。当然 `fmt.Sprintf` 通常是用来格式化字符串的，一般不会用来拼接字符串。

`strings.Builder`、`bytes.Buffer` 和 `[]byte` 的性能差距不大，而且消耗的内存也十分接近，性能最好且消耗内存最小的是 `preByteConcat`，这种方式预分配了内存，在字符串拼接的过程中，不需要进行字符串的拷贝，也不需要分配新的内存，因此性能最好，且内存消耗最小。

> 综合易用性和性能，一般推荐使用 `strings.Builder` 来拼接字符串。

##  比较 strings.Builder 和 bytes.Buffer

而 `strings.Builder`，`bytes.Buffer`，包括切片 `[]byte` 的内存是以倍数申请的。**参考slice扩容原理**

`strings.Builder` 和 `bytes.Buffer` 底层都是 `[]byte` 数组，但 `strings.Builder` 性能比 `bytes.Buffer` 略快约 10% 。一个比较重要的区别在于，`bytes.Buffer` 转化为字符串时重新申请了一块空间，存放生成的字符串变量，而 `strings.Builder` 直接将底层的 `[]byte` 转换成了字符串类型返回了回来

- bytes.Buffer

```go
// To build strings more efficiently, see the strings.Builder type.
func (b *Buffer) String() string {
	if b == nil {
		// Special case, useful in debugging.
		return "<nil>"
	}
	return string(b.buf[b.off:])
}
```

- strings.Builder

```go
// String returns the accumulated string.
func (b *Builder) String() string {
	return *(*string)(unsafe.Pointer(&b.buf))
}
```

<div STYLE="page-break-after: always;"></div>

# 数组

数组是由相同类型元素的集合组成的数据结构，计算机会为数组分配一块连续的内存来保存其中的元素，我们可以利用数组中元素的索引快速访问特定元素，

> 数组是单一内存块，并无附加结构

## 初始化

Go 语言的数组有两种不同的创建方式，一种是显式的指定数组大小，另一种是使用 `[...]T` 声明数组

```go
arr1 := [3]int{1, 2, 3}
arr2 := [...]int{1, 2, 3}
```

> 语法糖的方式只能用于外层数组

使用第一种方式 `[n]T`，那么变量的类型在编译进行到类型检查阶段（可以查看编译原理）就会被提取出来，该类型包含两个字段，分别是元素类型 `Elem` 和数组的大小 `Bound`，这两个字段共同构成了数组类型，而当前数组是否应该在堆栈中初始化也在编译期就确定了。

随后使用 `cmd/compile/internal/types.NewArray`创建包含数组大小的 `cmd/compile/internal/types.Array`结构体。

```go
func NewArray(elem *Type, bound int64) *Type {
	if bound < 0 {
		Fatalf("NewArray: invalid bound %v", bound)
	}
	t := New(TARRAY)
	t.Extra = &Array{Elem: elem, Bound: bound}
	t.SetNotInHeap(elem.NotInHeap())
	return t
}
```

当我们使用 `[...]T` 的方式声明数组时，编译器会在的 `cmd/compile/internal/gc.typecheckcomplit` 函数中对该数组的大小进行推导

```go
func typecheckcomplit(n *Node) (res *Node) {
	...
	if n.Right.Op == OTARRAY && n.Right.Left != nil && n.Right.Left.Op == ODDD {
		n.Right.Right = typecheck(n.Right.Right, ctxType)
		if n.Right.Right.Type == nil {
			n.Type = nil
			return n
		}
		elemType := n.Right.Right.Type

		length := typecheckarraylit(elemType, -1, n.List.Slice(), "array literal")

		n.Op = OARRAYLIT
		n.Type = types.NewArray(elemType, length)
		n.Right = nil
		return n
	}
	...

	switch t.Etype {
	case TARRAY:
		typecheckarraylit(t.Elem(), t.NumElem(), n.List.Slice(), "array literal")
		n.Op = OARRAYLIT
		n.Right = nil
	}
}
```

## 存放位置

对于一个由字面量组成的数组，根据数组元素数量的不同，编译器会在负责初始化字面量的 `cmd/compile/internal/gc.anylit`函数中做两种不同的优化：

- 当元素数量小于或者等于 4 个时，会直接将数组中的元素放置在栈上；
- 当元素数量大于 4 个时，会将数组中的元素放置到静态区并在运行时取出；

```go
func anylit(n *Node, var_ *Node, init *Nodes) {
	t := n.Type
	switch n.Op {
	case OSTRUCTLIT, OARRAYLIT:
		if n.List.Len() > 4 {
			...
		}

		fixedlit(inInitFunction, initKindLocalCode, n, var_, init)
	...
	}
}
```

总结起来，在不考虑逃逸分析的情况下，如果数组中元素的个数小于或者等于 4 个，那么所有的变量会直接在栈上初始化，如果数组元素大于 4 个，变量就会在静态存储区初始化然后拷贝到栈上，这些转换后的代码才会继续进入`中间代码生成`和`机器码生成`两个阶段，最后生成可以执行的二进制文件。

## 访问和赋值

无论是在栈上还是静态存储区，数组在内存中都是一连串的内存空间，我们通过指向数组开头的指针、元素的数量以及元素类型占的空间大小表示数组。

数组访问越界是非常严重的错误，Go 语言中可以在编译期间的静态类型检查判断数组越界，`cmd/compile/internal/gc.typecheck1`会验证访问数组的索引：

```go
func typecheck1(n *Node, top int) (res *Node) {
	switch n.Op {
	case OINDEX:
		ok |= ctxExpr
		l := n.Left  // array
		r := n.Right // index
		switch n.Left.Type.Etype {
		case TSTRING, TARRAY, TSLICE:
			...
			if n.Right.Type != nil && !n.Right.Type.IsInteger() {
				yyerror("non-integer array index %v", n.Right)
				break
			}
			if !n.Bounded() && Isconst(n.Right, CTINT) {
				x := n.Right.Int64()
				if x < 0 {
					yyerror("invalid array index %v (index must be non-negative)", n.Right)
				} else if n.Left.Type.IsArray() && x >= n.Left.Type.NumElem() {
					yyerror("invalid array index %v (out of bounds for %d-element array)", n.Right, n.Left.Type.NumElem())
				}
			}
		}
	...
	}
}
```

数组的赋值和更新操作 `a[i] = 2` 也会计算出数组当前元素的内存地址，然后修改当前内存地址的内容，这些赋值语句会被转换成如下所示的 SSA 代码：

```go
b1:
    ...
    v21 (5) = LocalAddr <*[3]int> {arr} v2 v19
    v22 (5) = PtrIndex <*int> v21 v13
    v23 (5) = Store <mem> {int} v22 v20 v19
    ...
```

Go赋值的过程中会先确定目标数组的地址，再通过 `PtrIndex` 获取目标元素的地址，最后使用 `Store` 指令将数据存入地址中，上述**数组寻址和赋值都是在编译阶段完成的，没有运行时的参与**。

> 可以导出ssa代码查看
>
> dumped SSA to ./ssa.html

<div STYLE="page-break-after: always;"></div>

# 切片

Slice又称动态数组，依托数组实现，可以方便的进行扩容、传递等，实际使用中比数组更灵活。

## slice数据结构

源码包中`src/runtime/slice.go:slice`定义了Slice的数据结构：

```go
type slice struct {
    array unsafe.Pointer
    len   int
    cap   int
}
```

从数据结构看Slice很清晰, array指针指向底层数组，len表示切片长度，cap表示底层数组容量。

编译期间的切片是 `cmd/compile/internal/types.Slice`类型的，但是在运行时切片可以由如下的` reflect.SliceHeader` 结构体表示，其中:

- `Data` 是指向数组的指针;
- `Len` 是当前切片的长度；
- `Cap` 是当前切片的容量，即 `Data` 数组的大小：

```go
type SliceHeader struct {
	Data uintptr
	Len  int
	Cap  int
}
```

`Data` 是一片连续的内存空间，这片内存空间可以用于存储切片中的全部元素，数组中的元素只是逻辑上的概念，底层存储其实都是连续的，所以我们可以将切片理解成一片连续的内存空间加上长度与容量的标识。

##  初始化

Go 语言中包含三种初始化切片的方式：

1. 通过下标的方式获得数组或者切片的一部分；
2. 使用字面量初始化新的切片；
3. 使用关键字 `make` 创建切片：

```go
arr[0:3] or slice[0:3]
slice := []int{1, 2, 3}
slice := make([]int, 10)
```

### 使用下标

使用下标创建切片是最原始也最接近汇编语言的方式，它是所有方法中最为底层的一种，编译器会将 `arr[0:3]` 或者 `slice[0:3]` 等语句转换成 `OpSliceMake` 操作

```go
func newSlice() []int {
	arr := [3]int{1, 2, 3}
	slice := arr[0:1]
	return slice
}
```

通过 `GOSSAFUNC` 变量编译上述代码可以得到一系列 SSA 中间代码，其中 `slice := arr[0:1]` 语句在 “decompose builtin” 阶段对应的代码如下所示：

```go
v27 (+5) = SliceMake <[]int> v11 v14 v17

name &arr[*[3]int]: v11
name slice.ptr[*int]: v11
name slice.len[int]: v14
name slice.cap[int]: v17
```

`SliceMake` 操作会接受四个参数创建新的切片，元素类型、数组指针、切片大小和容量，这也是我们在数据结构一节中提到的切片的几个字段 ，需要注意的是使用下标初始化切片不会拷贝原数组或者原切片中的数据，它只会创建一个指向原数组的切片结构体，所以修改新切片的数据也会修改原切片。

### 字面量（块儿）

当我们使用字面量 `[]int{1, 2, 3}` 创建新的切片时，`cmd/compile/internal/gc.slicelit`函数会在编译期间将它展开成如下所示的代码片段：

```go
var vstat [3]int
vstat[0] = 1
vstat[1] = 2
vstat[2] = 3
var vauto *[3]int = new([3]int)
*vauto = vstat
slice := vauto[:]
```

1. 根据切片中的元素数量对底层数组的大小进行推断并创建一个数组；
2. 将这些字面量元素存储到初始化的数组中；
3. 创建一个同样指向 `[3]int` 类型的数组指针；
4. 将静态存储区的数组 `vstat` 赋值给 `vauto` 指针所在的地址；
5. 通过 `[:]` 操作获取一个底层使用 `vauto` 的切片；

第 5 步中的 `[:]` 就是使用下标创建切片的方法，从这一点我们也能看出 `[:]` 操作是创建切片最底层的一种方法。

### make

如果使用字面量的方式创建切片，大部分的工作都会在编译期间完成。但是当我们使用 `make` 关键字创建切片时，很多工作都需要运行时的参与；调用方必须向 `make` 函数传入切片的大小以及可选的容量，类型检查期间的 `cmd/compile/internal/gc.typecheck1` 函数会校验入参：

```go
func typecheck1(n *Node, top int) (res *Node) {
	switch n.Op {
	...
	case OMAKE:
		args := n.List.Slice()

		i := 1
		switch t.Etype {
		case TSLICE:
			if i >= len(args) {
				yyerror("missing len argument to make(%v)", t)
				return n
			}

			l = args[i]
			i++
			var r *Node
			if i < len(args) {
				r = args[i]
			}
			...
			if Isconst(l, CTINT) && r != nil && Isconst(r, CTINT) && l.Val().U.(*Mpint).Cmp(r.Val().U.(*Mpint)) > 0 {
				yyerror("len larger than cap in make(%v)", t)
				return n
			}

			n.Left = l
			n.Right = r
			n.Op = OMAKESLICE
		}
	...
	}
}
```

如果当前的切片不会发生逃逸并且切片非常小的时候，`make([]int, 3, 4)` 会被直接转换成如下所示的代码：

```go
var arr [4]int
n := arr[:3]
```

当切片发生逃逸或者非常大时，运行时需要 `runtime.makeslice` 在堆上初始化切片

```go
func makeslice(et *_type, len, cap int) unsafe.Pointer {
	mem, overflow := math.MulUintptr(et.size, uintptr(cap))
	if overflow || mem > maxAlloc || len < 0 || len > cap {
		mem, overflow := math.MulUintptr(et.size, uintptr(len))
		if overflow || mem > maxAlloc || len < 0 {
			panicmakeslicelen()
		}
		panicmakeslicecap()
	}

	return mallocgc(mem, et, true)
}
```

## 访问元素

使用 `len` 和 `cap` 获取长度或者容量是切片最常见的操作，编译器将这它们看成两种特殊操作，即 `OLEN` 和 `OCAP`，`cmd/compile/internal/gc.state.expr`阶段将它们分别转换成 `OpSliceLen` 和 `OpSliceCap`：

```go
func (s *state) expr(n *Node) *ssa.Value {
	switch n.Op {
	case OLEN, OCAP:
		switch {
		case n.Left.Type.IsSlice():
			op := ssa.OpSliceLen
			if n.Op == OCAP {
				op = ssa.OpSliceCap
			}
			return s.newValue1(op, types.Types[TINT], s.expr(n.Left))
		...
		}
	...
	}
}
```

访问切片中的字段可能会触发 “decompose builtin” 阶段的优化，`len(slice)` 或者 `cap(slice)` 在一些情况下会直接替换成切片的长度或者容量，不需要在运行时获取：

```go
(SlicePtr (SliceMake ptr _ _ )) -> ptr
(SliceLen (SliceMake _ len _)) -> len
(SliceCap (SliceMake _ _ cap)) -> cap
```

除了获取切片的长度和容量之外，访问切片中元素使用的 `OINDEX` 操作也会在中间代码生成期间转换成对地址的直接访问：

```go
func (s *state) expr(n *Node) *ssa.Value {
	switch n.Op {
	case OINDEX:
		switch {
		case n.Left.Type.IsSlice():
			p := s.addr(n, false)
			return s.load(n.Left.Type.Elem(), p)
		...
		}
	...
	}
}
```

切片的操作基本都是在编译期间完成的，除了访问切片的长度、容量或者其中的元素之外，编译期间也会将包含 `range` 关键字的遍历转换成形式更简单的循环，我们会在后面的章节中介绍使用 `range` 遍历切片的过程。

## 追加和扩容

使用 `append` 关键字向切片中追加元素也是常见的切片操作，中间代码生成阶段的 `cmd/compile/internal/gc.state.append`方法会根据返回值是否会覆盖原变量，选择进入两种流程:

如果 `append` 返回的新切片不需要赋值回原有的变量，就会进入如下的处理流程：

```go
// append(slice, 1, 2, 3)
ptr, len, cap := slice
newlen := len + 3
if newlen > cap {
    ptr, len, cap = growslice(slice, newlen)
    newlen = len + 3
}
*(ptr+len) = 1
*(ptr+len+1) = 2
*(ptr+len+2) = 3
return makeslice(ptr, newlen, cap)
```

如果使用 `slice = append(slice, 1, 2, 3)` 语句，那么 `append` 后的切片会覆盖原切片，这时 `cmd/compile/internal/gc.state.append `方法会使用另一种方式展开关键字：

```go
// slice = append(slice, 1, 2, 3)
a := &slice
ptr, len, cap := slice
newlen := len + 3
if uint(newlen) > uint(cap) {
   newptr, len, newcap = growslice(slice, newlen)
   vardef(a)
   *a.cap = newcap
   *a.ptr = newptr
}
newlen = len + 3
*a.len = newlen
*(ptr+len) = 1
*(ptr+len+1) = 2
*(ptr+len+2) = 3
```

当切片的容量不足时，我们会调用 [`runtime.growslice`](https://draveness.me/golang/tree/runtime.growslice) 函数为切片扩容，扩容是为切片分配新的内存空间并拷贝原切片中元素的过程，我们先来看新切片的容量是如何确定的：

```go
func growslice(et *_type, old slice, cap int) slice {
	newcap := old.cap
	doublecap := newcap + newcap
	if cap > doublecap {
		newcap = cap
	} else {
		if old.len < 1024 {
			newcap = doublecap
		} else {
			for 0 < newcap && newcap < cap {
				newcap += newcap / 4
			}
			if newcap <= 0 {
				newcap = cap
			}
		}
	}
```

在分配内存空间之前需要先确定新的切片容量，运行时根据切片的当前容量选择不同的策略进行扩容：

1. 如果期望容量大于当前容量的两倍就会使用期望容量；
2. 如果当前切片的长度小于 1024 就会将容量翻倍；
3. 如果当前切片的长度大于 1024 就会每次增加 25% 的容量，直到新容量大于期望容量；

上述代码片段仅会确定切片的大致容量，下面还需要根据切片中的元素大小对齐内存，当数组中元素所占的字节大小为 1、8 或者 2 的倍数时，

简单总结一下扩容的过程，当我们执行上述代码时，会触发 [`runtime.growslice`](https://draveness.me/golang/tree/runtime.growslice) 函数扩容 `arr` 切片并传入期望的新容量 5，这时期望分配的内存大小为 40 字节；不过因为切片中的元素大小等于 `sys.PtrSize`，所以运行时会调用 [`runtime.roundupsize`](https://draveness.me/golang/tree/runtime.roundupsize) 向上取整内存的大小到 48 字节，所以新切片的容量为 48 / 8 = 6。

## 拷贝

切片的拷贝虽然不是常见的操作，但是却是我们学习切片实现原理必须要涉及的。当我们使用 `copy(a, b)` 的形式对切片进行拷贝时，编译期间的 [`cmd/compile/internal/gc.copyany`](https://draveness.me/golang/tree/cmd/compile/internal/gc.copyany) 也会分两种情况进行处理拷贝操作，如果当前 `copy` 不是在运行时调用的，`copy(a, b)` 会被直接转换成下面的代码：

```go
n := len(a)
if n > len(b) {
    n = len(b)
}
if a.ptr != b.ptr {
    memmove(a.ptr, b.ptr, n*sizeof(elem(a))) 
}
```

上述代码中的 [`runtime.memmove`](https://draveness.me/golang/tree/runtime.memmove) 会负责拷贝内存。而如果拷贝是在运行时发生的，例如：`go copy(a, b)`，编译器会使用 [`runtime.slicecopy`](https://draveness.me/golang/tree/runtime.slicecopy) 替换运行期间调用的 `copy`，该函数的实现很简单：

```go
func slicecopy(to, fm slice, width uintptr) int {
	if fm.len == 0 || to.len == 0 {
		return 0
	}
	n := fm.len
	if to.len < n {
		n = to.len
	}
	if width == 0 {
		return n
	}
	...

	size := uintptr(n) * width
	if size == 1 {
		*(*byte)(to.array) = *(*byte)(fm.array)
	} else {
		memmove(to.array, fm.array, size)
	}
	return n
}
```

<div STYLE="page-break-after: always;"></div>

# SSA代码

后端采用了更加形态更加贴近目标程序的IR来表示源代码，这种结构叫着SSA(Static Single Assignment)。

SSA是三地址代码（Three-address Code）的一种变体，SSA要求代码中每个变量只能被赋值一次，并且任何变量在使用之前必须先申明。这是一种极简的代码形式，基于对变量的以上两点约束，编译器可以很安全地对代码进行各种操作及优化，例如如果一个变量被赋值后没有被使用，那么其赋值语句就是可以删掉的。

Go 的编译器后端也使用 SSA 来作为代码的中间表示，函数编译的过程便是先将函数对应的 IR Tree 节点欢转换为 SSA, 再将该 SSA 翻译成目标机器代码。

我们可以通过两个环境变量来 dump 编译器生成的 SSA 结果：

- `GOSSAFUNC`: 指定需要 dump 的函数
- `GOSSADIR`: 指定结果文件的目录，默认为当前目录

```go
package main

type T struct {
    Name string
}

func (this *T) getName() string {
    if this == nil {
        return "<nil>"
    }
    return this.Name
}

func sum(i, j int) int {
    return i + j + 1
}
```

运行命令： `GOSSAFUNC="sum" go build main.go`:

> dumped SSA to ./ssa.html
>
> runtime.mainmain·f: function main is undeclared in the main package

<div STYLE="page-break-after: always;"></div>

# [go编译器指令](https://pkg.go.dev/cmd/compile)

编译指令的语法是一行特殊的注释，关键字//go开始(之间没有空格), 在cmd/compilex,很多还未列出如//go:build、Cgo、//line等需要详细看看

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

<div STYLE="page-break-after: always;"></div>

# Delve、GDB、LLDB调试（了解）

目前 Go 语言支持 GDB、LLDB 和 Delve 几种调试器。其中 GDB 是最早支持的调试工具，LLDB 是 macOS 系统推荐的标准调试工具。但是 GDB 和 LLDB 对 Go 语言的专有特性都缺乏很大支持，而只有 Delve 是专门为 Go 语言设计开发的调试工具。

需要关闭内联调试

```bash
# 关闭内联优化，方便调试
go build -gcflags "-N -l" demo.go

gdb demo
lldb demo 
delve demo
```

delve参数

```go
(dlv) help
The following commands are available:
    args ------------------------ Print function arguments.
    break (alias: b) ------------ Sets a breakpoint.
    breakpoints (alias: bp) ----- Print out info for active breakpoints.
    clear ----------------------- Deletes breakpoint.
    clearall -------------------- Deletes multiple breakpoints.
    condition (alias: cond) ----- Set breakpoint condition.
    config ---------------------- Changes configuration parameters.
    continue (alias: c) --------- Run until breakpoint or program termination.
    disassemble (alias: disass) - Disassembler.
    down ------------------------ Move the current frame down.
    exit (alias: quit | q) ------ Exit the debugger.
    frame ----------------------- Set the current frame, or execute command...
    funcs ----------------------- Print list of functions.
    goroutine ------------------- Shows or changes current goroutine
    goroutines ------------------ List program goroutines.
    help (alias: h) ------------- Prints the help message.
    list (alias: ls | l) -------- Show source code.
    locals ---------------------- Print local variables.
    next (alias: n) ------------- Step over to next source line.
    on -------------------------- Executes a command when a breakpoint is hit.
    print (alias: p) ------------ Evaluate an expression.
    regs ------------------------ Print contents of CPU registers.
    restart (alias: r) ---------- Restart process.
    set ------------------------- Changes the value of a variable.
    source ---------------------- Executes a file containing a list of delve...
    sources --------------------- Print list of source files.
    stack (alias: bt) ----------- Print stack trace.
    step (alias: s) ------------- Single step through program.
    step-instruction (alias: si)  Single step a single cpu instruction.
    stepout --------------------- Step out of the current function.
    thread (alias: tr) ---------- Switch to the specified thread.
    threads --------------------- Print out info for every traced thread.
    trace (alias: t) ------------ Set tracepoint.
    types ----------------------- Print list of types
    up -------------------------- Move the current frame up.
    vars ------------------------ Print package variables.
    whatis ---------------------- Prints type of an expression.
Type help followed by a command for full documentation.
(dlv)
```

要熟练使用 GDB ，你得熟悉的掌握它的指令，这里列举一下

- `r`：run，执行程序 
- `n`：next，下一步，不进入函数
- `s`：step，下一步，会进入函数
- `b`：breakponit，设置断点
- `l`：list，查看源码
- `c`：continue，继续执行到下一断点
- `bt`：backtrace，查看当前调用栈
- `p`：print，打印查看变量
- `q`：quit，退出 GDB
- `whatis`：查看对象类型
- `info breakpoints`：查看所有的断点
- `info locals`：查看局部变量
- `info args`：查看函数的参数值及要返回的变量值 
- `info frame`：堆栈帧信息
- `info goroutines`：查看 goroutines 信息。在使用前 ，需要注意先执行 source /usr/local/go/src/runtime/runtime-gdb.py
- `goroutine 1 bt`：查看指定序号的 goroutine 调用堆栈
- 回车：重复执行上一次操作

<div STYLE="page-break-after: always;"></div>

# 编译原理（了解）

## go build 过程

go build 参数说明

```bash
-a	将命令源码文件与库源码文件全部重新构建，即使是最新的
-n	把编译期间涉及的命令全部打印出来，但不会真的执行，非常方便我们学习
-race	开启竞态条件的检测，支持的平台有限制
-x	打印编译期间用到的命名，它与 -n 的区别是，它不仅打印还会执行
```

`go build -n main.go`

```go
E:\SomeFile\gospace\helloworld>go build -n main.go
...
"C:\\Program Files\\Go\\pkg\\tool\\windows_amd64\\compile.exe" -o "$WORK\\b001\\_pkg_.a" -trimpath "$WORK\\b001=>" -p main -complete -buildid v3BL3MHm16Q3kjspXXCg/v3BL3MHm16Q3kjspXXCg
 -goversion go1.18.2 -c=4 -nolocalimports -importcfg "$WORK\\b001\\importcfg" -pack 
...
"C:\\Program Files\\Go\\pkg\\tool\\windows_amd64\\buildid.exe" -w "$WORK\\b001\\_pkg_.a" # internal
...
"C:\\Program Files\\Go\\pkg\\tool\\windows_amd64\\link.exe" -o "$WORK\\b001\\exe\\a.out.exe" -importcfg "$WORK\\b001\\importcfg.link" -buildmode=pie -buildid=qEvHouU39HqKxAChmahv/v3BL
3MHm16Q3kjspXXCg/v3BL3MHm16Q3kjspXXCg/qEvHouU39HqKxAChmahv -extld=gcc "$WORK\\b001\\_pkg_.a"
```

这一部分是编译的核心，通过 `compile`、 `buildid`、 `link` 三个命令会编译出可执行文件 `a.out`。

然后通过 `mv` 更换成最终名字

## 流程图

Go 语言编译器的源代码在 [`src/cmd/compile`](https://github.com/golang/go/tree/master/src/cmd/compile) 目录中，目录下的文件共同组成了 Go 语言的编译器。

![go-byq-3](E:\SomeFile\stonebirdjx\static\go-ladder\go-byq-3.png)

编译器分前后端

- 前端一般承担着词法分析、语法分析、语义分析（类型检查）。

- 后端主要负责 中间码生成、目标代码优化，机器码生成。

## 语法分析

源代码在计算机眼里其实是一团乱麻，一个由字符组成的、无法被理解的字符串，所有的字符在计算器看来并没有什么区别。词法分析简单来说就是将我们写的源代码翻译成 `Token`

```Go
package main

import (
    "fmt"
    "go/scanner"
    "go/token"
)

func main() {
    src := []byte(`
package main

import "fmt"

func main() {
    fmt.Println("Hello, bytedance!")
}
`)

    var s scanner.Scanner
    fset := token.NewFileSet()
    file := fset.AddFile("", fset.Base(), len(src))
    s.Init(file, src, nil, 0)

    for {
        pos, tok, lit := s.Scan()
        fmt.Printf("%-6s%-8s%q\n", fset.Position(pos), tok, lit)

        if tok == token.EOF {
            break
        }
    }
}
```

输出

```bash
2:1   package "package"
2:9   IDENT   "main"
2:13  ;       "\n"
4:1   import  "import"
4:8   STRING  "\"fmt\""
4:13  ;       "\n"
6:1   func    "func"
6:6   IDENT   "main"
6:10  (       ""
6:11  )       ""
6:13  {       ""
7:5   IDENT   "fmt"
7:8   .       ""
7:9   IDENT   "Println"
7:16  (       ""
7:17  STRING  "\"Hello, bytedance!\""
7:36  )       ""
7:37  ;       "\n"
8:1   }       ""
8:2   ;       "\n"
8:3   EOF     ""
```

## 语法分析（抽象语法树）

语法分析是拿到Token，它将作为语法分析器的输入。然后经过处理后生成 `AST` 结构作为输出。

所谓的语法分析就是将 `Token` 转化为可识别的程序语法结构，而 `AST` 就是这个语法的抽象表示。构造这颗树有两种方法。

​	1、自上而下 这种方式会首先构造根节点，然后就开始扫描 `Token`，遇到 `STRING` 或者其它类型就知道这是在进行类型申明，`func` 就表示是函数申明。就这样一直扫描直到程序结束。

​	2、自下而上 这种是与上一种方式相反的，它先构造子树，然后再组装成一颗完整的树。

```go
package main

import (
    "go/ast"
    "go/parser"
    "go/token"
    "log"
)

func main() {
    src := []byte(`
package main

import "fmt"

func main() {
    fmt.Println("Hello, bytedance!")
}
`)

    fset := token.NewFileSet()

    file, err := parser.ParseFile(fset, "", src, 0)
    if err != nil {
        log.Fatal(err)
    }

    ast.Print(fset, file)
}
```

ATS

```bash
 0  *ast.File {
     1  .  Package: 2:1
     2  .  Name: *ast.Ident {
     3  .  .  NamePos: 2:9
     4  .  .  Name: "main"
     5  .  }
     6  .  Decls: []ast.Decl (len = 2) {
     7  .  .  0: *ast.GenDecl {
     8  .  .  .  TokPos: 4:1
     9  .  .  .  Tok: import
    10  .  .  .  Lparen: -
    11  .  .  .  Specs: []ast.Spec (len = 1) {
    12  .  .  .  .  0: *ast.ImportSpec {
    13  .  .  .  .  .  Path: *ast.BasicLit {
    14  .  .  .  .  .  .  ValuePos: 4:8
    15  .  .  .  .  .  .  Kind: STRING
    16  .  .  .  .  .  .  Value: "\"fmt\""
    17  .  .  .  .  .  }
    18  .  .  .  .  .  EndPos: -
    19  .  .  .  .  }
    20  .  .  .  }
    21  .  .  .  Rparen: -
    22  .  .  }
    23  .  .  1: *ast.FuncDecl {
    24  .  .  .  Name: *ast.Ident {
    25  .  .  .  .  NamePos: 6:6
    26  .  .  .  .  Name: "main"
    27  .  .  .  .  Obj: *ast.Object {
    28  .  .  .  .  .  Kind: func
    29  .  .  .  .  .  Name: "main"
    30  .  .  .  .  .  Decl: *(obj @ 23)
    31  .  .  .  .  }
    32  .  .  .  }
    33  .  .  .  Type: *ast.FuncType {
    34  .  .  .  .  Func: 6:1
    35  .  .  .  .  Params: *ast.FieldList {
    36  .  .  .  .  .  Opening: 6:10
    37  .  .  .  .  .  Closing: 6:11
    38  .  .  .  .  }
    39  .  .  .  }
    40  .  .  .  Body: *ast.BlockStmt {
    41  .  .  .  .  Lbrace: 6:13
    42  .  .  .  .  List: []ast.Stmt (len = 1) {
    43  .  .  .  .  .  0: *ast.ExprStmt {
    44  .  .  .  .  .  .  X: *ast.CallExpr {
    45  .  .  .  .  .  .  .  Fun: *ast.SelectorExpr {
    46  .  .  .  .  .  .  .  .  X: *ast.Ident {
    47  .  .  .  .  .  .  .  .  .  NamePos: 7:5
    48  .  .  .  .  .  .  .  .  .  Name: "fmt"
    49  .  .  .  .  .  .  .  .  }
    50  .  .  .  .  .  .  .  .  Sel: *ast.Ident {
    51  .  .  .  .  .  .  .  .  .  NamePos: 7:9
    52  .  .  .  .  .  .  .  .  .  Name: "Println"
    53  .  .  .  .  .  .  .  .  }
    54  .  .  .  .  .  .  .  }
    55  .  .  .  .  .  .  .  Lparen: 7:16
    56  .  .  .  .  .  .  .  Args: []ast.Expr (len = 1) {
    57  .  .  .  .  .  .  .  .  0: *ast.BasicLit {
    58  .  .  .  .  .  .  .  .  .  ValuePos: 7:17
    59  .  .  .  .  .  .  .  .  .  Kind: STRING
    60  .  .  .  .  .  .  .  .  .  Value: "\"Hello, bytedance!\""
    61  .  .  .  .  .  .  .  .  }
    62  .  .  .  .  .  .  .  }
    63  .  .  .  .  .  .  .  Ellipsis: -
    64  .  .  .  .  .  .  .  Rparen: 7:36
    65  .  .  .  .  .  .  }
    66  .  .  .  .  .  }
    67  .  .  .  .  }
    68  .  .  .  .  Rbrace: 8:1
    69  .  .  .  }
    70  .  .  }
    71  .  }
    72  .  Scope: *ast.Scope {
    73  .  .  Objects: map[string]*ast.Object (len = 1) {
    74  .  .  .  "main": *(obj @ 27)
    75  .  .  }
    76  .  }
    77  .  Imports: []*ast.ImportSpec (len = 1) {
    78  .  .  0: *(obj @ 12)
    79  .  }
    80  .  Unresolved: []*ast.Ident (len = 1) {
    81  .  .  0: *(obj @ 46)
    82  .  }
    83  }

```

> gofmt的原理就是先转抽象语法树，再转回代码

## 语义分析（类型检查）

`AST` 生成后，语义分析将使用它作为输入，并且的有一些相关的操作也会直接在这颗树上进行改写。

首先就是 `Golang` 文档中提到的会进行类型检查，还有类型推断，查看类型是否匹配，是否进行隐式转化（go 没有隐式转化）。

第一步是进行名称检查和类型推断，签定每个对象所属的标识符，以及每个表达式具有什么类型。类型检查也还有一些其它的检查要做，像“声明未使用”以及确定函数是否中止。

*我们常常在 debug 代码的时候，需要禁止内联，其实就是操作的这个阶段。*

```Bash
# 编译的时候禁止内联
go build -gcflags '-N -l'

-N 禁止编译优化
-l 禁止内联,禁止内联也可以一定程度上减小可执行程序大小
```

## 中间码生成（适应平台，重用代码）

然已经拿到 AST，机器运行需要的又是二进制。为什么不直接翻译成二进制呢？其实到目前为止从技术上来说已经完全没有问题了。

但是，我们有各种各样的操作系统，有不同的 CPU 类型，每一种的位数可能不同；寄存器能够使用的指令也不同，像是复杂指令集与精简指令集等；在进行各个平台的兼容之前，我们还需要替换一些底层函数，比如我们使用 make 来初始化 slice，此时会根据传入的类型替换为：`makeslice64` 或者 `makeslice`。当然还有像 painc、channel 等等函数的替换也会在中间码生成过程中进行替换。

**中间码存在的另外一个价值是提升后端编译的重用**，比如我们定义好了一套中间码应该是长什么样子，那么后端机器码生成就是相对固定的。每一种语言只需要完成自己的编译器前端工作即可。这也是大家可以看到现在开发一门新语言速度比较快的原因。编译是绝大部分都可以重复使用的。

而且为了接下来的优化工作，中间代码存在具有非凡的意义。因为有那么多的平台，如果有中间码我们可以把一些共性的优化都放到这里。

中间码也是有多种格式的，像 `Golang` 使用的就是 SSA 特性的中间码(IR)，这种形式的中间码，最重要的一个特性就是最在使用变量之前总是定义变量，并且每个变量只分配一次。

## 代码优化

在 go 的编译文档中，我并没找到独立的一步进行代码的优化。不过根据我们上面的分析，可以看到其实代码优化过程遍布编译器的每一个阶段。大家都会力所能及的做些事情。

通常我们除了用高效代码替换低效的之外，还有如下的一些处理：

- 并行性，充分利用现在多核计算机的特性
- 流水线，cpu 有时候在处理 a 指令的时候，还能同时处理 b 指令
- 指令的选择，为了让 cpu 完成某些操作，需要使用指令，但是不同的指令效率有非常大的差别，这里会进行指令优化
- 利用寄存器与高速缓存，我们都知道 cpu 从寄存器取是最快的，从高速缓存取次之。这里会进行充分的利用

## 机器码生成

经过优化后的中间代码，首先会在这个阶段被转化为汇编代码（Plan9），而汇编语言仅仅是机器码的文本表示，机器还不能真的去执行它。所以这个阶段会调用汇编器，汇编器会根据我们在执行编译时设置的架构，调用对应代码来生成目标机器码。

这里比有意思的是，`Golang` 总说自己的汇编器是跨平台的。其实他也是写了多分代码来翻译最终的机器码。因为在入口的时候他会根据我们所设置的 `GOARCH=xxx` 参数来进行初始化处理，然后最终调用对应架构编写的特定方法来生成机器码。这种上层逻辑一致，底层逻辑不一致的处理方式非常通用，非常值得我们学习。我们简单来一下这个处理。

首先看入口函数 `cmd/compile/main.go:main()`

```go
var archInits = map[string]func(*gc.Arch){
    "386":      x86.Init,
    "amd64":    amd64.Init,
    "amd64p32": amd64.Init,
    "arm":      arm.Init,
    "arm64":    arm64.Init,
    "mips":     mips.Init,
    "mipsle":   mips.Init,
    "mips64":   mips64.Init,
    "mips64le": mips64.Init,
    "ppc64":    ppc64.Init,
    "ppc64le":  ppc64.Init,
    "s390x":    s390x.Init,
    "wasm":     wasm.Init,
}

func main() {
    // 从上面的map根据参数选择对应架构的处理
    archInit, ok := archInits[objabi.GOARCH]
    if !ok {
        ......
    }
    // 把对应cpu架构的对应传到内部去
    gc.Main(archInit)
}
```

然后在 `cmd/internal/obj/plist.go` 中调用对应架构的方法进行处理

```
func Flushplist(ctxt *Link, plist *Plist, newprog ProgAlloc, myimportpath string) {
    ... ...
    for _, s := range text {
        mkfwd(s)
        linkpatch(ctxt, s, newprog)
        // 对应架构的方法进行自己的机器码翻译
        ctxt.Arch.Preprocess(ctxt, s, newprog)
        ctxt.Arch.Assemble(ctxt, s, newprog)

        linkpcln(ctxt, s)
        ctxt.populateDWARF(plist.Curfn, s, myimportpath)
    }
}
```

整个过程下来，可以看到编译器后端有很多工作需要做的，你需要对某一个指令集、cpu 的架构了解，才能正确的进行翻译机器码。同时不能仅仅是正确，一个语言的效率是高还是低，也在很大程度上取决于编译器后端的优化。特别是即将进入 AI 时代，越来越多的芯片厂商诞生，我估计以后对这方面人才的需求会变得越来越旺盛。

<div STYLE="page-break-after: always;"></div>

# Go汇编（了解）

Go 语言使用的 Plan 9 汇编。

以输出`hello，world`为例，Plan 9 汇编代码的整体逻辑同样是分了 Data 段、Text 段，同样是用 mov 等指令给寄存器赋值。

```bash
#include "textflag.h"

DATA  msg<>+0x00(SB)/8, $"Hello, W"
DATA  msg<>+0x08(SB)/8, $"orld!\n"
GLOBL msg<>(SB),NOPTR,$16

TEXT ·PrintMe(SB), NOSPLIT, $0
	MOVL 	$(0x2000000+4), AX 	 // write 系统调用数字编号 4
	MOVQ 	$1, DI 			      // 第 1 个参数 fd
	LEAQ 	msg<>(SB), SI 		// 第 2 个参数 buffer 指针地址
	MOVL 	$16, DX 		        // 第 3 个参数 count
	SYSCALL
	RET
```

go获取汇编代码

```bash
go tool compile -S main.go
# or
go build -gcflags -S main.go
```

## CPU、内存与寄存器

汇编主要是跟 CPU 和内存打交道，CPU 本身只负责运算，不负责存储，数据存储一般都是放在内存中。

我们知道 CPU 的运算速度远高于内存的读写速度，为了 CPU 不被内存读写拖后腿，CPU 内部引入**一级缓存、二级缓存和寄存器**的概念，这些资源都非常宝贵，**寄存器可以认为是在 CPU 内可以存储非常少量数据的超高速的存储单元**。因为寄存器个数有限且非常重要，每个寄存器都有自己的名字，最常用的有以下。

```bash
%EAX %EBX %ECX %EDX %EDI %ESI %EBP %ESP
```

## 通用寄存器

plan 9 汇编的通用寄存器包括以下寄存器。

`AX BX CX DX DI SI BP SP R8 R9 R10 R11 R12 R13 R14 PC`

plan9 中使用寄存器**不需要带 r 或 e 的前缀**，例如 rax，只要写 AX 就可以了。

```bash
eax->AX
ebx->BX
ecx->CX
```

plan9 汇编语言提供了如下映射，在汇编语言中直接引用就可使用物理寄存器了。

| amd64 | rax  | rbx  | rcx  | rdx  | rdi  | rsi  | rbp  | rsp  | r8   | r9   | r10  | r11  | r12  | r13  | r14  | rip  |
| ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Plan9 | AX   | BX   | CX   | DX   | DI   | SI   | BP   | SP   | R8   | R9   | R10  | R11  | R12  | R13  | R14  | PC   |

## 伪寄存器

伪寄存器并不是真正的寄存器，而是虚拟寄存器（由工具链维护），例如帧指针。

Go 汇编引入了四个伪寄存器，这四个伪寄存器非常重要：

- FP(Frame pointer)-帧指针：用来访问函数的参数
- PC(Program counter)-程序计数器：用于分支和跳转
- SB(Static base pointer)-静态基础指针：一般用于声明函数或者全局变量
- SP(Stack pointer)-栈指针：指向当前栈帧的局部变量的开始位置，一般用来引用函数的局部变量

所有用户定义的符号都作为偏移量写入伪寄存器 FP 和 SB。

汇编代码中需要表示用户定义的符号(变量)时，可以通过 SP 与偏移还有变量名的组合，比如`x-8(SP)` ，因为 SP 指向的是栈顶，所以偏移值都是负的，`x`则表示变量名

## 汇编程序指令

全局数据符号由一系列以 DATA 指令起始和一个 GLOBL 指令定义。

### 宏

在汇编文件中可以定义、引用宏。通过`#define get_tls(r)  MOVQ TLS, r`类似语句来定义一个宏，语法结构与C语言类似；通过`#include "textflag.h"`类似语句来引用一个外部宏定义文件。

```go
type vdsoVersionKey struct {
	version string
	verHash uint32
}

#define vdsoVersionKey__size 24
#define vdsoVersionKey_version 0
#define vdsoVersionKey_verHash 16

MOVQ vdsoVersionKey_version(DX) AX
MOVQ (vdsoVersionKey_version+vdsoVersionKey_verHash)(DX) AX
```

### 汇编常量

```bash
$1           # 十进制
$0xf4f8fcff  # 十六进制
$1.5         # 浮点数
$'a'         # 字符
$"abcd"      # 字符串

$2+2      # 常量表达式
$3&1<<2   # == $4
$(3&1)<<2 # == $4
```

### DATA - 初始化变量

DATA 命令用于初始化变量，格式`DATA symbol+offset(SB)/width, value` ,在给定的 offset 和 width 处初始化该符号的内存为 value。 DATA 必须使用增加的偏移量来写入给定符号的指令。

```go
DATA  msg<>+0x00(SB)/8, $"Hello, W"
DATA  msg<>+0x08(SB)/8, $"orld!\n"
```

### GLOBL - 全局变量

GLOBL 指令声明符号是全局的。参数是可选标志，并且数据的大小被声明为全局， **除非 DATA 指令已初始化，否则初始值将全为零**。该 GLOBL 指令必须遵循任何相应的 DATA 指令。

```go
GLOBL msg<>(SB),NOPTR,$16
```

注意到 msg 后面有一个`<>`，这表示这个全局变量只在当前文件中可以被访问

```bash
GLOBL divtab<>(SB), RODATA, $64
```

这条给变量`divtab<>`加了一个 flag `RODATA` ，表示里边存的是只读变量，最后的`$64`表示的是这个变量占用了 64 字节的空间。

### TEXT - 函数

TEXT用于函数定义，表示是汇编中的 .text 分段

```bash
TEXT pkgname·funname(SB),flag,$framesize-argsize
```

`pkgname` :可以省略，最好省略。不然修改包名还要级联修改；

`funname`: 声明的函数名

`flag`: 标志位，如 `NOSPLIT`，我们知道 Go Runtime 会追踪每个 stack 的使用情况，然后动态自增。而`NOSPLIT `标志位禁止检查，节省开销，但是写程序的人要保证这个函数是安全的。

- NOPROF = 1
  （对于TEXT项目。）不要分析标记的功能。此标志已弃用。
- DUPOK = 2
  在单个二进制文件中具有此符号的多个实例是合法的。链接器将选择要使用的重复项之一。
- NOSPLIT = 4
  （对于TEXT项。）请勿插入序言以检查是否必须拆分堆栈。例程的框架及其所调用的任何内容都必须适合堆栈段顶部的备用空间。用于保护例程，例如堆栈拆分代码本身。
- RODATA = 8
  （对于DATA和GLOBL项目。）将此数据放在只读部分中。
- NOPTR = 16
  （对于DATA和GLOBL项目。）此数据不包含任何指针，因此不需要由垃圾收集器进行扫描。
  包裹= 32
  （对于TEXT项目。）这是包装函数，不应视为禁用恢复。
- NEEDCTXT = 64
  （对于TEXT项目。）此函数是一个闭包，因此它使用其传入的上下文寄存器。

`framesize`: 函数栈帧大小 = 局部变量 + 调用其它函数参数空间的总大小

`argsize`: 一些参考资料说这里是 **参数+返回值**大小

```bash
TEXT runtime·profileloop(SB),NOSPLIT,$8  # $8 指运行时占8字节空间(栈帧大小)
	MOVQ	$runtime·profileloop1(SB), CX
	MOVQ	CX, 0(SP)
	CALL	runtime·externalthreadhandler(SB)
	RET
```

上边整段汇编代码称为一个`TEXT block` ，`runtime.profileloop(SB)`后边有一个`NOSPLIT` flag，紧随其后的`$8`表示`frame size` 

> 通常 `frame size` 的构成都是形如`$24-8` (中间的`-`只起到分隔的作用)，表示的是这个`TEXT block` 运行的时候需要占用 24 字节空间，参数和返回值要额外占用 8 字节空间（这 8 字节占用的是调用方栈帧里的空间）

## 常见操作指令

指令有几大类，**一类是用于数据移动的**，**一类是用于跳转的**（无条件跳转，有条件跳转）。**一类是用于逻辑运算和算术运算**。

指令后缀<mark>Q</mark>说明是64位上的汇编指令

[指令查询](https://github.com/golang/arch/blob/master/x86/x86.csv)

### MOVQ - 搬运长度

`MOV` 指令：其后缀表示搬运长度, `$NUM` 表示具体的数字，如下面例子

```go
MOVB $1, DI      	// 1 byte,  DI=1
MOVW $0x10, BX   	// 2 bytes, BX=10
MOVD $1, DX      	// 4 bytes, DX=1
MOVQ $-10, AX     // 8 bytes, AX=-10
```

MOV 指令有有好几种后缀 MOVB MOVW MOVL MOVQ 分别对应的是 1 字节 、2 字节 、4 字节、8 字节

### LEAQ - 将有效地址加载到指定的地址寄存器中

```go
// ret+24(FP) 这代表了第三个函数参数，是个地址
LEAQ	ret+24(FP), AX	// 把 ret+24(FP) 地址移到 AX 寄存器中
```

### ADDQ - 减法

`ADD` 加法计算指令

```bash
ADDQ  AX, BX   // BX += AX
# 栈空间: 高地址向低地址
ADDQ $0x18, SP // 对 SP 做加法，清除函数栈帧
```

### SUBQ - 减法

`SUB`减法计算指令

```bash
SUBQ  AX, BX   # BX -= AX
# 栈空间: 高地址向低地址
SUBQ $0x18, SP # 对 SP 做减法，为函数分配函数栈帧
```

### IMULQ - 无符号乘法

```bash
IMULQ AX, BX   # BX *= AX
```

### IDIVQ - 无符号除法

### CMP - 比较

### AND，OR，XOR - 位运算

### JMP - 无条件跳转

`JMP` 指令**无条件**跳转到目标地址，该地址用代码标号来标识，并被汇编器转换为偏移量

```bash
JMP addr   // 跳转到地址，地址可为代码中的地址，不过实际上手写不会出现这种东西
JMP label  // 跳转到标签，可以跳转到同一函数内的标签位置
JMP 2(PC)  // 以当前指令为基础，向前/后跳转 x 行
JMP -2(PC) // 同上
```

### JZ，JLS - 有条件跳转

```bash
# 有条件跳转
JZ target // 如果 zero flag 被 set 过，则跳转
JLS num		// 如果上一行的比较结果，左边小于右边则执行跳到 num 地址处
```

### CALL - 调用函数

```bash
CALL runtime.printnl(SB)
```

### RET - 返回

return

### leave - 恢复调用者者栈指针

## 寻址模式

汇编语言的一个很重要的概念就是它的寻址模式，Plan 9 汇编也不例外，它支持如下寻址模式：

```bash
R0              数据寄存器
A0              地址寄存器
F0              浮点寄存器
CAAR, CACR, 等  特殊名字
$con            常量
$fcon           浮点数常量
name+o(SB)      外部符号
name<>+o(SB)    局部符号
name+o(SP)      自动符号
name+o(FP)      实际参数
$name+o(SB)     外部地址
$name<>+o(SB)   局部地址
(A0)+           间接后增量
-(A0)           间接前增量
o(A0)
o()(R0.s)

symbol+offset(SP) 引用函数的局部变量，offset 的合法取值是 [-framesize, 0)
    局部变量都是 8 字节，那么第一个局部变量就可以用 localvar0-8(SP) 来表示

如果是 symbol+offset(SP) 形式，则表示伪寄存器 SP
如果是 offset(SP) 则表示硬件寄存器 SP
```

## 标志位

| 助记符 | 名字     | 用途                                                  |
| :----- | :------- | :---------------------------------------------------- |
| OF     | 溢出     | 0为无溢出 1为溢出                                     |
| CF     | 进位     | 0为最高位无进位或错位 1为有                           |
| PF     | 奇偶     | 0表示数据最低8位中1的个数为奇数，1则表示1的个数为偶数 |
| AF     | 辅助进位 |                                                       |
| ZF     | 零       | 0表示结果不为0 1表示结果为0                           |
| SF     | 符号     | 0表示最高位为0 1表示最高位为1                         |

<div STYLE="page-break-after: always;"></div>

# Go常用命令

## go build

```bash
-a	将命令源码文件与库源码文件全部重新构建，即使是最新的
-n	把编译期间涉及的命令全部打印出来，但不会真的执行，非常方便我们学习
-race	开启竞态条件的检测，支持的平台有限制
-x	打印编译期间用到的命名，它与 -n 的区别是，它不仅打印还会执行
-gcflags '[pattern=]arg list'
    传递go 工具编译调用的参数
-ldflags '[pattern=]arg list'
    传递go 工具链接调用的参数
```

-gcflags 、-ldflags 可以加`-help`参数查看详情

- `go build -gcflags -help`

<div STYLE="page-break-after: always;"></div>

# 事件说明

- 【2022-10-22】封面土拨鼠图片来源于谷歌搜索

<div STYLE="page-break-after: always;"></div>

# 参考

- 挖坑的张师傅、小贺coding
- [欧长坤](https://changkun.de/)
- [小米信息部技术团队](https://xiaomi-info.github.io/)
- [煎鱼](https://eddycjy.gitbook.io/)
