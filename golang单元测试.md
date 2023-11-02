<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [:point_right:单元测试](#point_right%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95)
  - [常用参数](#%E5%B8%B8%E7%94%A8%E5%8F%82%E6%95%B0)
  - [:point_right:子测试](#point_right%E5%AD%90%E6%B5%8B%E8%AF%95)
  - [:point_right:覆盖率](#point_right%E8%A6%86%E7%9B%96%E7%8E%87)
  - [并行](#%E5%B9%B6%E8%A1%8C)
  - [Helper](#helper)
  - [Cleanup 清理](#cleanup-%E6%B8%85%E7%90%86)
- [:point_right:基准（性能）测试](#point_right%E5%9F%BA%E5%87%86%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95)
  - [常用参数](#%E5%B8%B8%E7%94%A8%E5%8F%82%E6%95%B0-1)
  - [子测试](#%E5%AD%90%E6%B5%8B%E8%AF%95)
  - [性能剖析](#%E6%80%A7%E8%83%BD%E5%89%96%E6%9E%90)
- [Fuzz模式测试](#fuzz%E6%A8%A1%E5%BC%8F%E6%B5%8B%E8%AF%95)
  - [常用参数](#%E5%B8%B8%E7%94%A8%E5%8F%82%E6%95%B0-2)
- [:point_right:Example文档测试](#point_rightexample%E6%96%87%E6%A1%A3%E6%B5%8B%E8%AF%95)
- [:point_right:uber/mock](#point_rightubermock)
  - [:point_right:mockgen](#point_rightmockgen)
- [:point_right:testify](#point_righttestify)
  - [:point_right:assert - 断言](#point_rightassert---%E6%96%AD%E8%A8%80)
  - [:point_right:mock](#point_rightmock)
  - [:point_right:suite](#point_rightsuite)
- [:point_right:goconvey](#point_rightgoconvey)
- [:point_right:gostub](#point_rightgostub)
  - [为全局变量打桩](#%E4%B8%BA%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F%E6%89%93%E6%A1%A9)
  - [为函数打桩](#%E4%B8%BA%E5%87%BD%E6%95%B0%E6%89%93%E6%A1%A9)
  - [为过程打桩](#%E4%B8%BA%E8%BF%87%E7%A8%8B%E6%89%93%E6%A1%A9)
- [:point_right:monkey](#point_rightmonkey)
  - [为函数打桩](#%E4%B8%BA%E5%87%BD%E6%95%B0%E6%89%93%E6%A1%A9-1)
  - [为方法打桩](#%E4%B8%BA%E6%96%B9%E6%B3%95%E6%89%93%E6%A1%A9)
- [gomockey](#gomockey)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

golang支持的测试类型：

- 单元测试（unit testing）：对程序模块进行正确性检验。
- 基准测试（benchmark）：对某项性能指标进行测量和评估。
- 模糊测试（fuzz testing）：输入随机数据，发现潜在错误。
- 文档测试（example）(函数示例)：对函数的进行使用展示，通过go doc可以查看。

> 测试时一般会自定义一些测试数据，为了不影响原包，建议将测试包的包名加上 _test,与原包区分

# :point_right:单元测试

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

## :point_right:子测试

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

## :point_right:覆盖率

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

# :point_right:基准（性能）测试

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

# :point_right:Example文档测试

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

# :point_right:uber/mock

项目源自Google的golang/mock repo。不幸的是，Google 不再维护这个项目.

```Bash
go install go.uber.org/mock/mockgen@latest
```

```go
type Foo interface {
  Bar(x int) int
}

func SUT(f Foo) {
 // ...
}

func TestFoo(t *testing.T) {
  ctrl := gomock.NewController(t)

  m := NewMockFoo(ctrl)

  // Asserts that the first and only call to Bar() is passed 99.
  // Anything else will fail.
  m.
    EXPECT().
    Bar(gomock.Eq(99)).
    Return(101)

  SUT(m)
}
```

## :point_right:mockgen

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

# :point_right:testify

`testify`核心有三部分内容：

- `assert`：断言；
- `mock`：测试替身；
- `suite`：测试套件。

github：https://github.com/stretchr/testify

## :point_right:assert - 断言

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

## :point_right:mock

提供了一种轻松编写模拟对象的机制，可以在编写测试代码时使用模拟对象代替真实对象。

```go
package service

type db struct{}

// DB is fake database interface.
type DB interface {
	FetchMessage(lang string) (string, error)
	FetchDefaultMessage() (string, error)
}

type greeter struct {
	database DB
	lang     string
}

// GreeterService is service to greet your friends.
type GreeterService interface {
	Greet() string
	GreetInDefaultMsg() string
}

func (d *db) FetchMessage(lang string) (string, error) {
	// in real life, this code will call an external db
	// but for this sample we will just return the hardcoded example value
	if lang == "en" {
		return "hello", nil
	}
	if lang == "es" {
		return "holla", nil
	}
	return "bzzzz", nil
}

func (d *db) FetchDefaultMessage() (string, error) {
	return "default message", nil
}

func (g greeter) Greet() string {
	msg, _ := g.database.FetchMessage(g.lang) // call database to get the message based on the lang
	return "Message is: " + msg
}

func (g greeter) GreetInDefaultMsg() string {
	msg, _ := g.database.FetchDefaultMessage() // call database to get the default message
	return "Message is: " + msg
}

func NewDB() DB {
	return new(db)
}

func NewGreeter(db DB, lang string) GreeterService {
	return greeter{db, lang}
}
```

test.go

```go
package service

import (
	"testing"

	"github.com/stretchr/testify/assert"
	"github.com/stretchr/testify/mock"
)

type dbMock struct {
	mock.Mock
}

func (d *dbMock) FetchMessage(lang string) (string, error) {
	args := d.Called(lang)
	return args.String(0), args.Error(1)
}

func (d *dbMock) FetchDefaultMessage() (string, error) {
	args := d.Called()
	return args.String(0), args.Error(1)
}

func TestMockMethodWithoutArgs(t *testing.T) {
	theDBMock := dbMock{}                                            // create the mock
	theDBMock.On("FetchDefaultMessage").Return("foofofofof", nil)    // mock the expectation
	g := greeter{&theDBMock, "en"}                                   // create greeter object using mocked db
	assert.Equal(t, "Message is: foofofofof", g.GreetInDefaultMsg()) // assert what actual value that will come
	theDBMock.AssertNumberOfCalls(t, "FetchDefaultMessage", 1)       // we can assert how many times the mocked method will be called
	theDBMock.AssertExpectations(t)                                  // this method will ensure everything specified with On and Return was in fact called as expected
}

func TestMockMethodWithArgs(t *testing.T) {
	theDBMock := dbMock{}
	theDBMock.On("FetchMessage", "sg").Return("lah", nil) // if FetchMessage("sg") is called, then return "lah"
	g := greeter{&theDBMock, "sg"}
	assert.Equal(t, "Message is: lah", g.Greet())
	theDBMock.AssertExpectations(t)
}

func TestMockMethodWithArgsIgnoreArgs(t *testing.T) {
	theDBMock := dbMock{}
	theDBMock.On("FetchMessage", mock.Anything).Return("lah", nil) // if FetchMessage(...) is called with any argument, please also return lah
	g := greeter{&theDBMock, "in"}
	assert.Equal(t, "Message is: lah", g.Greet())
	theDBMock.AssertCalled(t, "FetchMessage", "in")
	theDBMock.AssertNotCalled(t, "FetchMessage", "ch")
	theDBMock.AssertExpectations(t)
	mock.AssertExpectationsForObjects(t, &theDBMock)
}

func TestMatchedBy(t *testing.T) {
	theDBMock := dbMock{}
	theDBMock.On("FetchMessage", mock.MatchedBy(func(lang string) bool { return lang[0] == 'i' })).Return("bzzzz", nil) // all of these call FetchMessage("iii"), FetchMessage("i"), FetchMessage("in") will match
	g := greeter{&theDBMock, "izz"}
	msg := g.Greet()
	assert.Equal(t, "Message is: bzzzz", msg)
	theDBMock.AssertExpectations(t)
}
```

## :point_right:suite

该套件包提供了您可能习惯于更常见的面向对象语言的功能。 有了它，您可以将测试套件构建为结构，在结构上构建设置/拆卸方法和测试方法，并按照正常情况使用“go test”运行它们。

```go
```

# :point_right:goconvey

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

# :point_right:gostub

## 为全局变量打桩

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
```

## 为函数打桩

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

## 为过程打桩

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

# :point_right:monkey

monkey是一个Go单元测试中十分常用的打桩工具，它在运行时通过汇编语言重写可执行文件，将目标函数或方法的实现跳转到桩实现，其原理类似于热补丁。

monkey库很强大，但是使用时需注意以下事项：

- monkey不支持内联函数，在测试的时候需要通过命令行参数`-gcflags=-l`关闭Go语言的内联优化。
- monkey不是线程安全的，所以不要把它用到并发的单元测试中。

github:https://github.com/bouk/monkey

## 为函数打桩

```go
func MyFunc(uid int64)string{
	u, err := varys.GetInfoByUID(uid)
	if err != nil {
		return "welcome"
	}
	// ...
  
	return fmt.Sprintf("hello %s\n", u.Name)
}

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

## 为方法打桩

```go

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

// TestUserGetInfo
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

# gomockey

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

test详情： https://github.com/agiledragon/gomonkey/tree/master/test