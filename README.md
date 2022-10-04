<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [前言](#%E5%89%8D%E8%A8%80)
- [HelloWorld](#helloworld)
- [小刀弑牛](#%E5%B0%8F%E5%88%80%E5%BC%91%E7%89%9B)
- [程序设计](#%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)
  - [package - 包](#package---%E5%8C%85)
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
- [测试框架](#%E6%B5%8B%E8%AF%95%E6%A1%86%E6%9E%B6)
- [后续建议](#%E5%90%8E%E7%BB%AD%E5%BB%BA%E8%AE%AE)
- [参考资料](#%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 前言

:speak_no_evil:如果能成为golang的cookbook就好了:see_no_evil:

整理知识，传播智慧，好好学习，天天向上。

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

# 程序设计

## package - 包

- 合理使用包名、成员名、导出符号、init函数。
- 了解相关环境变量。
- 注意包的依赖关系，导入包不要成环。
- 使用模块管理。

[package笔记](https://github.com/stonebirdjx/go-ladder/blob/master/%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/package-%E5%8C%85.md)



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

# 测试框架









# 后续建议

强者总是比较孤独,请坚持学习！送上一句黑鸡汤：

> "The last thing you want is to look back on your life and wonder... if only." --by caicloud

# 参考资料

[雨痕大佬的go学习笔记](https://github.com/qyuhen/book)

[饶全成](https://github.com/qcrao)