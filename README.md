<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [前言](#%E5%89%8D%E8%A8%80)
- [小刀弑牛](#%E5%B0%8F%E5%88%80%E5%BC%91%E7%89%9B)
- [helloworld](#helloworld)
  - [:point_right:程序总结](#point_right%E7%A8%8B%E5%BA%8F%E6%80%BB%E7%BB%93)
- [package-包](#package-%E5%8C%85)
  - [包声明](#%E5%8C%85%E5%A3%B0%E6%98%8E)
  - [导出符号](#%E5%AF%BC%E5%87%BA%E7%AC%A6%E5%8F%B7)
  - [init函数 - 初始化包](#init%E5%87%BD%E6%95%B0---%E5%88%9D%E5%A7%8B%E5%8C%96%E5%8C%85)
  - [内部包](#%E5%86%85%E9%83%A8%E5%8C%85)
  - [导入包](#%E5%AF%BC%E5%85%A5%E5%8C%85)
  - [module - 模块](#module---%E6%A8%A1%E5%9D%97)
  - [go work - 工作空间](#go-work---%E5%B7%A5%E4%BD%9C%E7%A9%BA%E9%97%B4)
  - [go vendor - 打包依赖源码](#go-vendor---%E6%89%93%E5%8C%85%E4%BE%9D%E8%B5%96%E6%BA%90%E7%A0%81)
  - [包相关的环境变量](#%E5%8C%85%E7%9B%B8%E5%85%B3%E7%9A%84%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
  - [:point_right:包总结](#point_right%E5%8C%85%E6%80%BB%E7%BB%93)
- [变量](#%E5%8F%98%E9%87%8F)
  - [变量声明](#%E5%8F%98%E9%87%8F%E5%A3%B0%E6%98%8E)
  - [:point_right:变量总结](#point_right%E5%8F%98%E9%87%8F%E6%80%BB%E7%BB%93)
- [程序结构](#%E7%A8%8B%E5%BA%8F%E7%BB%93%E6%9E%84)
  - [**:point_right:各种类型的内存长度和零值**](#point_right%E5%90%84%E7%A7%8D%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%86%85%E5%AD%98%E9%95%BF%E5%BA%A6%E5%92%8C%E9%9B%B6%E5%80%BC)
  - [:point_right:常量和iota](#point_right%E5%B8%B8%E9%87%8F%E5%92%8Ciota)
  - [:point_right:作用域](#point_right%E4%BD%9C%E7%94%A8%E5%9F%9F)
- [类型](#%E7%B1%BB%E5%9E%8B)
  - [基本数据类型](#%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
    - [:point_right:整型](#point_right%E6%95%B4%E5%9E%8B)
    - [:point_right:浮点型](#point_right%E6%B5%AE%E7%82%B9%E5%9E%8B)
    - [复数](#%E5%A4%8D%E6%95%B0)
    - [:point_right:布尔型](#point_right%E5%B8%83%E5%B0%94%E5%9E%8B)
  - [:point_right:字符串](#point_right%E5%AD%97%E7%AC%A6%E4%B8%B2)
  - [:point_right:数组](#point_right%E6%95%B0%E7%BB%84)
  - [:point_right:结构体](#point_right%E7%BB%93%E6%9E%84%E4%BD%93)
    - [:point_right:引用类型](#point_right%E5%BC%95%E7%94%A8%E7%B1%BB%E5%9E%8B)
    - [:point_right:类型转换](#point_right%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
    - [:point_right:自定义类型和别名](#point_right%E8%87%AA%E5%AE%9A%E4%B9%89%E7%B1%BB%E5%9E%8B%E5%92%8C%E5%88%AB%E5%90%8D)
    - [:point_right:类型比较](#point_right%E7%B1%BB%E5%9E%8B%E6%AF%94%E8%BE%83)
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

对于一些规则或者要求，可以参考[安全编码规范](https://github.com/stonebirdjx/go-ladder/blob/master/golang%E5%AE%89%E5%85%A8%E7%BC%96%E7%A0%81%E8%A7%84%E8%8C%83.md)

> 文档为个人平时生活收集和工作积累，不涉及商用，仅供学习和参考。请尊重信息、尊重开发者、勿在商业模式下传播。

# 小刀弑牛

[安装golang](https://go.dev/doc/install)

[官方入门地址](https://tour.go-zh.org/welcome/1)

> 如果你是没有其他语言基础的初学者，建议学习2-3遍。

# helloworld

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, world")
	fmt.Println("Hello, 世界")
}
```

完成并运行上面代码，你现在已经是一个golang程序员 ( `gopher` ) 了

## :point_right:程序总结

- 程序从 `main` 包的 `main` 函数开始运行。
- 函数体外每个语句应该以关键字开始，如：`var`、`func`、`type`、`const`、`import`、`package`

> 关于run背后的流程，学习原理时再掌握

# package-包

## 包声明

使用package关键字定义包，包由同一目录下的多个源文件构成。系统设计包的目的都是为了简化大型程序的设计和维护工作。

尽可能让命名有描述性且无歧义。

包名一般采用单数的形式。标准库的bytes、errors和strings使用了复数形式只是为了防止和关键字冲突。

包名原则上只由小写字面和数字组成。不包含大写字母和下划线等字符（测试包除外）。

```go
package zip
package sha256
package sha256_test
```

包名通常与目录名一致

```go
// 目录名：port-obj
// 对应的包名：portobj
package portobj
```

## 导出符号

包是成员作用域边界，包内成员可相互访问。 名称首字母大写为 导出成员（exported），可外部访问。

此规则适用于全局变量、全局常量、函数、类型、字段和方法等。

```go
package stonebird

var name = "stone bird"
var Total = 10  // 禁止导出全局变量

func GetName() string {
    return name
}
```

> 成员命名，避免使用包名为前缀
>
> 遵循包内修改原则，一般情况下不导出全局变量

## init函数 - 初始化包

包内任意 .go 文件内都可定义一到多个 init 初始化函数。 初始化函数由编译器生成代码自动执行（仅执行一次），不能被其他代码调用。

```go
var x = 100

func init() {
	println("a", x) // a 100
}

func init() {
	println("b", x) // a 100
}
```

可以使用GODEBUG查看初始化执行情况

```bash
GODEBUG=inittrace=1 ./test
init	internal/bytealg 	@0.008 ms, 	0.008 ms clock, 0 bytes, 0 allocs
init	runtime 			@0.040 ms, 	0.048 ms clock, 0 bytes, 0 allocs
init	main 				@0.31 ms,	0.014 ms clock, 0 bytes, 0 allocs
# 		包名 				   启动时刻     执行耗时          堆内存分配数量和次数。
```

使用init函数应注意：

- 避免在init函数中进行资源申请、打开文件、连接数据库等可能失败的操作
- 一个包（或一个文件）只能有init函数。如果存在多个，init函数不能相互有依赖。

## 内部包

内部包的规范约定：导出路径包含`internal`关键字的包，只允许`internal`的父级目录及父级目录的子包导入，其它包无法导入。

```bash
.
|-- checker
|   |-- internal
|   |   |-- cpu
|   |   |   `-- cpu.go
|   |   `-- ram
|   |       `-- ram.go
|   `-- server.go
|-- go.mod
|-- go.sum
`-- main.go
```

如上包结构的程序，`checker/internal/cpu`和`checker/internal/ram`只能被`checker`包及其子包中的代码导入，不能被`main.go`导入。

内部包（internal package）机制相当于增加了新访问权限控制： 

- 内部包（含自身）只能被其父目录（含所有层次子目录）访问。
- 内部包私有成员，依然只能在自己包内访问。

## 导入包

每个包是由一个全局唯一的字符串所标识的导入路径定位。使用包前，必须使用import语句导入路径。

```go
import (
    "fmt"
    "math/rand"
    "encoding/json"

    "golang.org/x/net/html"

    "github.com/go-sql-driver/mysql"
)
```

使用别名解决包冲突

```go
import (
	md "math/rand"
    m2d "math2/rand"
)
```

简便方式

```go
import (
	. "math/rand" // 使用简便方式。可以直接使用包内的导出符，工作中禁止使用
)
```

只初始化包，只执行init函数

```go
import (
	_ "math/rand" // 只执行init函数
)
```

导入包说明：

- 不能直接或间接导入自己，不支持任何形式的循环导入。不能形成导入换（A->B，B->C，C->A）
- 未使用的导入（不包括初始化方式）被编译器视为错误。
- 禁止使用 `.` 来简化导入包

## module - 模块

模块（module）是包（packages）和其依赖项（dependency）的集合。 直接体现是 go.mod 文件，存储了模块路径、编译器版本，以及依赖项列表。

初始化模块

```bash
go mod init github.com/stonebirdjx/test
```

go.mod内容

```go
module github.com/stonebirdjx/test // 模块路径

go 1.18 // 编译器最低版本号

require (
	github.com/gin-contrib/pprof v1.3.0
	github.com/gin-gonic/gin v1.8.1
	github.com/go-sql-driver/mysql v1.6.0
	github.com/google/uuid v1.3.0
)
```

go get添加、下载（更新）依赖项，可以向 go.mod 、 go.sum 添加依赖项和验证信息。

```bash
go get . # 分析源码，添加所有依赖。
go get -u example.com/test # 指定模块，下载最新版本。
go get -u=patch example.com/test@v1.3.4 # 指定版本号。
go get example.com/test@lastet # 最新版本。
go get example.com/test@4cf76c2 # 伪版本号（commit hash）
go get example.com/test@bufix # 分支（branch）
# 可使用 > 、 >= 、 < 、 <= 操作符指定版本查询范围（module query）。
go get example.com/test@>v1.3.4
```

go mod 管理模块。

- go mod init : 初始化模块，创建 go.mod 文件。 
- go mod tidy : 添加遗漏，移除不需要的依赖项。 
- go mod edit : 命令行方式编辑模块设置。
  - -require , -droprequire ：添加直接或间接依赖项。 
  - -replace , -dropreplace ：替换模块路径或版本。 

```go
go mod edit -require example.com/test@v1.3.4
```

go mod 版本标识，以 v 开头，然后是语义版本（semantic version）。

正式发布前的预发行版本，添加 -pre 后缀。 

使用 git tag 之类的功能标记语义化版本号。 

如没有标记，则使用提交（commit）信息（time, ident）构成伪版本号。

```bash
v(major).(minor).(patch)-(pre|beta)
 主要 次要 补丁 预发行或测试版
github.com/mattn/go-isatty v0.0.13  // indirect
github.com/mattn/go-isatty v0.0.13-pre // indirect
github.com/mattn/go-isatty v0.0.13-beta // indirect
github.com/modern-go/concurrent v0.0.0-20180228061459-e0a39a4cb421 // indirect
```

查看所有版本

```go
go list -m -versions github.com/shirou/gopsutil
go list -m -versions all
```

module说明：

- 如果子目录有go.mod文件，那么子目录属于独立模块，不属于当前模块
- 使用命令 go get 添加、下载（更新）依赖项，其他操作可用 go mod 完成。
- 使用 go clean -modcache 清除缓存。（自动重新下载）
- 更改go.mod后，使用go mod tidy更新依赖

## go work - 工作空间

go work 主要解决多模块开发遇到的问题: 比如编译时找不到未发布的模块

go work初始化 go.work 文件,支持go work use, go work replace 指令

```bash
go work init

go work use ./module1 ./module2
```

go.work文件内容

```bash
# cat go.work

go 1.18 

use (
	./module1
	./module2
)

# ./module1/go.mod
# ./module2/go.mod
```

编译时可以关闭 GOPROXY ，避免在线获取模块信息拖慢速度。

```go
GOPROXY=off go build 
```

## go vendor - 打包依赖源码

将依赖包复制到 ./vendor目录

```go
go mod vendor
```

禁用工作空间，以 ./vendor的方式编译

```go
go build -mod vendor
```

## 包相关的环境变量

模块感知（module-aware）模式，相关环境变量说明。 

- GO111MODULE ：模块感知模式开关。（默认：on） 
- GOMODCACHE ：模块缓存路径。 
- GOPROXY ：模块代理服务列表。 
- GOSUMDB ：模块数据安全校验数据库。 
- GOPRIVATE ：私有模块路径，绕过 GOPROXY 直接获取。 

> GOPRIVATE 是 GONOPROXY 、 GONOSUMDB 的默认值。

## :point_right:包总结

- 包名通常与目录名一致，注意命名格式和规范。
- 特殊含义的包名：`main` 用户可执行文件入口包 ； `all` 所有包，包括标准库和依赖项。`std` 标准库。 `cmd` 工具链。可使用`go list`查看所有的包，
- 一般情况下禁止导出全局变量。如需使用包的全局变量 **非导出符号+导出函数的形式。**
- 包内成员命名应避免使用包名作为前缀。如`stream.StreamBuffer ` 会显得很累赘。
- 避免使用init函数。
- 注意包依赖关系，禁止使用 `.` 来简化导入包。
- 使用`go list  -m -versions` 来查看所有包的版本
- 关闭GOPROXY，可以避免在线获取模块
- 源文件必须是 `utf-8` 的格式。

# 变量

## 变量声明

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

## :point_right:变量总结

- 使用`:=` 语法糖精简赋值，只能在函数内部使用
- 特殊只写变量 `_` ,用于忽略值，起占位作用
- 如果局部变量未使用编译期会报错。全局变量未使用不会报错
- 全局变量禁止其他包修改，只能小写



# 程序结构

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
	ls = unsafe.Sizeof(l)
)
```

在常量组中，如不提供类型和初始化值，那么视作与上一常量相同。

```go
const (
	s = "abc"
	x        // x = "abc"
)

// 数组通过索引定义的时候也是这个道理
```

关键字 iota 定义常量组中从 0 开始按行计数的自增枚举值。

简单的说iota是const常量组的索引，在n行，值为n-1

```go
const (
	Sunday    = iota // 0
	Monday           // 1，通常省略后续⾏表达式。
	Tuesday          // 2
	Wednesday        // 3
	Thursday         // 4
	Friday           // 5
	Saturday         // 6
)

const (
	_        = iota             // iota = 0
	KB int64 = 1 << (10 * iota) // iota = 1
	MB                          // 与 KB 表达式相同，iota = 2
	GB
	TB
)

// 可以提供多个 iota，它们各自增⻓
const (
	A, B = iota, iota << 10 // 0, 0 << 10
	C, D                    // 1, 1 << 10
)
```

如果iota自增被打断，则必须显式恢复

```go
const (
	A = iota // 0
	B        // 1
	C = "c"  // c
	D        // c，与上⼀⾏相同。
	E = iota // 4，显式恢复。注意计数包含了 C、D 两⾏。
	F        // 5
)
```

**常量说明**

- 常量是编译期可确定的值，所以最好不用函数，除非函数对常量计算，能返回可确定结果。
- iota在常量组中等同于数组的索引
- 开发过程中尽量不要使用iota

## :point_right:作用域

作用域指的是 定义的内容（变量、结构体...）可以有效使用范围。语法块（block）句法块是由 `{}`所包含的一系列语句，父级 `{}` 中定义的内容可以在子级 `{}` 中被访问，反之则不能。

```go
var a = "stone"

func block() {
	fmt.Println(a) // stone
	b := "bird"
	{
		fmt.Println(b) // bird
		c := "block content"
	}
	fmt.Println(c) // c不能被外层取到。Unused variable 'c'
}
```

**作用域说明**

- 使用时才申请，保证作用域尽量的小，作用域越小越安全。
- 有goto的场景要保证跳转后值有效性

# 类型

golang类型主要分为值类型和引用类型。值类型是指变量直接存储值。引用类型是指变量存储的是一个地址，有地址去访问真正存储的值。

值类型： int 、float 、bool、string，数组，结构体。

引用类型：map，slice，channel，func，interface、指针。

## 基本数据类型

### :point_right:整型 

有符号整型：int8、int16、int32和int64，分别对应8、16、32、64bit大小的有符号整数。

> int 在32位设备上对应int32，在64位身上对应int64

无符号整型：uint8、uint16、uint32和uint64

>  uint在32位设备上对应uint32，在64位身上对应uint64

运算符，照优先级递减的顺序排列

```go
*      /      %      <<       >>     &       &^
+      -      |      ^
==     !=     <      <=       >      >=
&&
||

// 位运算说明
&      位运算 AND
|      位运算 OR
^      位运算 XOR
&^     位清空（AND NOT）
<<     左移
>>     右移
```

**运算符说明**

- 没有 `~` 取反，取反也用 `^`
- 只有 `i++` 或者 `i--`， 没有`++i`或者`--i`
- 运算过程中保证除数不为0

> 位操作运算符`^`作为二元运算符时是按位异或（XOR），当用作一元运算符时表示按位取反

### :point_right:浮点型

提供了两种精度的浮点数，float32和float64。

```go
var f float32 = 16777216 // 1 << 24
fmt.Println(f == f+1)    // "true"!
```

浮点数的字面值可以直接写小数部分

```go
const e = 2.71828 // (approximately)
```

很小或很大的数最好用科学计数法书写，通过e或E来指定指数部分：

```go
const Avogadro = 6.02214129e23  // 阿伏伽德罗常数
const Planck   = 6.62606957e-34 // 普朗克常数
```

正无穷大和负无穷大，分别用于表示太大溢出的数字和除零的结果，还有NaN非数，一般用于表示无效的除法操作结果0/0或Sqrt(-1).

```go
var z float64
fmt.Println(z, -z, 1/z, -1/z, z/z) // "0 -0 +Inf -Inf NaN"
```

不能比较非数nan

```go
nan := math.NaN()
fmt.Println(nan == nan, nan < nan, nan > nan) // "false false false"
```

**浮点数说明**

- 浮点数和整型拥有一样的运算符和能力
- 不能比较非数nan
- 禁止使用浮点数作为循环的控制变量
- 使用%e（带指数）或%f的形式打印浮点数
- 浮点数的相等比较是危险的

### 复数

Go语言提供了两种精度的复数类型：complex64和complex128，分别对应float32和float64两种浮点数精度。内置的complex函数用于构建复数，内建的real和imag函数分别返回复数的实部和虚部：

复数运算 `i*i = -1`

加法： (a+bi)+(c+di)=(a+c)+(b+d)i

减法： (a+bi)-(c+di)=(a-c)+(b-d)i

乘法:	(a+bi)(c+di)=(ac-bd)+(bc+ad)i

除法：（7+i)/(3+4i)=((7+i)(3-4i))/((3+4i)(3-4i))

```go
var x complex128 = complex(1, 2) // 1+2i
var y complex128 = complex(3, 4) // 3+4i
fmt.Println(x*y)                 // "(-5+10i)"
fmt.Println(real(x*y))           // "-5"
fmt.Println(imag(x*y))           // "10"
```

以 `整数i` 的方式定义复数

```go
x := 1 + 2i
y := 3 + 4i
z := 1i * 1i
```

复数也可以用==和!=进行相等比较。只有两个复数的实部和虚部都相等的时候它们才是相等的

```go
fmt.Println(1 + 2i == 1 + 2i) 
```

**复数说明**

- 和浮点数一样，比较相等时精度问题很危险

### :point_right:布尔型

布尔类型的值只有两种：true和false。一元操作符`!`对应逻辑非操作，因此`!true`的值为`false`

```go
!false // true
!!false // fasle
```

尔值可以和&&（AND）和||（OR）操作符结合

```go
if 'a' <= c && c <= 'z' ||
    'A' <= c && c <= 'Z' ||
    '0' <= c && c <= '9' {
    // ...ASCII letter or digit...
}
```

## :point_right:字符串

字符串是 不可变 字节序列，其本身是一个复合结构。字符串可以包含任意的数据

```go
type stringStruct struct {
	str unsafe.Pointer
	len int
}
```

索引访问字节数组（非字符），不能获取元素地址

```go
s := "hello, world"
fmt.Println(len(s))     // "12"
fmt.Println(s[0], s[7]) // "104 119" ('h' and 'w')
fmt.Println(&s[3]) // 非法，字符串不能获取元素地址
```

原始字符串（raw string）`` 定义的字符串不做转义处理

```go
s := `stone
bird\n\t
`
fmt.Println(s)
// stone
// bird\n\t
```

子串内部指针依旧指向原字节数组

```go
func main() {
	s := "hello, world!"
	s2 := s[:4]
	p1 := (*reflect.StringHeader)(unsafe.Pointer(&s))
	p2 := (*reflect.StringHeader)(unsafe.Pointer(&s2))
	fmt.Printf("%#v, %#v\n", p1, p2)
}

// &StringHeader{ Data:0x497208, Len:13 }
// &StringHeader{ Data:0x497208, Len:4 }
```

要修改字符串，可先将其转换成[]rune 或 []byte，完成后再转换为 string。无论哪种转换，都会重新分配内存，并复制字节数组。

使用for循环遍历字符串时，也有 byte 和 rune 两种方式。

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

可以作为append，和copy参数

```go
func main() {
	s := "de"
	bs := make([]byte, 0)
	bs = append(bs, "abc"...)
	bs = append(bs, s...)
	buf := make([]byte, 5)
	copy(buf, "abc")
	copy(buf[3:], s)
	fmt.Printf("%s\n", bs) // abcde
	fmt.Printf("%s\n", buf)// abcde
}
```

字符串转义

```
\a      响铃
\b      退格
\f      换页
\n      换行
\r      回车
\t      制表符
\v      垂直制表符
\'      单引号（只用在 '\'' 形式的rune符号面值中）
\"      双引号（只用在 "..." 形式的字符串面值中）
\\      反斜杠
```

**字符串说明**

- 索引访问字节数组（非字符），不能用索引获取字节元素指针，&s[i] 非法。 
- 切片返回子串，依旧指向原数组。
- 内置函数 len 返回字节数组长度。
- 支持 != 、 == 、 < 、 <= 、 >= 、 > 、 + 、 += 
- 字符串不可被修改，换成[]rune 或 []byte修改后换回字符串，地址会变。

> 字符串的存储在只读段上，并不是堆上或者栈上

## :point_right:数组

数组是值类型，数组的长度是固定的, 赋值和传参会复制整个数组，而不是指针。

```go
var a [4]int // 元素自动初始化为零。

b := [4]int{2, 5} // 未提供初始值的元素自动初始化为 0。
c := [4]int{5, 3: 10} // 可指定索引位置初始化。
d := [...]int{1, 2, 3} // 编译器按初始化值数量确定数组长度。
/*
	a: [0 0 0 0]
	b: [2 5 0 0]
	c: [5 0 0 10]
	d: [1 2 3]
	
*/
a := [2][3]int{{1, 2, 3}, {4, 5, 6}}
```

支持索引赋值初始化，没有包含的索引为默认值

```go
e := [...]int{10, 3: 100} // 支持索引初始化，数组长度与此有关。

// e: [10 0 0 100]
```

多维数组不能使用语法糖 `...` 定义, 语法糖只能用于外层数组

```go
b := [...][2]int{{1, 1}, {2, 2}, {3, 3}} // 第 2 纬度不能⽤ "..."。第二维（多维）不能用语法糖
```

多维数组依旧是连续内存存储

```go
func main() {
	x := [...][2]int{
		{1, 2},
		{3, 4},
	}

	y := [...][3][2]int{
		{
			{1, 2},
			{3, 4},
			{5, 6},
		},
		{
			{10, 11},
			{12, 13},
			{14, 15},
		},
	}

	fmt.Println(x, len(x), cap(x)) // 2, 2
	fmt.Println(y, len(y), cap(y)) // 2, 2
}

/*
(gdb) x/4xg &x
0xc000066df0: 0x0000000000000001 0x0000000000000002
0xc000066e00: 0x0000000000000003 0x0000000000000004
(gdb) x/12xg &y
0xc000066e30: 0x0000000000000001 0x0000000000000002
0xc000066e40: 0x0000000000000003 0x0000000000000004
0xc000066e50: 0x0000000000000005 0x0000000000000006
0xc000066e60: 0x000000000000000a 0x000000000000000b
0xc000066e70: 0x000000000000000c 0x000000000000000d
0xc000066e80: 0x000000000000000e 0x000000000000000f
*/
```

取值

```go
var a [10]int 
a[0:10]、a[:10]、a[0:]、a[:] // 这些情况等价
```

数组比较，长度和类型相同且元素可比较的数组可以比较，[2]int 和 [3]int 是不同类型。

```go
a := [2]int{1, 2}
b := [...]int{1, 2}
c := [2]int{1, 3}
fmt.Println(a == b, a == c, b == c) // "true false false"
d := [3]int{1, 2}
fmt.Println(a == d) // compile error: cannot compare [2]int == [3]int
```

**组数说明**

- 数组是单一内存块,并无其他附加结构
- 支持索引赋值初始化，无索引的部分为默认值
- 支持获取元素指针，&s[i]
- 内置函数 len 和 cap 都返回一维长度
- 元素类型支持 == 、 != ，那么数组也支持
- 指针数组 [n]*T，数组指针 *[n]T。
- 数组通过下标取值前，为安全需判断数组的长度。

## :point_right:结构体

结构体是值类型，是一种聚合的数据类型，是由零个或多个任意类型的值聚合成的实体。每个值称为结构体的成员。

结构体赋值和传参会复制全部内容。可用"_" 定义补位字段，支持指向自身类型的指针成员

```go
type node struct {
	id    int
	value string
	next  *node // 自身指针类型。
}
```

匿名结构体

```go
// 匿名结构
user := struct {
		id   int
		name string
	}{
		id:   1,
		name: "user1",
	}
```

所有成员类型相同，顺序相同，可比较的前提下结构体才可以比较`==` 或 `!=`

```go
func main() {
	// 字段名、字段类型、标签和排列顺序都相同。
	d1 := struct {
		x int
		s string
	}{100, "abc"}
	var d2 struct {
		x int
		s string
	}
	d2.x = 100
	d2.s = "abc"
	println(d1 == d2) // true
}
```

**空结构体**

空结构（ struct{} ）是指没有字段的结构类型，常用于值可被忽略的场合，比如set集合，channel消息通知。 

无论是其自身，还是作为元素类型，其长度都为零，但这不影响作为实体存在。

```go
func main() {
	var a struct{}
	var b [1000]struct{}
	s := b[:]
	println(unsafe.Sizeof(a), unsafe.Sizeof(b)) // 0, 0
	println(len(s), cap(s))                     // 1000, 1000
}
```

**Tag**

标签（tag）不是注释，而是对字段进行描述的元数据。 不是数据成员，却是类型的组成部分。 内存布局相同，允许显式类型转换。

运行期，可用反射（reflect）获取标签信息。常被用作格式校验，数据库关系映射等。json、bson、orm，http参数等

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	type User struct {
		id   int    `field:"uid" type:"integer"`
		name string `field:"name" type:"text"`
	}
	t := reflect.TypeOf(User{})
	for i := 0; i < t.NumField(); i++ {
		f := t.Field(i)
		fmt.Println(f.Name, f.Tag.Get("field"), f.Tag.Get("type"))
	}
}

// id: uid integer
// name: name text
```

**匿名字段**

所谓匿名字段（anonymous field），是指没有名字，仅有类型的字段。也被称作嵌入字段、嵌入类型。 

隐式以类型名为字段名。 嵌入类型与其指针类型隐式字段名相同。（ * 不能用于名称） 可像直属字段那样直接访问。

```go
type Attr struct {
	perm int
}
type File struct {
	Attr // 隐式字段名 Attr，语法糖!
	name string
}
func main() {
	f := File {
		name: "test.dat",
		// 必须显式名称初始化。
		// perm: 0644,
		// ~~~~ unknown field 'perm' in struct literal of type File
		Attr: Attr{
			perm: 0644,
		},
	}
	// 像直属字段访问。
	fmt.Printf("%s, %o\n", f.name, f.perm)
}
```

直属字段与嵌入字段成员存在重名问题。

编译器优先选择直属命名字段，其次按嵌入层次逐级查找匿名字段成员。 如匿名字段成员被外层同名遮蔽，那么必须用显式字段名。

```go
type File struct {
	name []byte
}

type Data struct {
	File
	name string // 和 File.name 重名。
}

func main() {
	d := Data{
		name: "data",
		File: File{ []byte("file") },
	}

	d.name = "data2" // 优先选择直属命名字段。
	d.File.name = []byte("file2")

	fmt.Println(d.name, d.File.name)
}
```

匿名嵌入是 组合（composition），而非 继承（inheritance）。 结构虽然承载了 class 功能，但无法 多态（polymorphisn），只能以方法集配合接口实现。

**内存对齐**

不管结构包含多少字段，其内存总是一次性分配，各字段（含匿名字段成员）在相邻地址空间按定义顺序（含对 齐）排列。当然，对于引用类型、字符串和指针，结构内存中只包含其基本（头部）数据。 借助 unsafe 相关函数，输出所有字段的偏移量和长度。

对齐以所有字段中最长的基础类型宽度为准。编译器这么做的目的，即为了最大限度减少读写所需指令，也因 为某些架构平台自身的要求。

```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	v1 := struct {
		a byte
		b byte
		c int32 // <-- int32
	}{}

	v2 := struct {
		a byte
		b byte // <-- byte
	}{}

	v3 := struct {
		a byte
		b []int // <-- int
		c byte
	}{}

	fmt.Printf("v1: %d, %d\n", unsafe.Alignof(v1), unsafe.Sizeof(v1))
	fmt.Printf("v2: %d, %d\n", unsafe.Alignof(v2), unsafe.Sizeof(v2))
	fmt.Printf("v3: %d, %d\n", unsafe.Alignof(v3), unsafe.Sizeof(v3))
}

/*
v1: 4, 8
v2: 1, 2
v3: 8, 40
(gdb) x/2xw &v1
0xc000088e88: 0x00000201 0x00000003
+---+---+---+---+---+---+---+---+
| a | b | ? | ? | c |
+---+---+---+---+---+---+---+---+
|<----- 4 ----->|<----- 4 ----->|
(gdb) x/2xb &v2
0xc000088e86: 0x01 0x02
+---+---+
| a | b |
+---+---+
0 1 2
(gdb) x/5xg &v3
0xc000088f48: 0x0000000000000001 0x000000c000088e90
0xc000088f58: 0x0000000000000003 0x0000000000000003
0xc000088f68: 0x0000000000000002
+---+-------+-----------+-----------+-----------+---+-------+
| a | ... | b.ptr | b.len | b.cap | c | ... |
+---+-------+-----------+-----------+-----------+---+-------+
|<--- 8 --->|<--- 8 --->|<--- 8 --->|<--- 8 --->|<--- 8 --->|
*/

```

如果空结构是最后一个字段，那么将其当做长度 1 的类型，避免越界。







**结构体说明**

直属字段名必须唯一。

支持自身指针类型成员。 

可用 _ 忽略字段名。 

支持匿名结构和空结构。 

支持匿名嵌入其他类型。 

支持为字段添加标签。 

仅所有字段全部支持，才可做相等操作。 

可用指针选择字段，但不支持多级指针。 

字段名、类型、标签及排列顺序属类型组成部分。 

除对齐外，编译器不会优化或调整布局





### :point_right:引用类型

引用类型包括 slice、map 和 channel。它们有复杂的内部结构，除了申请内存外，还需要初始化相关属性，诸如指针、长度，甚至哈希分布、数据 队列等。

内置函数 new 计算类型大小，为其分配零值内存，返回指针。而make 会被编译器翻译成具体的创建函数，由其分配内存和初始化成员结构，以slice为例。

```
 header 		       array
 +-----------+ 		   +-----//-----+
 |   array   |	-----> | ...    ... |
 +-----------+         +-----//-----+
 |   len     |
 +-----------+
 |   cap     |
 +-----------+

 |<-- new -->|
 |<--------------  make  ----------->|
```

如上：使用 `new` 只分配了头部内存。使用 `make` 分别全部内存、并初始化。

```go
func slice() {
	var s1 []int
	var s2 = *new([]int)
	var s3 = make([]int, 10, 100)
	fmt.Println(unsafe.Sizeof(s1)) // 24
	fmt.Println(unsafe.Sizeof(s2)) // 24
	fmt.Println(unsafe.Sizeof(s3)) // 24
}
```

使用gdb调试可以发现未初始化的地址为0x0000000000000000

```
$ go build -gcflags "-N"
$ gdb ./test
(gdb) x/3xg &s1
0xc000032740: 0x0000000000000000 0x0000000000000000
0xc000032750: 0x0000000000000000
(gdb) x/3xg &s2
0xc000032728: 0x0000000000000000 0x0000000000000000
0xc000032738: 0x0000000000000000
(gdb) x/3xg &s3
0xc000032758: 0x000000c000032438 0x000000000000000a
0xc000032768: 0x0000000000000064
```

**引用类型说明**

- 禁止使用new初始化引用类型
- new 适用值类型如： int、float，string、数组、结构体等
- make适用于引用类型slice、map、channel

### :point_right:类型转换

除常量、别名以及未命名类型外，强制要求显式类型转换 `T(v)`。

```go
a := 100
b := byte(a)    // 显式转换
c := a + int(b) // 确保类型一致

// 不支持隐式类型转换 
var n int = b  // Error: cannot use b (type byte) as type int in assignment
if a { // The non-bool value 'a' (type int) used as a condition
	...
}
```

使用括号避免优先级错误

```go
(*int)(p)       // 如果没有括号 *(int(p))
(<-chan int)(c) // 如果没有括号 <-(chan int(c))
(func())(x)     // 如果没有括号 func() x
(func() int)(x) // --> 有返回值的函数类型可省略括号，但依然建议使用。
(func() int)(x) // 使用括号后，更易阅读。
```

转换时确保整数转换不会造成数据截断或者符号错误

```go
var a = math.MaxInt32
b := int16(a) // 不符合，不同类型强制转化数据会发生数据截断

var a = math.MinInt32
b := uint32(a) // 不符合，产生符号丢失
```

**类型转换说明**

- 只能强制显示转换类型
- 避免优先级错误
- 确保转换后数据安全，不会造成数据截断或者符号错误。

### :point_right:自定义类型和别名

使用关键字 `type` 定义用户自定义类型

```go
type A int   // 定义新类型
type B = int // 别名

var a A = 10
var b B = 10
var c int = 10

fmt.Println(a + c) // 错误：Invalid operation: a + c (mismatched types A and int)
fmt.Println(b + c)
fmt.Println(a > 8) // go编译器会强制转换A(8)
```

**自定义类型和别名说明**

- 即使底层类型相同，也非同一类型。（区别于别名） 
- 除运算符外，不继承任何信息。 
- 不能隐式转换，不能和底层类型直接比较、运算。
- 别名可以和底层类型进行运算和笔记

### :point_right:类型比较

**可以比较大小**

- 相同的基础数据类型

**可以比较相等 `==`** 

- 相同的基础数据类型
- 相同长度和类型的数组

- 相同成员和类型，并且成员可以比较的结构体
- 相同类型的channel
- 相同类型的别名

**不可以直接比较**

- 自定义类型和底层类型不可比较，是两种不同的类型
- slice、map、func同类型间不能直接比较，只能和 `nil` 比较

> 但可以使用reflect.DeepEqual或者cmp包来比较







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

[雨痕大佬的go学习笔记](https://github.com/qyuhen/book)

[饶全成](https://github.com/qcrao)

强者总是比较孤独,请坚持学习！送上一句黑鸡汤：

> "The last thing you want is to look back on your life and wonder... if only." --by caicloud
