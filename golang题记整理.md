<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [1. 下面这段代码输出的内容](#1-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E7%9A%84%E5%86%85%E5%AE%B9)
- [2. 下面这段代码输出什么，说明原因。](#2-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E8%AF%B4%E6%98%8E%E5%8E%9F%E5%9B%A0)
- [3. 下面两段代码输出什么？](#3-%E4%B8%8B%E9%9D%A2%E4%B8%A4%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [4. 下面这段代码有什么缺陷？](#4-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E7%BC%BA%E9%99%B7)
- [5. new() 与 make() 的区别](#5-new-%E4%B8%8E-make-%E7%9A%84%E5%8C%BA%E5%88%AB)
- [6. 下面这段代码能否通过编译?](#6-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E9%80%9A%E8%BF%87%E7%BC%96%E8%AF%91)
- [7. 下面这段代码能否通过编译？](#7-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E9%80%9A%E8%BF%87%E7%BC%96%E8%AF%91)
- [8. 下面这段代码能否通过编译？](#8-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E9%80%9A%E8%BF%87%E7%BC%96%E8%AF%91)
- [9. 下面这段代码能否通过编译？](#9-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E9%80%9A%E8%BF%87%E7%BC%96%E8%AF%91)
- [10. 通过指针变量p访问其成员变量name,有哪几种方式？](#10-%E9%80%9A%E8%BF%87%E6%8C%87%E9%92%88%E5%8F%98%E9%87%8Fp%E8%AE%BF%E9%97%AE%E5%85%B6%E6%88%90%E5%91%98%E5%8F%98%E9%87%8Fname%E6%9C%89%E5%93%AA%E5%87%A0%E7%A7%8D%E6%96%B9%E5%BC%8F)
- [11. 下面这段代码能否通过编译?](#11-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E9%80%9A%E8%BF%87%E7%BC%96%E8%AF%91)
- [12. 以下代码输出什么？](#12-%E4%BB%A5%E4%B8%8B%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 1. 下面这段代码输出的内容

```go
package main

import "fmt"

func main() {
    defer_call()
}

func defer_call()  {
    defer func() {fmt.Println("打印前")}()
    defer func() {fmt.Println("打印中")}()
    defer func() {fmt.Println("打印后")}()

    panic("触发异常")
}
```

Output:

```bash
打印后
打印中
打印前
panic: 触发异常
```

<font color="red"> `defer` 的执行顺序是先进后出。出现panic语句的时候，会先按照 `defer` 的后进先出顺序执行，最后才会执行panic。</font>

# 2. 下面这段代码输出什么，说明原因。

```go
package main

import "fmt"

func main() {
	slice := []int{0, 1, 2, 3}
	m := make(map[int]*int)

	for key, val := range slice {
		m[key] = &val // val的地址
	}

	for k, v := range m {
		fmt.Println(k, "->", *v)
	}
}
```

Output:

```bash
# 注：key的顺序无法确定
0 -> 3
1 -> 3
2 -> 3
3 -> 3
```

`for range` 循环的时候会创建每个元素的副本，而不是每个元素的引用，所以 `m[key] = &val` 取的都是变量val的地址，所以最后 `map` 中的所有元素的值都是变量 `val` 的地址，因为最后 `val` 被赋值为3，所有输出的都是3。

<font color="red">注意取得是&地址</font>

# 3. 下面两段代码输出什么？

```go
// 1.
func main() {
    s := make([]int, 5)
    s = append(s, 1, 2, 3)
    fmt.Println(s)
}

// 2.
func main() {
    s := make([]int, 0)
    s = append(s, 1, 2, 3, 4)
    fmt.Println(s)
}
```

Output:

```bash
# 1.
[0 0 0 0 0 1 2 3]

# 2.
[1 2 3 4]
```

使用 `append` 向 `slice` 中添加元素，第一题中slice容量为5，所以补5个0，第二题为0，所以不需要。

<font color="red">注意零值</font>

# 4. 下面这段代码有什么缺陷？

```go
func funcMui(x, y int) (sum int, error) {
    return x + y, nil
}
```

在函数有多个返回值时，只要有一个返回值有命名，其他的也必须命名。如果有多个返回值必须加上括号();如果只有一个返回值且命名也需要加上括号()。这里的第一个返回值有命名sum，第二个没有命名，所以错误。

<font color="red">注意返回值要么都匿名，要么都具名，开发中尽量少用具名返回</font>

# 5. new() 与 make() 的区别

`new(T)` 和 `make(T, args)` 是Go语言内建函数，用来分配内存，但适用的类型不用。

```go
func new(Type) *Type
func make(t Type, size ...IntegerType) Type
```

- `new(T)` 会为了 `T` 类型的新值分配已置零的内存空间，并返回地址（指针），即类型为 `*T` 的值。换句话说就是，返回一个指针，该指针指向新分配的、类型为 `T` 的零值。适用于值类型，如 `数组` 、 `结构体` 等。
- `make(T, args)` 返回初始化之后的T类型的值，也不是指针 `*T` ，是经过初始化之后的T的引用。 `make()` 只适用于 `slice` 、 `map` 和 `channel` 。

<font color="red">new()适用于值类型，分配已置零的内存空间，返回指针。make()适用于引用类型，申请了空间大小，返回的不是指针</font>。

> 如果用new初始化引用类型，注意要重新申请一下内存

# 6. 下面这段代码能否通过编译?

```go
func main() {
    list := new([]int) 
    list = append(list, 1)
    fmt.Println(list)
}
```

不能通过编译， `new([]int)` 之后的 `list` 是一个 `*int[]` 类型的指针，不能对指针执行 `append` 操作。可以使用 `make()` 初始化之后再用。同样的， `map` 和 `channel` 建议使用 `make()` 或字面量的方式初始化，不要用 `new` 。

用new需要改成以下

```go
func main() {
	list := new([]int)
	*list = make([]int, 0)
	*list = append(*list, 1)
	fmt.Println(*list)
}
```

<font color="red">append 第一个参数也不能是地址</font>

# 7. 下面这段代码能否通过编译？

```go
func main() {
    s1 := []int{1, 2, 3}
    s2 := []int{4, 5}
    s1 = append(s1, s2)
    fmt.Println(s1)
}
```

不能通过编译，`append()` 的第二个参数不能直接使用 `slice` ，需使用 `...` 操作符，将一个切片追加到另一个切片上： `append(s1, s2...)` 。或者直接跟上元素，形如： `append(s1, 1, 2, 3)` 。

<font color="red">append 切片（相同类型，不能是数组），需要用到语法糖</font>

#  8. 下面这段代码能否通过编译？

```go
var (
    size := 1024 // 函数外必须使用显示初始化
    max_size = size * 2 // 
)

func main() {
    fmt.Println(size, max_size)
}
```

不能通过编译，函数外必须使用显示初始化

<font color="red">`:=简短模式只能在函数内部使用`</font>

# 9. 下面这段代码能否通过编译？

```go
package main

import "fmt"

func main() {
	sn1 := struct {
		age  int
		name string
	}{age: 11, name: "qq"}
	sn2 := struct {
		age  int
		name string
	}{age: 11, name: "11"}

	if sn1 == sn2 {
		fmt.Println("sn1 == sn2")
	}

	sm1 := struct {
		age int
		m   map[string]string
	}{age: 11, m: map[string]string{"a": "1"}}
	sm2 := struct {
		age int
		m   map[string]string
	}{age: 11, m: map[string]string{"a": "1"}}

	if sm1 == sm2 {
		fmt.Println("sm1 == sm2")
	}
}
```

不能通过编译，结构体比较如下

1. 结构体只能比较是否相等，但是不能比较大小；
2. 想同类型的结构体才能进行比较，结构体是否相同不但与属性类型有关，还与属性顺序相关；
3. 如果struct的所有成员都可以比较，则该struct就可以通过==或!=进行比较是否相同，比较时逐个项进行比较，如果每一项都相等，则两个结构体才相等，否则不相等；

<font color="red">值类型的能比较：bool、数值型、字符、指针、数组等<br>引用类型不能比较：slice、map、函数</font>

#  10. 通过指针变量p访问其成员变量name,有哪几种方式？

```bash
A. p.name
B. (&p).name
C. (*p).name
D. p->name
```

A C   `&` 取址运算符， `*` 指针解引用

# 11. 下面这段代码能否通过编译?

```go
package main

import "fmt"

type MyInt1 int
type MyInt2 = int

func main() {
	var i int = 0
	var i1 MyInt1 = i // var i1 MyInt1 = MyInt1(i)
	var i2 MyInt2 = i
	fmt.Println(i1, i2)
}
```

不能通过编译，Go是强类型语言.

# 12. 以下代码输出什么？

```go
func main() {
    a := []int{7, 8, 9}
    fmt.Printf("%+v\n", a)
    ap(a)
    fmt.Printf("%+v\n", a)
    app(a)
    fmt.Printf("%+v\n", a)
}

func ap(a []int) {
    a = append(a, 10) // append 导致a的地址变了
}

func app(a []int) {
    a[0] = 1
}
```

Output：

```bash
[7 8 9]
[7 8 9]
[1 8 9]
```

因为append导致底层数组重新分配内存了，append中的a这个alice的底层数组和外面不是一个，并没有改变外面的。

