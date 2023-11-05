<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [结构体定义](#%E7%BB%93%E6%9E%84%E4%BD%93%E5%AE%9A%E4%B9%89)
- [声明字符串](#%E5%A3%B0%E6%98%8E%E5%AD%97%E7%AC%A6%E4%B8%B2)
- ["+"拼接](#%E6%8B%BC%E6%8E%A5)
- [字符串截取](#%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%88%AA%E5%8F%96)
- [类型转换](#%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
  - [[]byte2string](#byte2string)
  - [string2[]byte](#string2byte)
  - [[]rune2string](#rune2string)
  - [string2[]rune](#string2rune)
- [字符串拼接性能](#%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%8B%BC%E6%8E%A5%E6%80%A7%E8%83%BD)
  - [比较 strings.Builder 和 bytes.Buffer](#%E6%AF%94%E8%BE%83-stringsbuilder-%E5%92%8C-bytesbuffer)
- [为什么字符串不允许修改](#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%8D%E5%85%81%E8%AE%B8%E4%BF%AE%E6%94%B9)
- [[]byte转换成string一定会拷贝内存吗](#byte%E8%BD%AC%E6%8D%A2%E6%88%90string%E4%B8%80%E5%AE%9A%E4%BC%9A%E6%8B%B7%E8%B4%9D%E5%86%85%E5%AD%98%E5%90%97)
- [for遍历的两种方式](#for%E9%81%8D%E5%8E%86%E7%9A%84%E4%B8%A4%E7%A7%8D%E6%96%B9%E5%BC%8F)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

Go中的string是不可变的（只读），因此无法修改，只能使用 `len()` 获取长度，无法使用 `cap()` 获取容量，与slice有一定的相似之处，实际上两者底层都使用了一个数组保存所有的数据。

> string 存于只读段，属于常量,是所有 8 位字节字符串的集合，通常但不一定表示 UTF-8 编码的文本。

# 结构体定义

文件路径：`src/runtime/string.go#L232`

```go
type stringStruct struct {
	str unsafe.Pointer
	len int
}
```

> 无cap字段

`reflect包中也存在一个 `StringHeader` 结构体`，反射的时候使用。对比 `reflect.SliceHeader` 发现，两者有相似之处。

```go
type StringHeader struct {
	Data uintptr
	Len  int
}

type SliceHeader struct {
  Data uintptr
  Len int
  Cap int  // 故cap=len。
}
```

# 声明字符串

字符串构建过程是先根据字符串构建stringStruct，再转换成string

string在runtime包中就是stringStruct，对外呈现叫做string。

```go
var str string
str = "Hello"

// gostringnocopy
func gostringnocopy(str *byte) string {//根据字符串地址构建string
	ss := stringStruct{str: unsafe.Pointer(str), len: findnull(str)} //先构造stringStruct
	s := *(*string)(unsafe.Pointer(&ss))//再将stringStruct转换成string
	return s
}
```

# "+"拼接

Go支持通过 `+` 拼接多个string，但是需要注意的是如果拼接后的字符串长度超过`32`字节，则需要分配内存空间；反之则存放在缓冲区上。

Go 语言拼接字符串会使用 `+` 符号，编译器会将该符号对应的 `OADD` 节点转换成 `OADDSTR` 类型的节点,随后在 `cmd/compile/internal/gc.walkexpr` 中调用 `cmd/compile/internal/gc.addstr `函数生成用于拼接字符串的代码

```go
func walkexpr(n *Node, init *Nodes) *Node {
	switch n.Op {
	...
	case OADDSTR:
		n = addstr(n, init)
	}
}
```

```go
func addstr(n *Node, init *Nodes) *Node {
	c := n.List.Len()

	buf := nodnil()
	args := []*Node{buf}
	for _, n2 := range n.List.Slice() {
		args = append(args, conv(n2, types.Types[TSTRING]))
	}

	var fn string
	if c <= 5 {
		fn = fmt.Sprintf("concatstring%d", c)
	} else {
		fn = "concatstrings"

		t := types.NewSlice(types.Types[TSTRING])
		slice := nod(OCOMPLIT, nil, typenod(t))
		slice.List.Set(args[1:])
		args = []*Node{buf, slice}
	}

	cat := syslook(fn)
	r := nod(OCALL, cat, nil)
	r.List.Set(args)
	...
	return r
}
```

- 如果小于或者等于 5 个，那么会调用 `concatstring{2,3,4,5}` 等一系列函数；
- 如果超过 5 个，那么会选择 `runtime.concatstrings`传入一个数组切片

concatstrings 实现 Go 字符串连接 x+y+z+...

```go
// The constant is known to the compiler.
// There is no fundamental theory behind this number.
const tmpStringBufSize = 32

type tmpBuf [tmpStringBufSize]byte

// concatstrings implements a Go string concatenation x+y+z+...
// The operands are passed in the slice a.
// If buf != nil, the compiler has determined that the result does not
// escape the calling function, so the string data can be stored in buf
// if small enough.
func concatstrings(buf *tmpBuf, a []string) string {
	idx := 0
	l := 0
	count := 0
	for i, x := range a {
		n := len(x)
		if n == 0 {
			continue
		}
		if l+n < l {
			throw("string concatenation too long")
		}
		l += n
		count++
		idx = i
	}
	if count == 0 {
		return ""
	}

	// If there is just one string and either it is not on the stack
	// or our result does not escape the calling frame (buf != nil),
	// then we can return that string directly.
	if count == 1 && (buf != nil || !stringDataOnStack(a[idx])) {
		return a[idx]
	}
  
  // 分配一定的空间存放结果
	s, b := rawstringtmp(buf, l)
	for _, x := range a {
		copy(b, x)
		b = b[len(x):]
	}
	return s
}
```

rawstringtmp

该函数的主要作用分配空间以便存放拼接后的结果，当然，如果缓冲区不为nil且结果字符串长度小于等于32，则直接保存到缓冲区中

```go
func rawstringtmp(buf *tmpBuf, l int) (s string, b []byte) {
  // 将拼接后的结果保存到栈上的缓冲区中
	if buf != nil && l <= len(buf) {
		b = buf[:l]
		s = slicebytetostringtmp(&b[0], len(b))
	} else {
    // 调用 mallocgc 分配内存空间以便存放结果
		s, b = rawstring(l)
	}
	return
}
```

# 字符串截取

默认是先转成[]byte，从[]byte中再截取子串。unicode编码记得先使用rune

`newstring :=oldstring[low:hight]`

- 0<=low<=hight<=len(oldstring)
- low 默认为0
- hight默认为len(oldstring)

```go
str := "XHelloWorldX"
content := str[1 : len(str)-1]
fmt.Println(content)
// 中文
str := "a中文cd"
str = string([]rune(str)[:4])
fmt.Println(str)
```

# 类型转换

## []byte2string

`strings([]byte)` 会用到`runtime.slicebytetostring`

文件路径：`src/runtime/string.go`

```go
func slicebytetostring(buf *tmpBuf, ptr *byte, n int) string {
	if n == 0 {
		// Turns out to be a relatively common case.
		// Consider that you want to parse out data between parens in "foo()bar",
		// you find the indices and convert the subslice to string.
		return ""
	}
	if raceenabled {
		racereadrangepc(unsafe.Pointer(ptr),
			uintptr(n),
			getcallerpc(),
			abi.FuncPCABIInternal(slicebytetostring))
	}
	if msanenabled {
		msanread(unsafe.Pointer(ptr), uintptr(n))
	}
	if asanenabled {
		asanread(unsafe.Pointer(ptr), uintptr(n))
	}
	if n == 1 {
		p := unsafe.Pointer(&staticuint64s[*ptr])
		if goarch.BigEndian {
			p = add(p, 7)
		}
		return unsafe.String((*byte)(p), 1)
	}

	var p unsafe.Pointer
	if buf != nil && n <= len(buf) {
		p = unsafe.Pointer(buf)
	} else {
		p = mallocgc(uintptr(n), nil, false)
	}
	memmove(p, unsafe.Pointer(ptr), uintptr(n))
	return unsafe.String((*byte)(p), n)
}
```

## string2[]byte

`[]byte(string)` 会用到`runtime.slicebytetostring`

文件路径：`src/runtime/string.go`

```go
func stringtoslicebyte(buf *tmpBuf, s string) []byte {
	var b []byte
	if buf != nil && len(s) <= len(buf) {
		*buf = tmpBuf{}
		b = buf[:len(s)]
	} else {
		b = rawbyteslice(len(s))
	}
	copy(b, s)
	return b
}
```

## []rune2string

`string([]rune)`会用到`runtime.slicerunetostring`

文件路径：`src/runtime/string.go`

```go
func slicerunetostring(buf *tmpBuf, a []rune) string {
	if raceenabled && len(a) > 0 {
		racereadrangepc(unsafe.Pointer(&a[0]),
			uintptr(len(a))*unsafe.Sizeof(a[0]),
			getcallerpc(),
			abi.FuncPCABIInternal(slicerunetostring))
	}
	if msanenabled && len(a) > 0 {
		msanread(unsafe.Pointer(&a[0]), uintptr(len(a))*unsafe.Sizeof(a[0]))
	}
	if asanenabled && len(a) > 0 {
		asanread(unsafe.Pointer(&a[0]), uintptr(len(a))*unsafe.Sizeof(a[0]))
	}
	var dum [4]byte
	size1 := 0
	for _, r := range a {
		size1 += encoderune(dum[:], r)
	}
	s, b := rawstringtmp(buf, size1+3)
	size2 := 0
	for _, r := range a {
		// check for race
		if size2 >= size1 {
			break
		}
		size2 += encoderune(b[size2:], r)
	}
	return s[:size2]
}
```

## string2[]rune

`[]rune(string)`会用到` runtime.stringtoslicerune`

文件路径：`src/runtime/string.go`

```go
func stringtoslicerune(buf *[tmpStringBufSize]rune, s string) []rune {
	// two passes.
	// unlike slicerunetostring, no race because strings are immutable.
	n := 0
	for range s {
		n++
	}

	var a []rune
	if buf != nil && n <= len(buf) {
		*buf = [tmpStringBufSize]rune{}
		a = buf[:n]
	} else {
		a = rawruneslice(n)
	}

	n = 0
	for _, r := range s {
		a[n] = r
		n++
	}
	return a
}
```

# 字符串拼接性能

## 比较 strings.Builder 和 bytes.Buffer

`strings.Builder` 和 `bytes.Buffer` 底层都是 `[]byte` 数组，但 `strings.Builder` 性能比 `bytes.Buffer` 略快约 10% 。一个比较重要的区别在于，`bytes.Buffer` 转化为字符串时重新申请了一块空间，存放生成的字符串变量，而 `strings.Builder` 直接将底层的 `[]byte` 转换成了字符串类型返回了回来。

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

# 为什么字符串不允许修改

string通常指向字符串字面量，而字符串字面量存储存储位置是只读段，而不是堆或栈上，所以string不可修改。

# []byte转换成string一定会拷贝内存吗

赋值情况下使用unsafe包可以不内存拷贝

```go
func main() {
    a :="aaa"
    ssh := *(*reflect.StringHeader)(unsafe.Pointer(&a))
    b := *(*[]byte)(unsafe.Pointer(&ssh))  
    fmt.Printf("%v",b)
}
```

不赋值（临时使用的情况下）不会内存拷贝

- 字符串拼接，如"<" + "string(b)" + ">"；
- 字符串比较：string(b) == "foo"

因为是临时把byte切片转换成string，也就避免了因byte切片同容改成而导致string引用失败的情况，所以此时可以不必拷贝内存新建一个string。

> string 转[]byte 也是同理

# for遍历的两种方式

使用for循环遍历字符串时，也有 byte 和 rune 两种方式。

要修改字符串，可先将其转换成[]rune 或 []byte，完成后再转换为 string。无论哪种转换，都会重新分配内存，并复制字节数组。

```go
func main() {
	s := "abcef"
	for i := 0; i < len(s); i++ { // byte
		fmt.Printf("%c,", s[i])
	}
  
	fmt.Println()
	for _, r := range s { // rune
		fmt.Printf("%c,", r)
	}
}
```

