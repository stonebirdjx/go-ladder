<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [前言](#%E5%89%8D%E8%A8%80)
- [HelloWorld](#helloworld)
- [小刀弑牛](#%E5%B0%8F%E5%88%80%E5%BC%91%E7%89%9B)
- [package - 包](#package---%E5%8C%85)
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
- [泛型](#%E6%B3%9B%E5%9E%8B)
  - [类型约束](#%E7%B1%BB%E5%9E%8B%E7%BA%A6%E6%9D%9F)
  - [泛型函数](#%E6%B3%9B%E5%9E%8B%E5%87%BD%E6%95%B0)
  - [泛型类型](#%E6%B3%9B%E5%9E%8B%E7%B1%BB%E5%9E%8B)
  - [声明泛型类型限制 (type constraint)](#%E5%A3%B0%E6%98%8E%E6%B3%9B%E5%9E%8B%E7%B1%BB%E5%9E%8B%E9%99%90%E5%88%B6-type-constraint)
  - [类型转换](#%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
  - [:point_right:暂不支持泛型方法](#point_right%E6%9A%82%E4%B8%8D%E6%94%AF%E6%8C%81%E6%B3%9B%E5%9E%8B%E6%96%B9%E6%B3%95)
  - [:point_right:泛型总结](#point_right%E6%B3%9B%E5%9E%8B%E6%80%BB%E7%BB%93)
- [测试（TDD）](#%E6%B5%8B%E8%AF%95tdd)
  - [:point_right:单元测试](#point_right%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95)
    - [:point_right:并行](#point_right%E5%B9%B6%E8%A1%8C)
    - [Helper](#helper)
    - [清理](#%E6%B8%85%E7%90%86)
    - [:point_right:子测试](#point_right%E5%AD%90%E6%B5%8B%E8%AF%95)
    - [:point_right:覆盖率](#point_right%E8%A6%86%E7%9B%96%E7%8E%87)
  - [:point_right:基准（性能）测试](#point_right%E5%9F%BA%E5%87%86%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95)
    - [:point_right:子测试](#point_right%E5%AD%90%E6%B5%8B%E8%AF%95-1)
    - [并行](#%E5%B9%B6%E8%A1%8C)
    - [:point_right:内存](#point_right%E5%86%85%E5%AD%98)
    - [:point_right:计时器](#point_right%E8%AE%A1%E6%97%B6%E5%99%A8)
    - [:point_right:性能剖析](#point_right%E6%80%A7%E8%83%BD%E5%89%96%E6%9E%90)
  - [模糊测试](#%E6%A8%A1%E7%B3%8A%E6%B5%8B%E8%AF%95)
  - [:point_right:文档测试（示例函数）](#point_right%E6%96%87%E6%A1%A3%E6%B5%8B%E8%AF%95%E7%A4%BA%E4%BE%8B%E5%87%BD%E6%95%B0)
  - [main测试](#main%E6%B5%8B%E8%AF%95)
  - [测试总结](#%E6%B5%8B%E8%AF%95%E6%80%BB%E7%BB%93)
- [测试框架](#%E6%B5%8B%E8%AF%95%E6%A1%86%E6%9E%B6)
  - [:point_right:testify](#point_righttestify)
    - [:point_right:assert - 断言](#point_rightassert---%E6%96%AD%E8%A8%80)
    - [mock - 打桩](#mock---%E6%89%93%E6%A1%A9)
    - [suite - 测试套件](#suite---%E6%B5%8B%E8%AF%95%E5%A5%97%E4%BB%B6)
  - [Mock](#mock)
  - [:point_right:goconvey](#point_rightgoconvey)
  - [gostub](#gostub)
    - [为全局变量打桩](#%E4%B8%BA%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F%E6%89%93%E6%A1%A9)
    - [为函数打桩](#%E4%B8%BA%E5%87%BD%E6%95%B0%E6%89%93%E6%A1%A9)
    - [为过程打桩](#%E4%B8%BA%E8%BF%87%E7%A8%8B%E6%89%93%E6%A1%A9)
  - [monkey](#monkey)
    - [为函数打桩](#%E4%B8%BA%E5%87%BD%E6%95%B0%E6%89%93%E6%A1%A9-1)
    - [为方法打桩](#%E4%B8%BA%E6%96%B9%E6%B3%95%E6%89%93%E6%A1%A9)
  - [:point_right:gomonkey](#point_rightgomonkey)
  - [测试框架总结](#%E6%B5%8B%E8%AF%95%E6%A1%86%E6%9E%B6%E6%80%BB%E7%BB%93)
- [建议](#%E5%BB%BA%E8%AE%AE)
- [参考资料](#%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 前言

:speak_no_evil:如果能成为golang的cookbook就好了:see_no_evil:

整理知识，传播智慧，好好学习，天天向上。

基于golang1.8+

对于一些规则或者要求，可以参考[安全编码规范](https://github.com/stonebirdjx/go-ladder/blob/master/golang%E5%AE%89%E5%85%A8%E7%BC%96%E7%A0%81%E8%A7%84%E8%8C%83.md)

> 文档为个人平时生活收集和工作积累，不涉及商用，仅供学习和参考。请尊重信息、尊重开发者、勿在商业模式下传播。

# HelloWorld

[安装golang](https://go.dev/doc/install)

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, world")
	fmt.Println("Hello, 世界")
}
```

完成并运行上面代码，你现在已经是一个golang程序员 ( `gopher` ) 了

- 程序从 `main` 包的 `main` 函数开始运行。
- 函数体外每个语句应该以关键字开始，如：`var`、`func`、`type`、`const`、`import`、`package`
- 关于run背后的流程，学习原理时再掌握

:beers:扬帆起航

# 小刀弑牛

[官方入门地址](https://tour.go-zh.org/welcome/1)

> 入门重要途径，切勿心急，建议手敲一遍
>
> 如果你是没有其他语言基础的初学者，建议学习2-3遍。

# package - 包

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

# 泛型

泛型（generic）是一种编程范式，这种范式与特定的类型无关，泛型允许在函数和类型的实现中使用某个类型集合中的任何一种类型。

Go语言在泛型中增加了三个新的重要内容：

- 函数和类型 新增对类型形参的支持
- 支持推导，可省略类型实参（type argument）。

> 鉴于 interface{} 很常用，专门为其引入别名 any 。

## 类型约束

```go
[T any] // 任意类型。
[T int] // 只能是 int。
[T ~int] // 是 int 或底层类型是 int 的类型。
[T int | string] // 只能是 int 或 string。
[T io.Reader] // 任何实现 io.Reader 接口的类型。
```

- 竖线（ | ）表示类型集，匹配其中任一类型即可。
- 波浪线（ ~ ）表示底层类型（underlying type）是该类型的所有类型。

## 泛型函数

```go
func Add[T string | int | int64 | float64](a, b T) T {
	return a + b
}

// 使用
Add(1, 2)
Add(1.0,2.0)
```

## 泛型类型

```go
type ListType[T int | int32 | int64 | string] []T
type MapType[K int | int32, V int64 | string] map[K]V

func main() {
    var intList ListType[int]
    intList = []int{1, 2, 3}
    fmt.Println(intList)
    strList := ListType[string]{"1", "2", "3"}
    fmt.Println(strList)

    intMap := MapType[int, string]{1: "1", 2: "2"}
    int32Map := MapType[int32, int64]{1: 2, 3: 4}
    fmt.Println(intMap)
    fmt.Println(int32Map)
}
```

## 声明泛型类型限制 (type constraint)

在 Go 的类型限制是通过接口实现

```go
type Ordered interface {
	Integer | Float | ~string
}

type Integer interface {
	Signed | Unsigned
}

type Signed interface {
	~int | ~int8 | ~int16 | ~int32 | ~int64
}

type Unsigned interface {
	~uint | ~uint8 | ~uint16 | ~uint32 | ~uint64
}
```

泛型函数内不能定义新类型。 

如类型约束不是接口，则无法调用其成员。

## 类型转换

不支持直接 switch 类型推断（type assert），可先转换为普通接口 any。

```go
func test[T any](x T) {
	// switch x.(type) {}
	// ~~~~ cannot use type switch on type parameter value x
	// 先转换为接口 ----------

	// var i interface{} = x
	var i any = x
	switch i.(type) {
	case int: println("int", x)
	}
}
```

## :point_right:暂不支持泛型方法

```go
// S 是一个普通的struct,但是包含一个泛型方法Identity.
type S struct{}

// Identity 一个泛型方法，支持任意类型.
func (s S) Identity[T string](v T) T { return v }

var s S
s.Identity("stb") // 会报错
```

可以使用结构体支持泛型来代替

```go
package database

type Client struct{ ... }

type Querier[T any] struct {
	client *Client
}

func NewQuerier[T any](c *Client) *Querier[T] {
	return &Querier[T]{
		client: c,
	}
}

func (q *Querier[T]) All(ctx context.Context) ([]T, error) {
	// implementation
}

func (q *Querier[T]) Filter(ctx context.Context, filter ...Filter) ([]T, error) {
	// implementation
}
```

## :point_right:泛型总结

- 泛型虽然简化了代码，在平时开发过程中尽量少使用泛型。
- 泛型可能引发 动态调用、无法内联、内存逃逸 等性能问题。

- 尽量在数据结构中使用泛型。
- 不要将接口传递给泛型函数。

# 测试（TDD）

golang支持的测试类型：

- 单元测试（unit testing）：对程序模块进行正确性检验。 
- 基准测试（benchmark）：对某项性能指标进行测量和评估。
- 模糊测试（fuzz testing）：输入随机数据，发现潜在错误。
- 文档测试（example）(函数示例)：对函数的进行使用展示，通过go doc可以查看。

要保证测试文件以 `_test.go` 结尾，所有以`_test.go`为后缀名的源文件在执行go build时不会被构建成包的一部分，它们是go test测试的一部分。

> 测试时一般会自定义一些测试数据，为了不影响原包，建议将测试包的包名加上 `_test`,与原包区分

## :point_right:单元测试

单元测试函数的名字必须以Test开头，可选的后缀名必须以大写字母开头，函数参数必须 *testing.T（T类型的指针入参） ：

```bash
func TestSin(t *testing.T) { /* ... */ }
func TestCos(t *testing.T) { /* ... */ }
func TestLog(t *testing.T) { /* ... */ }
```

用`go test`命令即可启动单元测试

```bash
go test -v
go test math  // 测试指定包
go test path/stb/ -v -run "Add" // 相对路径
```

执行参数

- -args : 命令行参数。（包列表必须在此参数前）
-  -c : 仅编译，不执行。（用 -o 修改默认测试可执行文件名）
-  -json : 输出 JSON 格式。 
- :point_right:-count : 执行次数（默认1）,带参数可以导致换成失效。或者使用` go clean -testcache`清除缓存
- -list : 用正则表达式列出测试函数名，不执行。 
- :point_right:-run ：用正则表达式执行要执行的测试函数。 
- -timeout : 超时 panic!（默认 10m） -
- :point_right:-v : 输出详细信息。 
- -x : 输出构建信息。

### :point_right:并行

默认情况下 包内用例串行、（如果）多包并行

参数 -p, -parallel 用于设置 GOMAXPROCS ，这会影响并发执行。

```bash
go test path/stb/ -v -run "Add" -p 2
```

调用 Parallel 后，测试函数暂停（ PAUSE ）。 等所有串行测试结束后，再恢复（ CONT ）并发执行。

t.Parallel() 可以让函数并行，否则是串行。

```go
func TestA(t *testing.T) {
	t.Parallel() // 通常放在第一行。
	time.Sleep(time.Second * 10)
}
```

### Helper 

t.Helper() 是函数标记的测试助手。 

输出测试信息时跳过助手函数，直接显示测试函数文件名、行号。 

直接在测试函数中调用无效。 

测试助手可用作断言。 

```go
func assert(t *testing.T, b bool) {
	t.Helper()
	if !b { t.Fatal("assert fatal") }
}

go test -v -run "A"
=== RUN TestA
main_test.go:13: assert fatal # 不会显示助手函数信息。
```

### 清理

t.Cleanup()为测试函数注册清理函数，在测试结束时执行。

- 如注册多个，则按 FILO 顺序执行。 
- 即便发生 panic ，也能确保清理函数执行。

和 defer 的区别：即便在其他函数内注册，也会等测试结束后再执行

```go
func TestA(t *testing.T) {
    t.Cleanup(func(){})
	if !b { t.Fatal("assert fatal") }
}
```

### :point_right:子测试

使用t.Run("name",tFunc)运行子测试

```go
func (t *T) Run(name string, f func(t *T)) bool
```

子测试优点

- 将测试函数拆分为子测试，更符合套件（suite）模式。
- 便于编写初始化（setup）和清理（teardown）逻辑。 
- 表驱动（table driven）时，拆分成多个并发测试。 
- 便于观察子测试时间，不用考虑外部环境影响。

> 表驱动（table driven）将数据和逻辑分离，便于维护和扩展。

```go
func TestA(t *testing.T) {
	time.Sleep(time.Second)
}

func TestB(t *testing.T) {
	time.Sleep(time.Second)
}

func TestC(t *testing.T) {
	time.Sleep(time.Second)
}

func TestSuite(t *testing.T) {
	t.Log("setup")
	defer t.Log("teardown")
	t.Run("A", TestA)
	t.Run("B", TestB)
	t.Run("C", TestC)
}
```

**注意子测试提前退出**

Parallel 会挂起当前测试，让 Run 提前退出。

```go
func TestSuite(t *testing.T) {
    tests := []int{1, 2}
    defer t.Log("teardown")
	
    for v := range tests {
        x := v
        // 挂起后会直接退出主程序
        t.Run("", func(t *testing.T) {
            t.Parallel()
            time.Sleep(time.Second * 5)
            println(x)
        })
	}
}
```

子测试并发通过把多个子测试放到一个组中并发执行

```go
func TestSuite(t *testing.T) {
    tests := []int{1, 2}
    defer t.Log("teardown")

	// 放到测试组不受影响。
 	t.Run("group", func(t *testing.T) {
 		for v := range tests {
 			x := v

 			// 因 Parallel 提前退出。
			 t.Run("", func(t *testing.T) {
 				t.Parallel()
 				time.Sleep(time.Second * 5)
 				println(x)
 			})
 		}
	})
}
```

### :point_right:覆盖率 

代码覆盖率（code coverage）是度量测试完整和有效性的一种手段。 

- 通过覆盖率值，分析测试代码编写质量。 
- 检测是否提供完备测试条件，是否执行了全部目标代码。 
- 量化测试，让白盒测试真正起到应有的质量保障作用。 

> 并非追求形式上的数字百分比，而是为改进测试提供一个发现缺陷的机会。 

通过`go test -cover` 获取覆盖率

```bash
go test -cover ./stb_test

ok stb_test 0.005s coverage: 66.7% of statements
```

获取更详细信息，可指定 covermode 和 coverprofile 参数。 

set : 检测语句是否执行。（默认）

count : 检测语句执行次数。 

atomic : 同 count ，但支持并发模式。

```bash
go test -cover -covermode count -coverprofile cover.out ./mylib
```

使用 go tool cover 解读 cover.out 文件。

```bash
go tool cover -html cover.out
go tool cover -html cover.out -o cover.html
go tool cover -func cover.out -o cover.txt
```

## :point_right:基准（性能）测试

基准测试是测量一个程序在固定工作负载下的性能。

基准测试函数和普通测试函数写法类似，但是以Benchmark为前缀名，并且带有一个`*testing.B`类型的参数；

`*testing.B`参数除了提供和`*testing.T`类似的方法，还有额外一些和性能测量相关的方法。它还提供了一个整数N，用于指定操作执行的循环次数。

```bash
import "testing"

func BenchmarkIsPalindrome(b *testing.B) {
    for i := 0; i < b.N; i++ {
        IsPalindrome("A man, a plan, a canal: Panama")
    }
}

go test -bench=.
go test -bench=IsPalindrome
```

- 文件名必须以“_test.go”结尾；
- 函数名必须以“BenchmarkXxx”开始；
- 使用命令“go test -bench=.”即可开始性能测试；
- 仅执行性能测试，可用 -run NONE 忽略单元测试。

```bash
go test -v -bench . -run None ./mylib
```

基本参数

- -bench ：指定要运行的测试。（正则表达式， -bench . ）
-  -benchtime ：单次测试运行时间或循环次数。（默认 1s，1m20s，100x） 
- -count ：执行几轮测试。（benchtime * count） 
- -cpu ：测试所用 CPU 核心数。（ -cpu 1,2,4 执行三轮测试） 
- -list : 列出测试函数，不执行。
-  -benchmem ：显示内存分配（堆）信息。

`-benchmem`命令行标志参数将在报告中包含内存的分配数据统计。

```bash
go test -bench=. -benchmem
PASS
BenchmarkIsPalindrome    2000000    807 ns/op    128 B/op  1 allocs/op
```

### :point_right:子测试

操作与 T 基本一致。但没有 Parallel ，而是 RunParallel

```go
func BenchmarkSubs(b *testing.B) {
	b.Log("setup")
	b.Cleanup(func(){ b.Log("cleanup") })
	b.Run("A", BenchmarkA)
    b.Run("B", BenchmarkB)
    b.Run("C", BenchmarkC)
}
```

### 并行

方法 b.RunParallel 创建多个 goroutine 并发测试单个目标。 

- 不能操作计时器。（ StartTimer 、 StopTimer 、 ResetTimer ） 
- 不能执行子测试。（ Run ）

```go
func BenchmarkA(b *testing.B) {
    b.RunParallel(func(pb *testing.PB) {
        for pb.Next() {
        	time.Sleep(time.Microsecond)
        }
    })
}

go test -v -bench . -cpu 1,2,4
```

### :point_right:内存

调用 ReportAllocs ，无论是否有 -benchmem 参数都会输出内存信息

```go
func BenchmarkMem(b *testing.B) {
    b.ReportAllocs()

    for i := 0; i < b.N; i++ {
   		_ = make([]byte, 1<<20)
    }
}

go test -v -run None -bench Mem -benchmem ./mylib
```

### :point_right:计时器

计时器默认自动处理。如测试逻辑中有需要排除的因素，可手工调用。

```go
func BenchmarkTimer(b *testing.B) {
    // setup
    time.Sleep(time.Second)
    
    // teardown
    defer time.Sleep(time.Second)
    
    // 重置计时器，避免 setup 干扰。
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
   		add(1, 2)
    }
    
    // 停止计时器，避免 teardown 干扰。
    b.StopTimer()
}
```

### :point_right:性能剖析

基准测试（Benchmark）对于衡量特定操作的性能是有帮助的，但是当我们试图让程序跑的更快的时候，我们通常并不知道从哪里开始优化。

```bash
go test -cpuprofile=cpu.out
go test -blockprofile=block.out
go test -memprofile=mem.out
```

一旦我们已经收集到了用于分析的采样数据，我们就可以使用pprof来分析这些数据。

```go
go tool pprof -text -nodecount=10 ./http.test cpu.out
```

参数`-text`用于指定输出格式

##  模糊测试

又称随机测试，一种基于随机输入发现代码缺陷的自动化测试技术。

- 对单元测预定数据的补充，查找未预料错误。 
- 并非验证逻辑正确，而是发现输入处理缺陷。

格式

- 保存在 _test.go 内。 
- 函数名以 Fuzz 为前缀。 类型 F 和 T 方法类似，省略。 



- 初始数据，称作 种子语料（seed corpus）。 
- 基于种子构造的随机数据，称作 随机语料（random corpus）。

```go
func FuzzAdd(f *testing.F) {
    // 添加种子。（可选）
    f.Add(1, 1)
    f.Add(1, 2)
    f.Add(1, 3)
    
     // 随机测试。（第一参数为 *T，后续和测试目标函数相同）
    f.Fuzz(func(t *testing.T, x, y int) {
    	add(x, y)
    })
    
    // f.Fuzz(func(t *testing.T, i int, s string) {
        // out, err := Foo(i, s)
        // if err != nil && out != "" {
        //     t.Errorf("%q, %v", out, err)
        // }
	// })
}

 
```

默认被当作普通单元测试运行。测试数据就是种子，不会引入随机语料。

只有添加 -fuzz 参数才会进行随机测试。 

默认无限期执行，直到失败或被用户中断（ CTRL + C ）。 

-fuzz : 测试目标。（regex） 

-fuzztime : 时长或次数。（ 1m20s , 100x ）

```bash
go test -v -fuzz Add -fuzztime 20s ./mylib
```

## :point_right:文档测试（示例函数）

示例函数一般以`example_test.go`为测试文件。一般用于go doc 做文档输出

```go
// 检测单行输出
func ExampleSayHello() {
    gotest.SayHello()
    // OutPut: Hello World
}

// 检测多行输出
func ExampleSayGoodbye() {
    gotest.SayGoodbye()
    // OutPut:
    // Hello,
    // goodbye
}

// 检测乱序输出
func ExamplePrintNames() {
    gotest.PrintNames()
    // Unordered output:
    // Jim
    // Bob
    // Tom
    // Sue
}
```

```bash
go test -v -run "Example"
```

示例行数总结

- 例子测试函数名需要以"Example"开头；
- 检测单行输出格式为“// Output: <期望字符串>”；
- 检测多行输出格式为“// Output: \ <期望字符串> \ <期望字符串>”，每个期望字符串占一行；
- 检测无序输出格式为"// Unordered output: \ <期望字符串> \ <期望字符串>"，每个期望字符串占一行；
- 测试字符串时会自动忽略字符串前后的空白字符；
- 如果测试函数中没有“Output”标识，则该测试函数不会被执行；
- 执行测试可以使用`go test <xxx_test.go>`，此时仅执行特定文件中的测试函数；



## main测试

有时希望在整个测试程序做一些全局的setup和Tear-down，这时就需要Main测试了。

所谓Main测试，即声明一个`func TestMain(m *testing.M)`，它是名字比较特殊的测试，参数类型为`testing.M`指针。如果声明了这样一个函数，当前测试程序将不是直接执行各项测试，而是将测试交给TestMain调度。

```go
package hello

import(
    "fmt"
    "testing"
)

func TestAdd(t *testing.T) {
        r := Add(1, 2)
        if r !=3 {

                t.Errorf("Add(1, 2) failed. Got %d, expected 3.", r)
        }

}


func TestMain(m *testing.M) {
    fmt.Println("begin")
    defer fmt.Println("end")
    m.Run() // 执行测试，包括单元测试、性能测试和示例测试
}
```

如果所有测试均通过测试，m.Run()返回0，否同m.Run()返回1，代表测试失败。

## 测试总结

- 掌握单元测试、子测试、覆盖率。
- 掌握基准测试，查看性能和内存情况。

# 测试框架

## :point_right:testify

`testify`核心有三部分内容：

- `assert`：断言；
- `mock`：测试替身；
- `suite`：测试套件。

github：https://github.com/stretchr/testify

### :point_right:assert - 断言

官方例子

```go
package yours

import (
  "testing"
  "github.com/stretchr/testify/assert"
)

func TestSomething(t *testing.T) {

  // assert equality
  assert.Equal(t, 123, 123, "they should be equal")

  // assert inequality
  assert.NotEqual(t, 123, 456, "they should not be equal")

  // assert for nil (good for errors)
  assert.Nil(t, object)

  // assert for not nil (good when you expect something)
  if assert.NotNil(t, object) {

    // now we know that object isn't nil, we are safe to make
    // further assertions without causing any errors
    assert.Equal(t, "Something", object.Value)
  }
    
}
```

如果多次断言，请使用以下命令：

```go
package yours

import (
  "testing"
  "github.com/stretchr/testify/assert"
)

func TestSomething(t *testing.T) {
  assert := assert.New(t)

  // assert equality
  assert.Equal(123, 123, "they should be equal")

  // assert inequality
  assert.NotEqual(123, 456, "they should not be equal")

  // assert for nil (good for errors)
  assert.Nil(object)

  // assert for not nil (good when you expect something)
  if assert.NotNil(object) {

    // now we know that object isn't nil, we are safe to make
    // further assertions without causing any errors
    assert.Equal("Something", object.Value)
  }
}
```

> `require`提供了和`assert`同样的接口，但是遇到错误时，`require`直接终止测试，而`assert`返回`false`。

### mock - 打桩

mock 包提供了一种机制，可以轻松编写模拟对象，在编写测试代码时可以使用模拟对象代替真实对象。

> 为方法打桩

main.go

```go
package main

import (
    "fmt"
)

type MessageService interface {
    SendChargeNotification(int) error
}

type SMSService struct{}

type MyService struct {
    messageService MessageService
}

func (sms SMSService) SendChargeNotification(value int) error {
    fmt.Println("Sending Production Charge Notification")
    return nil
}

func (a MyService) ChargeCustomer(value int) error {
    a.messageService.SendChargeNotification(value)
    fmt.Printf("Charging Customer For the value of %d\n", value)
    return nil
}

func main() {
    fmt.Println("Hello World")

    smsService := SMSService{}
    myService := MyService{smsService}
    myService.ChargeCustomer(100)
}
```

main_test.go

```go
package main

import (
    "fmt"
    "testing"

    "github.com/stretchr/testify/mock"
)

// smsServiceMock
type smsServiceMock struct {
    mock.Mock
}

// Our mocked smsService method
func (m *smsServiceMock) SendChargeNotification(value int) bool {
    fmt.Println("Mocked charge notification function")
    fmt.Printf("Value passed in: %d\n", value)
 	// this records that the method was called and passes in the value
 	// it was called with
  	args := m.Called(value)
  	// it then returns whatever we tell it to return
  	// in this case true to simulate an SMS Service Notification
  	// sent out
    return args.Bool(0)
}

// we need to satisfy our MessageService interface
// which sadly means we have to stub out every method
// defined in that interface
func (m *smsServiceMock) DummyFunc() {
    fmt.Println("Dummy")
}

// TestChargeCustomer is where the magic happens
// here we create our SMSService mock
func TestChargeCustomer(t *testing.T) {
    smsService := new(smsServiceMock)

   // we then define what should be returned from SendChargeNotification
   // when we pass in the value 100 to it. In this case, we want to return
   // true as it was successful in sending a notification
    smsService.On("SendChargeNotification", 100).Return(true)

   // next we want to define the service we wish to test
   myService := MyService{smsService}
   // and call said method
    myService.ChargeCustomer(100)

   // at the end, we verify that our myService.ChargeCustomer
   // method called our mocked SendChargeNotification method
    smsService.AssertExpectations(t)
}

```

### suite - 测试套件

套件包提供了您可能习惯于从更常见的面向对象语言中使用的功能。 有了它，您可以将测试套件构建为结构，在您的结构上构建设置/拆卸方法和测试方法，并按照正常方式使用“go test”运行它们。

```go
type MySuite struct {
  suite.Suite
  recorder *httptest.ResponseRecorder
  mux      *http.ServeMux
}

func (s *MySuite) SetupSuite() {
  s.recorder = httptest.NewRecorder()
  s.mux = http.NewServeMux()
  s.mux.HandleFunc("/", index)
  s.mux.HandleFunc("/greeting", greeting)
}

func (s *MySuite) TestIndex() {
  request, _ := http.NewRequest("GET", "/", nil)
  s.mux.ServeHTTP(s.recorder, request)

  s.Assert().Equal(s.recorder.Code, 200, "get index error")
  s.Assert().Contains(s.recorder.Body.String(), "Hello World", "body error")
}

func (s *MySuite) TestGreeting() {
  request, _ := http.NewRequest("GET", "/greeting", nil)
  request.URL.RawQuery = "name=dj"

  s.mux.ServeHTTP(s.recorder, request)

  s.Assert().Equal(s.recorder.Code, 200, "greeting error")
  s.Assert().Contains(s.recorder.Body.String(), "welcome, dj", "body error")
}
```

测试函数

```go
func TestHTTP(t *testing.T) {
  suite.Run(t, new(MySuite))
}
```

## Mock 

gomock是官方提供的 mock 框架，同时还提供了 mockgen 工具用来辅助生成测试代码。

github：https://github.com/golang/mock

> 为interface打桩

第一步，先创建user.go

```go
// user.go
package user

// User 表示一个用户
type User struct {
   Name string
}
// UserRepository 用户仓库
type UserRepository interface {
   // 根据用户id查询得到一个用户或是错误信息
   FindOne(id int) (*User,error)
}
```

第二步，通过mockgen在同目录下生成mock文件user_mock.go

```bash
mockgen -source user.go -destination user_mock.go -package user
```

第三步，写user_test.go

```go
// 静态设置返回值
func TestReturn(t *testing.T) {
	ctrl := gomock.NewController(t)
	defer ctrl.Finish()
	
    // 调用mock
	repo := NewMockUserRepository(ctrl)
	// 期望FindOne(1)返回张三用户
	repo.EXPECT().FindOne(1).Return(&User{Name: "张三"}, nil)
	// 期望FindOne(2)返回李四用户
	repo.EXPECT().FindOne(2).Return(&User{Name: "李四"}, nil)
	// 期望给FindOne(3)返回找不到用户的错误
	repo.EXPECT().FindOne(3).Return(nil, errors.New("user not found"))
	// 验证一下结果
	log.Println(repo.FindOne(1)) // 这是张三
	log.Println(repo.FindOne(2)) // 这是李四
	log.Println(repo.FindOne(3)) // user not found
	log.Println(repo.FindOne(4)) //没有设置4的返回值，却执行了调用，测试不通过
}
// 动态设置返回值
func TestReturnDynamic(t *testing.T) {
	ctrl := gomock.NewController(t)
	defer ctrl.Finish()
	repo := NewMockUserRepository(ctrl)
	// 常用方法之一：DoAndReturn()，动态设置返回值 
	repo.EXPECT().FindOne(gomock.Any()).DoAndReturn(func(i int) (*User,error) {
		if i == 0 {
			return nil, errors.New("user not found")
		}
		if i < 100 {
			return &User{
				Name:"小于100",
			}, nil
		} else {
			return &User{
				Name:"大于等于100",
			}, nil
		}
	})
	log.Println(repo.FindOne(120))
	//log.Println(repo.FindOne(66))
	//log.Println(repo.FindOne(0))
}

// 调用次数
func TestTimes(t *testing.T) {
	ctrl := gomock.NewController(t)
	defer ctrl.Finish()

	repo := NewMockUserRepository(ctrl)
	// 默认期望调用一次
	repo.EXPECT().FindOne(1).Return(&User{Name: "张三"}, nil)
	// 期望调用2次
	repo.EXPECT().FindOne(2).Return(&User{Name: "李四"}, nil).Times(2)
	// 调用多少次可以,包括0次
	repo.EXPECT().FindOne(3).Return(nil, errors.New("user not found")).AnyTimes()

	// 验证一下结果
	log.Println(repo.FindOne(1)) // 这是张三
	log.Println(repo.FindOne(2)) // 这是李四
	log.Println(repo.FindOne(2)) // FindOne(2) 需调用两次,注释本行代码将导致测试不通过
	log.Println(repo.FindOne(3)) // user not found, 不限调用次数，注释掉本行也能通过测试
}
```

## :point_right:goconvey

goconvey是一款针对Golang的测试框架，可以管理和运行测试用例，同时提供了丰富的断言函数，并支持很多 Web 界面特性。

github：https://github.com/smartystreets/goconvey

```go
func Division(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("被除数不能为 0")
    }
    return a / b, nil
}

// 测试函数
func TestDivision(t *testing.T) {
    Convey("将两数相除", t, func() {

        Convey("除以非 0 数", func() {
            num, err := Division(10, 2)
            So(err, ShouldBeNil)
            So(num, ShouldEqual, 5)
        })

        Convey("除以 0", func() {
            _, err := Division(10, 0)
            So(err, ShouldNotBeNil)
        })
    })
}
```

**浏览器打开**

```bash
$GOPATH/bin/goconvey

http://localhost:8080
```

## gostub

GoStub是一款轻量级的单元测试框架，接口友好，可以对全局变量、函数或过程(没有返回值的函数)进行打桩。

github：https://github.com/prashantv/gostub

### 为全局变量打桩

```go
package main

import (
   "fmt"
   "github.com/prashantv/gostub"
)

var counter = 100

func stubGlobalVariable() {
   stubs := gostub.Stub(&counter, 200)
   defer stubs.Reset()
   fmt.Println("Counter:", counter)
}

func main() {
   stubGlobalVariable()
}

// output:
// Counter: 200
```

### 为函数打桩

方法一：把函数当做变量

```go
var Exec = func(cmd string, args ...string) (string, error) {
   ...
}
stubs := Stub(&Exec, func(cmd string, args ...string) (string, error) {
   return "test", nil
})
defer stubs.Reset()
```

方法二:StubFunc

```go
package Adapter

import (
   "time"
   "fmt"
   "os"
   "github.com/prashantv/gostub"
)

var timeNow = time.Now
var osHostname = os.Hostname

func getDate() int {
   return timeNow().Day()
}

func getHostName() (string, error) {
   return osHostname()
}

func StubTimeNowFunction() {
   stubs := gostub.Stub(&timeNow, func() time.Time {
      return time.Date(2015, 6, 1, 0, 0, 0, 0, time.UTC)
   })
   defer stubs.Reset()
   fmt.Println(getDate()) 
}

func StubHostNameFunction() {
   stubs := gostub.StubFunc(&osHostname, "LocalHost", nil)
   defer stubs.Reset()
   fmt.Println(getHostName())
}
```

### 为过程打桩

没有返回值的函数称为过程。通常将资源清理类函数定义为过程。

```go
package main

import (
   "fmt"

   "github.com/prashantv/gostub"
)

var CleanUp = cleanUp

func cleanUp(val string) {
   fmt.Println(val)
}

func main() {
    defer stubs.Stub(&CleanUp, func(s string) {
		fmt.Println(fmt.Sprintf("Clean %s old cache", s))
	}).Reset()
    
   CleanUp("Hello go")
}
// Clean Hello go old cache
```







## monkey

monkey是一个Go单元测试中十分常用的打桩工具，它在运行时通过汇编语言重写可执行文件，将目标函数或方法的实现跳转到桩实现，其原理类似于热补丁。

monkey库很强大，但是使用时需注意以下事项：

- monkey不支持内联函数，在测试的时候需要通过命令行参数`-gcflags=-l`关闭Go语言的内联优化。
- monkey不是线程安全的，所以不要把它用到并发的单元测试中。

github:https://github.com/bouk/monkey

> 为函数和方法打桩

### 为函数打桩

```go
// func.go

func MyFunc(uid int64)string{
	u, err := varys.GetInfoByUID(uid)
	if err != nil {
		return "welcome"
	}

	// 这里是一些逻辑代码...

	return fmt.Sprintf("hello %s\n", u.Name)
}
```

test

```go
// func_test.go

func TestMyFunc(t *testing.T) {
	// 对 varys.GetInfoByUID 进行打桩
	// 无论传入的uid是多少，都返回 &varys.UserInfo{Name: "liwenzhou"}, nil
	monkey.Patch(varys.GetInfoByUID, func(int64)(*varys.UserInfo, error) {
		return &varys.UserInfo{Name: "liwenzhou"}, nil
	})

	ret := MyFunc(123)
	if !strings.Contains(ret, "liwenzhou"){
		t.Fatal()
	}
}

// 这里为防止内联优化添加了-gcflags=-l参数。
go test -run=TestMyFunc -v -gcflags=-l
```

### 为方法打桩

```go
// method.go

type User struct {
	Name string
	Birthday string
}

// CalcAge 计算用户年龄
func (u *User) CalcAge() int {
	t, err := time.Parse("2006-01-02", u.Birthday)
	if err != nil {
		return -1
	}
	return int(time.Now().Sub(t).Hours()/24.0)/365
}


// GetInfo 获取用户相关信息
func (u *User) GetInfo()string{
	age := u.CalcAge()
	if age <= 0 {
		return fmt.Sprintf("%s很神秘，我们还不了解ta。", u.Name)
	}
	return fmt.Sprintf("%s今年%d岁了，ta是我们的朋友。", u.Name, age)
}
```

test

```go
// method_test.go

func TestUserGetInfo(t *testing.T) {
	var u = &User{
		Name:     "q1mi",
		Birthday: "1990-12-20",
	}

	// 为对象方法打桩
	monkey.PatchInstanceMethod(reflect.TypeOf(u), "CalcAge", func(*User)int {
		return 18
	})

	ret := u.GetInfo()  // 内部调用u.CalcAge方法时会返回18
	if !strings.Contains(ret, "朋友"){
		t.Fatal()
	}
}
```

## :point_right:gomonkey

基于monkey

github: https://github.com/agiledragon/gomonkey

| 函数说明                                                     |                                            |                                                              |
| ------------------------------------------------------------ | ------------------------------------------ | ------------------------------------------------------------ |
| **ApplyFunc(target, double interface{}**                     | 为**函数**/ **接口**打一个桩               | target表示函数名，第二个参数表示桩函数。                     |
| **ApplyFuncSeq(target interface{}, outputs []OutputCell）**  | 为**函数**/ **接口**打一个特定的**桩序列** | target表示函数名，第二个参数表示桩序列参数（返回值需序列）。 |
| **ApplyMethod(target reflect.Type, methodName string, double interface{}）** | 为**成员方法**打一个桩                     | target表示对象类型，对象的方法名，第三个参数表示桩函数。     |
| **ApplyMethodSeq(target reflect.Type, methodName string, outputs []OutputCell）** | 为**成员方法**打一个特定的**桩序列**       | target表示对象类型，对象的方法名，第三个参数表示桩序列参数（返回值需序列）。 |
| **ApplyFuncVar(target, double interface{}）**                | 为**函数变量**打一个桩                     | target表示函数变量，第二个参数表示桩函数。                   |
| **ApplyFuncVarSeq(target interface{}, outputs []OutputCell）** | 为**函数变量**打一个特定的**桩序列**       | target表示函数变量，第二个参数表示桩序列参数（返回值需序列）。 |
| **ApplyGlobalVar(target, double interface{})**               | 为**全局变量**打一个桩                     | target表示函数变量，第二个参数表示桩函数。                   |
| **Reset()**                                                  | 删除桩                                     |                                                              |

测试例子

https://github.com/agiledragon/gomonkey/tree/master/test

## 测试框架总结

- 熟练掌握基本测试。
- 对于普通功能函数 掌握 testify 
- 对于一些请求、链接需要打桩的函数 掌握goconvey 、gomonkey

# 建议

- 写函数时就把单测写了, 提高代码覆盖率，不要出现有未执行的代码（一些不必要的分支除外）。

强者总是比较孤独,请坚持学习！送上一句黑鸡汤：

> "The last thing you want is to look back on your life and wonder... if only." --by caicloud

# 参考资料

[雨痕大佬的go学习笔记](https://github.com/qyuhen/book)

[饶全成](https://github.com/qcrao)