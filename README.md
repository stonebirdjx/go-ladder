<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [前言](#%E5%89%8D%E8%A8%80)
- [:point_right:小刀弑牛](#point_right%E5%B0%8F%E5%88%80%E5%BC%91%E7%89%9B)
- [:point_right:helloworld](#point_righthelloworld)
- [语言](#%E8%AF%AD%E8%A8%80)
  - [:point_right:package - 包](#point_rightpackage---%E5%8C%85)
  - [**:point_right:各种类型的内存长度和零值**](#point_right%E5%90%84%E7%A7%8D%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%86%85%E5%AD%98%E9%95%BF%E5%BA%A6%E5%92%8C%E9%9B%B6%E5%80%BC)
  - [:point_right:变量](#point_right%E5%8F%98%E9%87%8F)
  - [:point_right:常量和iota](#point_right%E5%B8%B8%E9%87%8F%E5%92%8Ciota)
- [安全编码规范](#%E5%AE%89%E5%85%A8%E7%BC%96%E7%A0%81%E8%A7%84%E8%8C%83)
- [算法达人](#%E7%AE%97%E6%B3%95%E8%BE%BE%E4%BA%BA)
- [原理透析](#%E5%8E%9F%E7%90%86%E9%80%8F%E6%9E%90)
- [面试题记](#%E9%9D%A2%E8%AF%95%E9%A2%98%E8%AE%B0)
- [徐徐前行](#%E5%BE%90%E5%BE%90%E5%89%8D%E8%A1%8C)
- [参考资料](#%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 前言

:speak_no_evil:如果能成为golang的cookbook就好了:see_no_evil:

整理知识，传播智慧，好好学习，天天向上。

> 文档为个人平时生活收集和工作积累，不涉及商用，仅供学习和参考。请尊重信息、尊重开发者、勿在商业模式下传播。

# :point_right:小刀弑牛

[安装golang](https://go.dev/doc/install)

[官方入门地址](https://tour.go-zh.org/welcome/1)

> 如果你是没有其他语言基础的初学者，建议学习2-3遍。

要求了解go语言的语法结构、数据类型、变量、常量、运算符、条件分支、循环语句、函数、作用域、结构体、指针、数组、切片、map、接口、并发等。达到熟练使用的地步。

# :point_right:helloworld

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, 世界")
}
```

完成并运行上面代码，你现在已经是一个golang程序员 ( `gopher` ) 了，需要知道

- 程序从 `main` 包的 `main` 函数开始运行。
- 函数体外每个语句应该以关键字开始，如：`var`、`func`、`type`、`const`、`import`、`package`

> 关于run背后的流程，学习原理时再掌握

# 语言

## :point_right:package - 包

使用package关键字定义包

包名原则上只由小写字面和数字组成。不包含大写字母和下划线等字符（测试包除外）

```go
package zip
package sha256
package sha256_test
```

一般包名和目录保持一致

```go
// 目录名：port-obj
// 对应的包名：portobj
package portobj
```

包内以大写字母开头(函数、变量、常量、结构体...)，那么它就是已导出(外部可调用)。

```go
package stonebird

var name = "stone bird"
var Total = 10  // 禁止导出全局变量

func GetName() string {
    return name
}
```

- 注意包命名规范
- 在导入一个包时，你只能引用其中已导出的名字。任何**未导出**的名字在该包外均无法访问
- 由于全局变量可修改直接导出并不安全，一般情况下禁止导出全局变量。如需包的全局变量可使用 **常量** 或者 **非导出符号+导出函数的形式**

## **:point_right:各种类型的内存长度和零值**

| 类型          | 内存长度 | 零值           | 说明                                                       |
| ------------- | -------- | -------------- | ---------------------------------------------------------- |
| bool          | 1        | false          |                                                            |
| byte          | 1        | 0              | 对应uint8                                                  |
| rune          | 4        | 0              | 对应int32                                                  |
| int，uint     | 4或8     | 0              | 32位内存长度为4，对应int32。64位系统内存长度为8，对应int64 |
| int8、uint8   | 1        | 0              | 取值范围：-128-127, 0~255                                  |
| int16、uint16 | 2        | 0              | 取值范围：-32768 ~ 32767, 0 ~ 65535                        |
| int32、uint32 | 4        | 0              | 取值范围：-(2^31) ~ (2^31) - 1 ， 0 ~ (2^32) - 1           |
| int64、uint64 | 8        | 0              | 取值范围：-(2^63) ~ (2^63) - 1， 0 ~ (2^64) - 1            |
| float32       | 4        | 0              | 浮点数                                                     |
| float64       | 8        | 0              | 浮点数                                                     |
| complex64     | 8        | (0+0i)         | 复数                                                       |
| complex128    | 16       | (0+0i)         | 复数                                                       |
| uintptr       | 4或8     | 0              | 存储指针的 uint32 或 uint64 整数                           |
| array         |          | 值类型的默认值 | 值类型的默认值                                             |
| struct        |          | 值类型的默认值 | 值类型的默认值                                             |
| string        |          | ""             | utf-8字符串                                                |
| slice         |          | nil            | 引用类型                                                   |
| map           |          | nil            | 引用类型                                                   |
| channel       |          | nil            | 引用类型                                                   |
| interface     |          | nil            | 接口                                                       |
| function      |          | nil            | 函数                                                       |

**默认值（零值）说明**

- 数字类型 `int、uint、float32 、float64` 的零值为 0
- 布尔类型 `bool` 的零值为`false`
- 字符串 `string`  的零值为 `""`
- 数组(array)和结构体零值为元素的值类型
- 其他引用类型（指针、切片、映射、通道、函数和接口）的零值则是 nil

> 内存长度目前了解即可，在结构体内存对齐时可以详细了解

## :point_right:变量

使用var关键字定义变量，如果提供初始化值，可省略变量类型，由编译器自动推断。

```go
var x, y, z int
var s, n = "abc", 123
var (
    a int
    b float32
)

func main() {
    n, s := 0x1234, "Hello, World!"
    x, _, z := something()
    println(x, s, n)
}
```

**变量说明**

- 使用`:=` 语法糖精简赋值，只能在函数内部使用
- 特殊只写变量 `_` ,用于忽略值，起占位作用
- 如果局部变量未使用编译期会报错。全局变量未使用不会报错
- 全局变量禁止其他包修改，只能小写

## :point_right:常量和iota

常量使用const关键字定义，常量值必须是编译期可确定的数字、字符串、布尔值，还可以是 len、cap、unsafe.Sizeof 等编译期可确定结果的函数返回值。

```go
// 多常量初始化
const x, y int = 1, 2 

// 类型推断
const s = "Hello, World!" 
const b = false 

const (
    l  = "stonebird"
    ll = len(l)
    lc = cap([]byte(l))
    ls = unsafe.Sizeof(l)
)
```

在常量组中，如不提供类型和初始化值，那么视作与上一常量相同。

```go
const (
	s = "abc"
	x 			// x = "abc"
)

// 数组通过索引定义的时候也是这个道理
```

关键字 iota 定义常量组中从 0 开始按行计数的自增枚举值。

简单的说iota是const常量组的索引，在n行，值为n-1

```go
const (
    Sunday = iota // 0
    Monday // 1，通常省略后续⾏表达式。
    Tuesday // 2
    Wednesday // 3
    Thursday // 4
    Friday // 5
    Saturday // 6
)

const (
    _ = iota // iota = 0
    KB int64 = 1 << (10 * iota) // iota = 1
    MB 							// 与 KB 表达式相同，iota = 2
    GB
    TB
)

// 可以提供多个 iota，它们各自增⻓
const (
    A, B = iota, iota << 10 	// 0, 0 << 10
    C, D 						// 1, 1 << 10
)
```

如果iota自增被打断，则必须显式恢复

```go
const (
    A = iota 	// 0
    B 			// 1
    C = "c" 	// c 
    D 			// c，与上⼀⾏相同。
    E = iota 	// 4，显式恢复。注意计数包含了 C、D 两⾏。
	F 			// 5
)
```

**常量说明**

- 常量是编译期可确定的值，所以最好不用函数，除非函数对常量计算，能返回可确定结果。
- iota在常量组中等同于数组的索引
- 开发过程中尽量不要使用iota



# 安全编码规范

入门一面语言后，我们要让自己的代码显得更安全、简洁、规范、高效。让自己显得更加的pro。

下面是我的总结，不够全面，仅供参考

[golang安全编码规范](https://github.com/stonebirdjx/go-ladder/blob/master/golang%E5%AE%89%E5%85%A8%E7%BC%96%E7%A0%81%E8%A7%84%E8%8C%83.md)

# 算法达人

# 原理透析

# 面试题记

# 徐徐前行

现在你已经是优秀的go语言开发者了，请继续追踪golang社区，了解版本新特性。容器编排和AI算法是先进的方向，新的旅程如下。

[k8s-ladder:每个人都是k8s大师](https://github.com/stonebirdjx/k8s-ladder)

[python-ladder:每个人都是python大师](https://github.com/stonebirdjx/go-ladder/blob/master/gosafe.md)

# 参考资料

强者总是比较孤独,请坚持学习！送上一句黑鸡汤：

> "The last thing you want is to look back on your life and wonder... if only." --by caicloud
