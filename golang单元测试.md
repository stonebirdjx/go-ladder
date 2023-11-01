<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [单元测试](#%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95)
  - [常用参数](#%E5%B8%B8%E7%94%A8%E5%8F%82%E6%95%B0)
  - [子测试](#%E5%AD%90%E6%B5%8B%E8%AF%95)
  - [覆盖率](#%E8%A6%86%E7%9B%96%E7%8E%87)
  - [并行](#%E5%B9%B6%E8%A1%8C)
  - [Helper](#helper)
  - [Cleanup 清理](#cleanup-%E6%B8%85%E7%90%86)
- [基准（性能）测试](#%E5%9F%BA%E5%87%86%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95)
  - [常用参数](#%E5%B8%B8%E7%94%A8%E5%8F%82%E6%95%B0-1)
  - [子测试](#%E5%AD%90%E6%B5%8B%E8%AF%95-1)
  - [性能剖析](#%E6%80%A7%E8%83%BD%E5%89%96%E6%9E%90)
- [Fuzz模式测试](#fuzz%E6%A8%A1%E5%BC%8F%E6%B5%8B%E8%AF%95)
  - [常用参数](#%E5%B8%B8%E7%94%A8%E5%8F%82%E6%95%B0-2)
- [Example文档测试](#example%E6%96%87%E6%A1%A3%E6%B5%8B%E8%AF%95)
- [uber/mock](#ubermock)
  - [mockgen](#mockgen)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

golang支持的测试类型：

- 单元测试（unit testing）：对程序模块进行正确性检验。
- 基准测试（benchmark）：对某项性能指标进行测量和评估。
- 模糊测试（fuzz testing）：输入随机数据，发现潜在错误。
- 文档测试（example）(函数示例)：对函数的进行使用展示，通过go doc可以查看。

> 测试时一般会自定义一些测试数据，为了不影响原包，建议将测试包的包名加上 _test,与原包区分

# 单元测试

元测试函数的名字必须以Test开头，可选的后缀名必须以大写字母开头，函数参数必须是 `*testing.T`

```Go
func TestSin(t *testing.T) { /* ... */ }
func TestCos(t *testing.T) { /* ... */ }
func TestLog(t *testing.T) { /* ... */ }
```

执行

```Go
go test ./ -cover
// go test ./... -coverprofile=cover.out
// go tool cover -html=cover.out
```

## 常用参数

- `-count num` : 执行次数（默认1）,带参数可以导致缓存失效。或者使用 go clean -testcache清除缓存
- `-list` : 用正则表达式列出测试函数名，不执行。
- `-run pattern`：用正则表达式执行要执行的测试函数。
- `-timeout` : 超时 panic!（默认 10m） -
- `-v` : 输出详细信息。
- `-p, -parallel` 用于设置 GOMAXPROCS ，这会影响并发执行。
- `-coverprofile file`：将代码覆盖率信息写入指定的文件中，以便稍后生成覆盖率报告。

## 子测试

使用t.Run("name",Func)运行子测试

```go
func (t *T) Run(name string, f func(t *T)) bool
```

子测试优点

- 将测试函数拆分为子测试，更符合套件（suite）模式。
- 便于编写初始化（setup）和清理（teardown）逻辑。
- 表驱动（table driven）时，拆分成多个并发测试。
- 便于观察子测试时间，不用考虑外部环境影响。

```Go
// Parallel 会挂起当前测试，让 Run 提前退出。
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

## 覆盖率

代码覆盖率（code coverage）是度量测试完整和有效性的一种手段。

- 通过覆盖率值，分析测试代码编写质量。
- 检测是否提供完备测试条件，是否执行了全部目标代码。
- 量化测试，让白盒测试真正起到应有的质量保障作用。

```Bash
go test -cover ./stb_test

ok stb_test 0.005s coverage: 66.7% of statements
```

## 并行

默认情况下包内用例串行、多包并行,参数 -p, -parallel 用于设置 GOMAXPROCS ，这会影响并发执行。

```Bash
go test path/stb/ -v -run "Add" -p 2
```

## Helper

Helper 将调用函数标记为测试帮助函数。 当打印文件和行信息时，该功能将被跳过。 Helper 可以从多个 goroutine 中同时调用。

```Go
func assert(t *testing.T, b bool) {
        t.Helper()
        if !b { t.Fatal("assert fatal") }
}
```

## Cleanup 清理

Cleanup 注册一个在测试（或子测试）及其所有子测试完成时调用的函数。 清理函数将按最后添加、最先调用的顺序调用。

t.Cleanup()为测试函数注册清理函数，在测试结束时执行。

- 如注册多个，则按 FILO 顺序执行。
- 即便发生 panic ，也能确保清理函数执行。

和 defer 的区别：即便在其他函数内注册，也会等测试结束后再执行

```go
func createUser(t *testing.T, db *gorm.DB) {
	user := User{Username: "jane", Password: "doe123"}
	if err := db.Create(&user).Error; err != nil {
		t.Fatal(err)
	}

	t.Cleanup(func() {
		db.Delete(&user)
	})
}

func TestServer(t *testing.T) {
	db, err := gorm.Open("sqlite3", "./cleanups_test.db")
	if err != nil {
		t.Fatal(err)
	}
	defer db.Close()
	createUser(t, db)
	r := Router(db)
    // Snipped for brewity...
}
```

# 基准（性能）测试

基准测试是测量一个程序在固定工作负载下的性能。

基准测试函数和普通测试函数写法类似，但是以Benchmark为前缀名，并且带有一个*testing.B类型的参数；

```Go
import "testing"

func BenchmarkIsPalindrome(b *testing.B) {
    for i := 0; i < b.N; i++ {
        IsPalindrome("A man, a plan, a canal: Panama")
    }
}

go test -bench=.
go test -bench=IsPalindrome

// 仅执行性能测试，可用 -run NONE 忽略单元测试。
go test -v -bench . -run None ./mylib
```

## 常用参数

- `-bench regex` ：指定要运行的测试。（正则表达式， -bench . ）
- `-benchtime num`：单次测试运行时间或循环次数。（默认 1s，1m20s，100x）
- `-count  num`：执行几轮测试。（benchtime * count）
- `-cpu`：测试所用 CPU 核心数。（ -cpu 1,2,4 执行三轮测试）
- `-list` : 列出测试函数，不执行。
- `-benchmem` ：显示内存分配（堆）信息。

## 子测试

```Go
func BenchmarkSubs(b *testing.B) {
        b.Log("setup")
        b.Cleanup(func(){ b.Log("cleanup") })
        b.Run("A", BenchmarkA)
    b.Run("B", BenchmarkB)
    b.Run("C", BenchmarkC)
}
```

## 性能剖析

```Go
go test -bench=. -cpuprofile=cpu.out
go test -bench=. -blockprofile=block.out
go test -bench=. -memprofile=mem.out

go tool pprof -text -nodecount=10 ./http.test cpu.out
```

# Fuzz模式测试

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

```bash
go test -v -fuzz Add -fuzztime 20s ./mylib
```

## 常用参数

- `-fuzz  regex`: 测试目标。
- `-fuzztime` : 时长或次数。（ 1m20s , 100x ）

# Example文档测试

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

示例行数总结

- 例子测试函数名需要以"Example"开头；
- 检测单行输出格式为“// Output: <期望字符串>”；
- 检测多行输出格式为“// Output: \ <期望字符串> \ <期望字符串>”，每个期望字符串占一行；
- 检测无序输出格式为"// Unordered output: \ <期望字符串> \ <期望字符串>"，每个期望字符串占一行；
- 测试字符串时会自动忽略字符串前后的空白字符；
- 如果测试函数中没有“Output”标识，则该测试函数不会被执行；
- 执行测试可以使用`go test <xxx_test.go>`，此时仅执行特定文件中的测试函数；

# uber/mock

项目源自Google的golang/mock repo。不幸的是，Google 不再维护这个项目.

```Bash
go install go.uber.org/mock/mockgen@latest
```

## mockgen

```
source mode
mockgen -source=foo.go [other options]
reflect mode
mockgen database/sql/driver Conn,Driver

# Convenient for `go:generate`.
mockgen . Conn,Driver
```

常用Flags:mockgen 命令用于生成模拟类的源代码。 它支持以下标志：

- `-source`：包含要模拟的接口的文件。
- `-destination`：写入结果源代码的文件。 如果不设置此项，代码将打印到标准输出。
- `-package`：用于生成的模拟类源代码的包。 如果不设置此项，包名称将与输入文件的包连接起来。

```Bash
mockgen -source=./person/male.go -destination=./mock/male_mock.go -package=mock
```