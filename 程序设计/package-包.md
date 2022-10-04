# 包声明

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

# 导出符号

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

# init函数 - 初始化包

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

# 内部包

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

# 导入包

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

# module - 模块

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

# go work - 工作空间

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

# go vendor - 打包依赖源码

将依赖包复制到 ./vendor目录

```go
go mod vendor
```

禁用工作空间，以 ./vendor的方式编译

```go
go build -mod vendor
```

# 包相关的环境变量

模块感知（module-aware）模式，相关环境变量说明。 

- GO111MODULE ：模块感知模式开关。（默认：on） 
- GOMODCACHE ：模块缓存路径。 
- GOPROXY ：模块代理服务列表。 
- GOSUMDB ：模块数据安全校验数据库。 
- GOPRIVATE ：私有模块路径，绕过 GOPROXY 直接获取。 

> GOPRIVATE 是 GONOPROXY 、 GONOSUMDB 的默认值。

# :point_right:包总结

- 包名通常与目录名一致，注意命名格式和规范。
- 特殊含义的包名：`main` 用户可执行文件入口包 ； `all` 所有包，包括标准库和依赖项。`std` 标准库。 `cmd` 工具链。可使用`go list`查看所有的包，
- 一般情况下禁止导出全局变量。如需使用包的全局变量 **非导出符号+导出函数的形式。**
- 包内成员命名应避免使用包名作为前缀。如`stream.StreamBuffer ` 会显得很累赘。
- 避免使用init函数。
- 注意包依赖关系，禁止使用 `.` 来简化导入包。
- 使用`go list  -m -versions` 来查看所有包的版本
- 关闭GOPROXY，可以避免在线获取模块
- 源文件必须是 `utf-8` 的格式。