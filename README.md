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
- [函数](#%E5%87%BD%E6%95%B0)
  - [函数声明](#%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E)
  - [:point_right:值拷贝传递](#point_right%E5%80%BC%E6%8B%B7%E8%B4%9D%E4%BC%A0%E9%80%92)
  - [:point_right:可变参数](#point_right%E5%8F%AF%E5%8F%98%E5%8F%82%E6%95%B0)
  - [参数生命周期](#%E5%8F%82%E6%95%B0%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
  - [多返回值](#%E5%A4%9A%E8%BF%94%E5%9B%9E%E5%80%BC)
  - [具名返回值](#%E5%85%B7%E5%90%8D%E8%BF%94%E5%9B%9E%E5%80%BC)
  - [:point_right:匿名函数](#point_right%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0)
  - [:point_right:闭包](#point_right%E9%97%AD%E5%8C%85)
    - [闭包应用](#%E9%97%AD%E5%8C%85%E5%BA%94%E7%94%A8)
  - [:point_right:defer 延迟调用](#point_rightdefer-%E5%BB%B6%E8%BF%9F%E8%B0%83%E7%94%A8)
    - [:point_right:具名返回下的defer](#point_right%E5%85%B7%E5%90%8D%E8%BF%94%E5%9B%9E%E4%B8%8B%E7%9A%84defer)
  - [错误处理](#%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86)
  - [:point_right:panic/recover](#point_rightpanicrecover)
  - [:point_right:函数总结](#point_right%E5%87%BD%E6%95%B0%E6%80%BB%E7%BB%93)
- [方法](#%E6%96%B9%E6%B3%95)
- [接口](#%E6%8E%A5%E5%8F%A3)
  - [空接口可以被赋值给任何对象](#%E7%A9%BA%E6%8E%A5%E5%8F%A3%E5%8F%AF%E4%BB%A5%E8%A2%AB%E8%B5%8B%E5%80%BC%E7%BB%99%E4%BB%BB%E4%BD%95%E5%AF%B9%E8%B1%A1)
  - [空值判断](#%E7%A9%BA%E5%80%BC%E5%88%A4%E6%96%AD)
  - [接口实现](#%E6%8E%A5%E5%8F%A3%E5%AE%9E%E7%8E%B0)
  - [类型断言](#%E7%B1%BB%E5%9E%8B%E6%96%AD%E8%A8%80)
  - [让编译器检查](#%E8%AE%A9%E7%BC%96%E8%AF%91%E5%99%A8%E6%A3%80%E6%9F%A5)
  - [:point_right:接口总结](#point_right%E6%8E%A5%E5%8F%A3%E6%80%BB%E7%BB%93)
- [Channel - 通道](#channel---%E9%80%9A%E9%81%93)
  - [同步channel](#%E5%90%8C%E6%AD%A5channel)
  - [异步channel](#%E5%BC%82%E6%AD%A5channel)
  - [消息收发](#%E6%B6%88%E6%81%AF%E6%94%B6%E5%8F%91)
  - [单向channel](#%E5%8D%95%E5%90%91channel)
  - [select多路复用](#select%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8)
  - [关闭channel](#%E5%85%B3%E9%97%ADchannel)
  - [:point_right:channel总结](#point_rightchannel%E6%80%BB%E7%BB%93)
- [Goroutine - go程](#goroutine---go%E7%A8%8B)
  - [等待任务结束](#%E7%AD%89%E5%BE%85%E4%BB%BB%E5%8A%A1%E7%BB%93%E6%9D%9F)
  - [终止任务](#%E7%BB%88%E6%AD%A2%E4%BB%BB%E5%8A%A1)
  - [GOMAXPROCS - 进程数（ing）](#gomaxprocs---%E8%BF%9B%E7%A8%8B%E6%95%B0ing)
  - [挂起](#%E6%8C%82%E8%B5%B7)
  - [顺序执行](#%E9%A1%BA%E5%BA%8F%E6%89%A7%E8%A1%8C)
  - [线程管理](#%E7%BA%BF%E7%A8%8B%E7%AE%A1%E7%90%86)
  - [:point_right:goroutine总结](#point_rightgoroutine%E6%80%BB%E7%BB%93)
- [sync-共享变量安全](#sync-%E5%85%B1%E4%BA%AB%E5%8F%98%E9%87%8F%E5%AE%89%E5%85%A8)
  - [竞争检测](#%E7%AB%9E%E4%BA%89%E6%A3%80%E6%B5%8B)
  - [sync.Mutex互斥锁](#syncmutex%E4%BA%92%E6%96%A5%E9%94%81)
  - [sync.RWMutex读写锁](#syncrwmutex%E8%AF%BB%E5%86%99%E9%94%81)
  - [sync.Cond条件变量](#synccond%E6%9D%A1%E4%BB%B6%E5%8F%98%E9%87%8F)
  - [sync.Once单次执行](#synconce%E5%8D%95%E6%AC%A1%E6%89%A7%E8%A1%8C)
  - [sync.Pool复用临时对象](#syncpool%E5%A4%8D%E7%94%A8%E4%B8%B4%E6%97%B6%E5%AF%B9%E8%B1%A1)
  - [sync.Map并发安全的map](#syncmap%E5%B9%B6%E5%8F%91%E5%AE%89%E5%85%A8%E7%9A%84map)
  - [:point_right:锁总结](#point_right%E9%94%81%E6%80%BB%E7%BB%93)
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

# 函数

函数是结构化编程的最小模块单元，函数可以让我们将一个语句序列打包为一个单元，然后可以从程序中其它地方多次调用。

函数将复杂过程分解成若干较小任务，隐藏细节，使得程序结构更加清晰，易于维护。

## 函数声明

使用关键字 func 用于定义函数，声明包括函数名、形式参数列表、返回值列表（可省略）以及函数体。

```go
func name(parameter-list) (result-list) {
    body
}
func f(i, j, k int, s, t string)                 { /* ... */ }
func f(i int, j int, k int,  s string, t string) { /* ... */ }
```

如果实参包括引用类型，如指针，slice(切片)、map、function、channel等类型，实参可能会由于函数的间接引用被修改。

```go
func changeS(s []int) error{
    if len(s) < 1 {
        return errors.New("length err")
    }
    s[0] = 8
    return nil
}
```

不能在函数里面声明函数

```go
func main() {
    // 不支持命名函数，需改用匿名的方式
    // func add() {}  
}
```

## :point_right:值拷贝传递

不管是指针、引用类型，还是其他类型，参数总是 值拷贝传递（pass by value）。区别无非是复制完整目标对 象，还是仅复制头部或指针而已。

```go
//go:noline
func test(x *int, s []int) {
	println(*x, len(s))
}

func main() {
    x := 100
    s := []int{1, 2, 3}
    test(&x, s)
}

/*
$ go build
$ go tool objdump -S -s "main\.main" ./test
func main() {
    x := 100
  	 0x462c94 MOVQ $0x64, 0x20(SP)

    s := []int{1, 2, 3}
     0x462ca9 MOVQ $0x1, 0x28(SP)
     0x462cb2 MOVQ $0x2, 0x30(SP)
     0x462cbb MOVQ $0x3, 0x38(SP)

    test(&x, s)
     0x462cc4 LEAQ 0x20(SP), AX ; &x
     0x462cc9 LEAQ 0x28(SP), BX ; s.ptr
     0x462cce MOVL $0x3, CX ; s.len
     0x462cd3 MOVQ CX, DI
     0x462cd6 CALL main.test(SB)
}
*/
```

## :point_right:可变参数

变参本质上就是切片。只能接收一到多个同类型参数，且必须放在列表尾部

```go
func test(s string, a ...int) {
	fmt.Printf("%T, %v\n", a, a)
}

func main() {
	test("abc", 1, 2, 3, 4)
}
```

当切片为参数时必须展开

```go
func test(a ...int) {
	fmt.Println(a)
}

func main() {
    a := [3]int{10, 20, 30}
    
     // 转换为切片后展开。
    test(a[:]...)
    var s []int
    s = append(s, []int{1,2,3}...) // 追加一个切片, 切片需要解包
}
```

既然变参是切片，那么参数复制的仅是切片自身。正因如此，就有机会修改实参。

```go
//go:noinline
func test(a ...int) {
	for i := range a {
		a[i] += 100
	}
}
func main() {
	a := []int{1, 2, 3}
	test(a...)
	println(a[1])
}

// 102
```

## 参数生命周期

参数和其他局部变量的生命周期，未必能坚持到函数调用结束。 

垃圾回收非常积极，对后续不再使用的对象，可能会提前清理。

```go
//go:noinline
func test(x []byte) {
    println(len(x))
     // 模拟垃圾回收触发。
    runtime.SetFinalizer(&x, func(*[]byte){ println("drop!") })
    runtime.GC()
    println("exit.")

     // 确保目标活着。
    // runtime.KeepAlive(&x)
}

func main() {
    test(make([]byte, 10<<20))
}
// 10485760
// drop!
// exit.
```

## 多返回值

一个函数可以返回多个值。

```go
func getInfo(id string) (string, error) {
	return "", nil
}
```

## 具名返回值

具名返回可以传达函数返回值的含义。尤其在返回值的类型都相同时

```go
func add(x, y int) (sum int) {
    sum = x + y
    return 
    
    // 显示返回
    // c = sum + 1
    // return c
}
```

**可以显示覆盖返回值**

```go
func add(x, y int) (sum int) {
    sum = x + y
   
    // 显示返回
    c := sum + 1
    return c
}
```

要么不具名返回，要么全部具名返回，否则会报错

```go
func test() (int, s string, e error) {
    // return 0, "", nil
    // ~ cannot use 0 as string value in return statement
}
```

## :point_right:匿名函数

匿名函数是指没有名字符号的函数。除此之外，和普通函数类似。 

- 可在函数内定义匿名函数，形成嵌套。 

- 可随定义直接传参调用，并返回结果。 

- 可保存到变量、结构字段，或作为参数、返回值传递。

   

- 匿名函数可引用环境变量，形成闭包。 

- 不曾使用的匿名函数被当作错误。 

- 不能使用编译指令。（ //go:noinline ）

```go
func main() {
	// 变量
	add := func(x, y int) int {
		return x + y
	}
	_ = add(1, 2)
}
```

编译器为匿名函数自动生成名字。

```go
func main() {
    func() {
    	println("abc")
    }()
}
/*
$ go build -gcflags "-l"
$ go tool objdump -S -s "main\.main" ./test
CALL main.main.func1(SB)
*/
```

## :point_right:闭包

闭包（closure）是匿名函数和其引用的外部环境变量组合体。

```go
//go:noinline
func test() func() {
    x := 100
    println(&x, x)
    
    return func() { // 返回闭包，而非函数。
        x++
        println(&x, x) // 引用环境变量 x 。
    }
}
func main() {
	test()()
}
/*
0xc000018060 100
0xc000018060 101
*/
```

闭包会延长环境变量生命周期。 

所谓闭包，实质上是由匿名函数和（一到多个）环境变量指针构成的结构体。

### 闭包应用

绑定局部变量

```go
func test() func() {
    n := 0
    return func() {
        n++
        println("call:", n)
    }
}

func main() {
    f := test()
    f()
    f()
    f()
}
/*
call: 1
call: 2
call: 3
*/
```

绑定状态（模拟结构体方法）

```go
func handle(db Database) func(http.ResponseWriter, *http.Request) {
    return func(w http.ResponseWriter, r *http.Request) {
        url := db.Get()
        io.WriteString(w, url)
    }
}

func main() {
    db := NewDatabase("localhost:5432")

    http.HandleFunc("/url", handle(db))
    http.ListenAndServe(":3000", nil)
}
```

作为装饰器使用，包装函数，改变其签名或增加额外功能。

```go
// 增加功能。
func proxy(f func()) func() {
    return func() {
        n := time.Now()
        defer func() {
        	fmt.Println(time.Now().Sub(n))
        }()

        f()
    }
}
```

## :point_right:defer 延迟调用

语句 defer 注册 稍后执行的函数调用。这些调用被称作 延迟调用，因为它们直到当前函数结束前（ RET ）才被执行。

- 常用于资源释放、锁定解除，以及错误处理等操作。 
- 多个延迟调用按 FILO 次序执行。 
- 运行时确保延延迟调用总被执行。（ os.Exit 除外）

```go
func main() {
    f, err := os.Open("./main.go")
    if err != nil {
    	log.Fatalln(err)
    }

    defer f.Close()
    
    b, err := io.ReadAll(f)
    if err != nil {
    	log.Fatalln(err)
    }
    println(string(b))
}
```

:point_right:正常退出，或以 panic 中断，运行时都会确保延迟函数被执行。

```go
func main() {
    defer println("defer!")
    panic("panic!") // 换成 os.Exit，会立即终止进程，
} 
/*
defer!
panic: panic!
*/
```

注意参数复制行为，必要时以指针或引用类型代替。

```go
func main() {
    x := 100
    defer println("defer:", x)
    x++
    println("main:", x)
}
/*
main: 101
defer: 100
*/
```

### :point_right:具名返回下的defer

注意defer的执行顺序，函数执行 -> defer -> return，具名返回会预先申请返回值的地址

```go
func test() (z int) {
    defer func() {
    	z += 200
    }()
    return 100 // return = (ret_val_z = 100, call defer, ret)
}

func main() {
	println(test()) // 300
}
```

如果不是具名返回

```go
func test() int {
    z := 0
    defer func() {
    	z += 200 // 本地变量，与返回值无关。z 是defer执行func的z，不是外面的z
    }()

    z = 100
    return z // return = (ret_val = 100, call defer, ret)
}

func main() {
	println(test()) // 100
}
```

## 错误处理

必须检查所有返回错误，这也导致代码不太美观。

```go
func main() {
    file, err := os.Open("./main.go")
    if err != nil {
    	log.Fatalln(err)
    }
    defer file.Close()
    
    content, err := io.ReadAll(file)
    if err != nil {
    	log.Fatalln(err)
    }
   
    fmt.Println(string(content))
}
```

error也是值类型，可以使用 == 、!= 进行比较

- err != nil ：是否有错误。
- err == ErrVar ：某个具体错误。 
- case ErrVal ：错误匹配。 
- e, ok := err.(T) ：类型转换。

标准库：

- errors.New : 创建错误对象。 
- fmt.Errorf : 以 %w 包装错误对象，构建错误链。 

包装对象： 

- errors.Unwrap : 返回被包装错误对象。 
- errors.Is : 递归错误链，检查是否有指定错误对象。 
- errors.As : 递归错误链，查找类型相符的对象。

## :point_right:panic/recover

Go的类型系统会在编译时捕获很多错误，但有些错误只能在运行时检查，如数组访问越界、空指针引用等。这些运行时错误会引起panic异常。

与返回 error 相比， panic / recover 的使用方式，很像结构化异常。

```go
func panic(v any)
func recover() any

func Reset(x *Buffer) {
    if x == nil {
        panic("x is nil") // unnecessary!
    }
    x.elements = nil
}
```

在defer函数中调用了内置函数recover，并且定义该defer语句的函数发生了panic异常，recover会使程序从panic中恢复，并返回panic value。导致panic异常的函数不会继续运行，但能正常返回。在未发生panic时调用recover，recover会返回nil。

```go
func Parse(input string) (s *Syntax, err error) {
    defer func() {
        if p := recover(); p != nil {
            err = fmt.Errorf("internal error: %v", p)
        }
    }()
    // ...parser...
}
```

注意捕获顺序

```go
func main() {
    defer func() {
        log.Println(recover()) // 捕获第二次：p2
    }()

    func() {
        defer func(){
            panic("p2") // 第二次！
        }()
        defer func() {
            log.Println(recover()) // 捕获第一次：p1
        }()
        panic("p1") // 第一次！
    }()
    println("exit.")
}
// p1
// p2
```

必须在defer中使用recover

```go
func catch() {
    recover()
}

func main() {
    defer catch() // 有效！在延迟函数内直接调用。
    // defer log.Println(recover()) // 无效！作为参数立即执行。
    // defer recover() // 无效！被直接注册为延迟调用。
    panic("p!")
}
```

## :point_right:函数总结

- 函数只能判断是否为 nil ，不支持比较操作。
- 开发过程中非必要不要修改引用类型的值.
- 开发过程中非必要不使用具名返回。
- 开发过程中非必要不使用匿名函数。
- defer设计返回值，应注意是否是具名返回（或者指针），以防止结果达不到预期。
- 开发过程中禁止在循环里使用defer。
- 除main函数特殊场景外（如关闭资源、连接等），其他必须检查err错误。
- 开发过程中非必要不使用panic。
- recover函数必须在defer中使用，:point_right:只能捕获当前go程的panic。

# 方法

# 接口

接口是合约，是一系列方法的集合。

只要目标类型方法集包含接口全部方法，就视为实现该接口，无需显示声明。 

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Closer interface {
    Close() error
}

type ReadWriter interface {
    Reader
    Writer
}

type ReadWriteCloser interface {
    Reader
    Writer
    Closer
}
```

- 只能声明方法，不能有字段, 不能实现。 
- 申明接口时，尽量保证接口小，可匿名嵌入其他接口。 
- 接口名通常以 er 作为名称后缀。 
- 空接口（ interface{} , any ）没有任何方法声明。

## 空接口可以被赋值给任何对象

```go
var a any = 123
a = "stb"
```

## 空值判断

接口内部由两个字段组成：类型 和 值。 

只有两字段都为 nil 时，接口才等于 nil 。可利用反射完善判断结果。

相同类型的接口底层可以做 比较时，可以用 == 对比

```go
func main() {
    var t1, t2 any
    println(t1 == nil, t1 == t2) // true, true

    t1, t2 = 100, 100
    println(t1 == t2) // true

    // t1, t2 = map[string]int{}, map[string]int{}
    // println(t1 == t2)
    // ~~~~~~~~ panic: comparing uncomparable type map
}
```

## 接口实现

目标类型方法集包含接口全部方法，就视为实现该接口，（接口本身是指针）

父集接口 可以 隐形转换成其子集，反之，子集不能转换成父集

```go
package main

type Aer interface {
	toString(string) string
}

type Xer interface {
	// Aer
	test()
	toString(string) string
}

type N struct{}

func (*N) test() {}

func (N) toString(s string) string { return s }

func main() {
	var x Xer = &N{} // super
	var a Aer = x // sub
	a.toString("abc")
	// var x2 Xer = a
	// ~ Aer does not implement Xer (missing test method)

	// x2 := Xer(a)
	// ~ Aer does not implement Xer
}
```

## 类型断言

类型断言检查它操作对象的动态类型是否和断言的类型匹配。使用`i.(t)`及interface.(type)的格式来判断interface的底层类型

```go
var w io.Writer
w = os.Stdout
f := w.(*os.File)      // success: f == os.Stdout
c := w.(*bytes.Buffer) // panic: interface holds *os.File, not *bytes.Buffer

```

使用ok来判断类型是否正常转换

```go
c, ok := w.(*bytes.Buffer) 
if !ok {
    errThings()
}
```

> 不使用ok，有panic风险

使用 switch 来判断类型

```go
switch i.(type) {
case nil:       // ...
case int, uint: // ...
case bool:      // ...
case string:    // ...
default:        // ...
}
```

>  switch i.(type) 不支持 fallthrough

## 让编译器检查

借助编译器 使用 `var name interfaceName = some` 判断接口是否实现

```go
type X int
var _ fmt.Stringer = X(0) //
// ~ X does not implement fmt.Stringer (missing String method)

type FuncString func() string
    func (f FuncString) String() string {
        return f()
    }

func main() {
    f := func() string {
   		return "hello, world!"
	}
    
	var t fmt.Stringer = FuncString(f)
	fmt.Println(t)
}
```

## :point_right:接口总结

- 类型断言时一定要使用ok模式，检查是否正常转换
- 接口的switch不支持`fallthrough`
- 使用借助编译器或者开发工具检查接口是否实现

# Channel - 通道

golang 鼓励使用 CSP 通道，以通信来代替内存共享，实现并发安全

通道（channel）行为类似消息队列。不限收发人数，不可重复消费。

channel属于引用类型 和map类似，channel也对应一个make创建的底层数据结构的引用。

```go
ch := make(chan int) // ch has type 'chan int'
```

## 同步channel

同步channel没有数据缓冲区，须收发双方到场直接交换数据。

- 发送数据阻塞，直到有另一方准备妥当或通道关闭。 
- 可通过 cap == 0 判断为无缓冲通道。

```go
ch := make(chan struct{})
```

## 异步channel

通道自带固定大小缓冲区（buffer）

- 发送数据有空位时，不会阻塞。 
- 用 cap 、 len 获取缓冲区大小和当前缓冲数据量。

```go
ch := make(chan struct{},10)
```

## 消息收发

使用`chan <- value`向channel发送消息。

```go
ch <- v
```

ok模式取值，如果ok值为false，通道被关闭。

```go
v,ok := <- ch
```

for-range变量取值

```go
for  v := range ch {
    ...
}
```

## 单向channel

发送消息

```go
chan<- type
```

接收消息

```go
<-chan int
```

只能由发送方（生成者）关闭channel。如下

```go
c := make(chan int)
var send chan<- int = c
var recv <-chan int = c

func squarer(send chan<- int) {
    defer close(send)
}

func printer(recv <-chan int) {
 
}
```

## select多路复用

用 select 语句处理多个通道，随机选择可用通道做收发操作。 

将失效通道置为 nil （阻塞，不可用），用作结束判断。

```go
select {
case x, ok = <-ch1: 
    if !ok { 
        ch1 = nil 
    }
case x, ok = <-ch2: 
    if !ok { 
        ch2 = nil
    }
default:
    doSomething()
}
```

空 select 语句，一直阻塞。

```go
select{}
```

## 关闭channel

使用close关闭channel

```go
close(ch)
```

对于 closed 或 nil 通道，规则如下： 

- 无论收发， nil 通道都会阻塞。 
- 不能关闭 nil 通道。 
- 重复关闭通道，引发 panic ！ 
- 向已关闭通道发送数据，引发 panic ！ 
- 从已关闭通道接收数据，返回缓冲数据或零值。

## :point_right:channel总结

- 开发时为了快速消费，channel的容量要么为0，要么为1，不允许有过多的缓冲。
- 两个相同类型的channel可以使用==运算符比较。如果两个channel引用的是相同的对象，那么比较的结果为真。一个channel也可以和nil进行比较。
- channel作为参数传递时，为了安全和规范，只能传递单向channel。
- 只能有生产者关闭channel。



# Goroutine - go程

在Go语言中，每一个并发的执行单元叫作一个goroutine。

简单将 goroutine 归为协程（coroutine）并不合适。 类似多线程和协程的综合体，最大限度提升执行效率，发挥多核处理能力。 

**goroutine优点**

- 极小初始栈（2 KB），按需扩张。 
- 无锁内存分配和复用，提升并发性能。 
- 调度器平衡任务队列，充分利用多处理器。 
- 线程自动休眠和唤醒，减少内核开销。 
- 基于信号（signal）实现抢占式任务调度。

所有用户代码都以 goroutine 执行，包括 main.main 入口函数。 当一个程序启动时，其主函数即在一个单独的goroutine中运行，我们叫它main goroutine。

进程结束，不会等待其他正在执行或尚未执行的任务。

```go
func main(){
    go foo()
}
```

## 等待任务结束

等待任务结束方式

- time.Sleep  ：睡眠等待。不推荐使用。
- channel : 信号通知。
- WaitGroup ：等待多个任务结束。 
- Context ：上下文通知。 
- Mutex ：锁阻塞。不推荐使用

**channel**

channel 的方式,通知消息行为 可以使用空结构体。

```go
package main

import (
    "time"
    "fmt"
)

func Process(ch chan int) {
    //Do some work...
    time.Sleep(time.Second)

    ch <- 1 //管道中写入一个元素表示当前协程已结束
}

func main() {
    channels := make([]chan int, 10) //创建一个10个元素的切片，元素类型为channel

    for i:= 0; i < 10; i++ {
        channels[i] = make(chan int) //切片中放入一个channel
        go Process(channels[i])      //启动协程，传一个管道用于通信
    }

    for i, ch := range channels {  //遍历切片，等待子协程结束
        <-ch
        fmt.Println("Routine ", i, " quit!")
    }
}
```

**WaitGroup**

添加计数（ WaitGroup.Add ）应在创建任务和等待之前，否则可能导致等待提前解除。

```go
package main

import "sync"

func main() {
	var wg sync.WaitGroup

	for i := 0; i < 10; i++ {
		wg.Add(1)
		go func(id int) {
			defer wg.Done()
			println(id, "done.")
		}(i)
	}

	wg.Wait()
}
```

**context**

```go
package main

import (
	"context"
	"fmt"
)

func main() {
	gen := func(ctx context.Context) <-chan int {
		dst := make(chan int)
		n := 1
		go func() {
			for {
				select {
				case <-ctx.Done():
					return // returning not to leak the goroutine
				case dst <- n:
					n++
				}
			}
		}()
		return dst
	}

	ctx, cancel := context.WithCancel(context.Background())
	defer cancel() // cancel when we are finished consuming integers

	for n := range gen(ctx) {
		fmt.Println(n)
		if n == 5 {
			break
		}
	}
}
```

**mutex**

```go
package main

import "sync"

func main() {
	var lock sync.Mutex
	lock.Lock()
	go func() {
		defer lock.Unlock()
		println("done.")
	}()
	lock.Lock()
	lock.Unlock()
	println("exit")
}
```

## 终止任务

主动结束任务，有以下几种方式。 

- 调用 runtime.Goexit 终止任务。 
- 调用 os.Exit 结束进程。

## GOMAXPROCS - 进程数（ing）

一般来讲，程序运行时就将GOMAXPROCS大小设置为CPU核数，可让Go程序充分利用CPU。 

GOMAXPROCS 并不是越大越好，在某些IO密集型的应用里，这个值可能并不意味着性能最好。 理论上当某个Goroutine进入系统调用时，会有一个新的M被启用或创建，继续占满CPU。任何时候都只有几个参与并发任务执行，其他处于休眠状态（可以查看GMP模型）。

运行时可以设置GOMAXPROCS的环境变量,用time命令查看运行时间。程序中可以使用`runtime.GOMAXPROCS()`设置P的个数

```bash
time GOMAXPROCS=1 ./test
```

除并发控制外，还可以通过 ` GOGC` 、 `GOMEMLIMIT `调整垃圾回收频率和最大阈值。

## 挂起

使用`runtime.Gosched()`暂时挂起任务，释放线程去执行其他任务。当前任务被放回任务队列，等待下次被某个线程重新获取后继续执行。

```go
go func() {
    defer wg.Done()
    <- b
    for i := 0; i < 5; i++ {
        println("b", i)
        if i == 2 { 
            runtime.Gosched() 
        }
    }
}()
```

## 顺序执行

```go
func main() {
    const CNT = 5
    var wg sync.WaitGroup
    wg.Add(CNT)
    var chans [CNT]chan struct{}

    for i := 0; i < CNT; i++ {
        chans[i] = make(chan struct{})
        go func(id int){
            defer wg.Done()
            <- chans[id]
            println(id)
        }(i)
    }
    // 次序（延时，给调度器时间处理）
    for _, x := range []int{4, 0, 1, 3, 2} {
        close(chans[x])
    	time.Sleep(time.Millisecond * 10)
    }
    wg.Wait()
}
```

## 线程管理

Go 语言既然专门将线程进一步抽象为 Goroutine，自然也就不希望我们对线程做过多的操作，事实也是如此， 大部分的用户代码并不需要线程级的操作。

 `runtime.LockOSThread` 会将当前 Goroutine 锁在一个固定的 OS 线程上执行。

`LockOSThread/UnlockOSThread` 也是目前唯一一个能够让 M 退出的做法（将 Goroutine 锁在 OS 线程上，且在 Goroutine 死亡退出时不调用 Unlock 方法）。 

> 详情看原理

## :point_right:goroutine总结

- 开发时要注意保证go程顺利执行与结束。
- 不要频繁调用 GOMAXPROCS ，它会导致 STW，影响性能。
- 使用WaitGroup和context来控制go程活动。
- 使用channel通信来代替内存共享（csp）
- 以参数方式传入外部容器，但要避免和其他并发任务竞争（data race）。

# sync-共享变量安全

通道并非用来取代锁，各有不同使用场景。 

通道解决高级别逻辑层次并发架构，锁则用来保护低级别局部代码（多goroutine之间的共享变量）安全。 

- 竟态条件：多线程同时读写共享资源（竟态资源）。 
- 临界区：读写竟态资源的代码片段。 

锁的种类：

- 互斥锁：同一时刻，只有一个线程能进入临界区。 
- 读写锁：写独占（其他读写均被阻塞），读共享。 
- 信号量：允许指定数量线程进入临界区。 
- 自旋锁：失败后，以循环积极尝试。（无上下文切换，小粒度） 
- 悲观锁：操作前独占锁定。 
- 乐观锁：假定无竞争，后置检查。（Lock Free, CAS）

## 竞争检测

Go已经为我们准备好动态分析工具，竞争检查器（the race detector）。只要在go build，go run或者go test命令后面加上`-race`的flag即可。

```bash
go build -race && ./test
```

## sync.Mutex互斥锁

Go 标准库中提供了 sync.Mutex 互斥锁类型及其两个方法：

- Lock 加锁
- Unlock 释放锁

```go
type data struct {
	sync.Mutex
}

func (d *data) do() {
	prefix()
	func() {
		d.Lock()
		defer d.Unlock()
		doing()
	}()
	suffix()
}
```

- 互斥锁要尽早释放
- 不支持重入锁（连续多次锁入，释放）。

## sync.RWMutex读写锁

读写锁分为读锁和写锁，读锁是允许同时执行的，但写锁是互斥的。一般来说，有如下几种情况：

- 读锁之间不互斥，没有写锁的情况下，读锁是无阻塞的，多个协程可以同时获得读锁。
- 写锁之间是互斥的，存在写锁，其他写锁阻塞。
- 写锁与读锁是互斥的，如果存在读锁，写锁阻塞，如果存在写锁，读锁阻塞。

Go 标准库中提供了 sync.RWMutex 互斥锁类型及其四个方法：

- Lock 加写锁
- Unlock 释放写锁
- RLock 加读锁
- RUnlock 释放读锁

```go
type RWLock struct {
	count int
	mu    sync.RWMutex
}

func (l *RWLock) Write() {
	l.mu.Lock()
    defer l.mu.Unlock()
	l.count++
	
}

func (l *RWLock) Read() {
	l.mu.RLock()
    defer l.mu.RUnlock()
	_ = l.count
}
```

## sync.Cond条件变量

`sync.Cond` 条件变量用来协调想要访问共享资源的那些 goroutine，当共享资源的状态发生变化的时候，它可以用来通知被互斥锁阻塞的 goroutine。

使用场景：

`sync.Cond` 经常用在多个 goroutine 等待，一个 goroutine 通知（事件发生）的场景。如果是一个通知，一个等待，使用互斥锁或 channel 就能搞定了。比如：有一个协程在异步地接收数据，剩下的多个协程必须等待这个协程接收完数据，才能读取到正确的数据。

```go
var done = false

func read(name string, c *sync.Cond) {
	c.L.Lock()
	for !done {
		c.Wait()
	}
	log.Println(name, "starts reading")
	c.L.Unlock()
}

func write(name string, c *sync.Cond) {
	log.Println(name, "starts writing")
	time.Sleep(time.Second)
	c.L.Lock()
	done = true
	c.L.Unlock()
	log.Println(name, "wakes all")
	c.Broadcast()
}

func main() {
	cond := sync.NewCond(&sync.Mutex{})

	go read("reader1", cond)
	go read("reader2", cond)
	go read("reader3", cond)
	write("writer", cond)

	time.Sleep(time.Second * 3)
}
```

## sync.Once单次执行

`sync.Once` 是 Go 标准库提供的使函数只执行一次的实现，常应用于单例模式，例如初始化配置、保持数据库连接等。

```go
func main() {
    var once sync.Once
    f1 := func() { println("1") }
    f2 := func() { println("2") }
    once.Do(f1)
    // 以下目标函数不会执行。
    once.Do(f1)
    once.Do(f2)
    once.Do(f2)
}
```

- 声明一个 `sync.Once` 只能执行一次.Do()函数

## sync.Pool复用临时对象

sync.Pool 保存和复用临时对象，减少内存分配，降低 GC 压力。

sync.Pool 的大小是可伸缩的，高负载时会动态扩容，存放在池中的对象如果不活跃了会被自动清理。

- `Get()` 用于从对象池中获取对象，因为返回值是 `interface{}`，因此需要类型转换。
- `Put()` 则是在对象使用完毕后，返回对象池。

```go
var studentPool = sync.Pool{
    New: func() interface{} { 
        return new(Student) 
    },
}

stu := studentPool.Get().(*Student)
json.Unmarshal(buf, stu)
studentPool.Put(stu)
```

## sync.Map并发安全的map

Go语言中的 map 在并发情况下，只读是线程安全的，同时读写是线程不安全的。

```go
package main

import (
      "fmt"
      "sync"
)

func main() {
    var scene sync.Map

    // 将键值对保存到sync.Map
    scene.Store("greece", 97)
    scene.Store("london", 100)
    scene.Store("egypt", 200)

    // 从sync.Map中根据键取值
    fmt.Println(scene.Load("london"))

    // 根据键删除对应的键值对
    scene.Delete("london")

    // 遍历所有sync.Map中的键值对
    scene.Range(func(k, v interface{}) bool {
        fmt.Println("iterate:", k, v)
        return true
    })
}
```



## :point_right:锁总结

- 开发时，为了效率和规范，尽量保证锁的颗粒度小，及早释放。



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