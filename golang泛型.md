<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [类型约束](#%E7%B1%BB%E5%9E%8B%E7%BA%A6%E6%9D%9F)
- [定义类型约束](#%E5%AE%9A%E4%B9%89%E7%B1%BB%E5%9E%8B%E7%BA%A6%E6%9D%9F)
- [泛型类型](#%E6%B3%9B%E5%9E%8B%E7%B1%BB%E5%9E%8B)
- [泛型函数](#%E6%B3%9B%E5%9E%8B%E5%87%BD%E6%95%B0)
- [泛型结构体](#%E6%B3%9B%E5%9E%8B%E7%BB%93%E6%9E%84%E4%BD%93)
  - [无泛型方法](#%E6%97%A0%E6%B3%9B%E5%9E%8B%E6%96%B9%E6%B3%95)
- [类型转换](#%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
- [总结](#%E6%80%BB%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

泛型（generic）是一种代码复用技术，是一种编程范式，这种范式与特定的类型无关，泛型允许在函数和类型的实现中使用某个类型集合中的任何一种类型。

资料：https://go.googlesource.com/proposal/+/refs/heads/master/design/43651-type-parameters.md#No-parameterized-methods

实现方式：

- 模板（stenciling）：为每次调用生成代码实例，即便类型参数相同。
  - 模板方式性能最佳，但编译时间较长，且生成文件较大。
- 字典（dictionaries）：单份代码实例，以字典传递类型参数信息。
  - 字典方式代码最少，但复杂度较高，且性能最差。

在Go语言中泛型叫做`Type Parameter`(类型参数).

实现：GCShape

```bash
go build -gcflags "-N -l -S"
```

# 类型约束

```go
[T any] // 任意类型。
[T int] // 只能是 int。
[T ~int] // 是 int 或底层类型是 int 的类型。
[T int | string] // 只能是 int 或 string。
[T io.Reader] // 任何实现 io.Reader 接口的类型。
```

- 竖线（ | ）表示类型集，匹配其中任一类型即可。
- 波浪线（ ~ ）表示底层类型（underlying type）是该类型的所有类型。

# 定义类型约束

在 Go 的类型限制是通过接口实现

```go
// 声明泛型类型限制
type Integer interface {
	Signed | Unsigned
}

type Signed interface {
	~int | ~int8 | ~int16 | ~int32 | ~int64
}

type Unsigned interface {
	~uint | ~uint8 | ~uint16 | ~uint32 | ~uint64
}

// 其他类型
type Category struct {
    ID int32
    Name string
    Slug string
}

type Post struct {
    ID int32
    Categories []Category
    Title string
    Text string
    Slug string
}

type cacheable interface {
    Category | Post
}
```

除了类型集合外，还可以有方法声明

```go
type cacheable interface {
    Category | Post
  	TestFunc(string) string
}
```

# 泛型类型

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

# 泛型函数

```go
func Add[T string | int | int64 | float64](a, b T) T {
	return a + b
}

// 使用
Add("stone","bird")
Add(1, 2)
Add(1.0,2.0)
```

# 泛型结构体

```go
type cache[T any] struct {
    data map[string]any
}

func (c *cache[T]) Set(key string, value T) {
    c.data[key] = value
}

func (c *cache[T]) Get(key string) (v T) {
    if v, ok := c.data[key]; ok {
        return v
    }

    return
}
```

## 无泛型方法

```go
type MyThing[T any] struct {
	value T
}

func (m MyThing[T]) Perform[R any](f func(T) R) R { // 会报错
	return f(m.value)
}

func main() {
	fmt.Println("Hello, playground")
}
```

> Perform报错：method must have no type parameters

可以使用结构体支持泛型成员来代替

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

# 类型转换

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

# 总结

- 泛型虽然简化了代码，在平时开发过程中尽量少使用泛型。
- 泛型可能引发 动态调用、无法内联、内存逃逸 等性能问题。
- 尽量在数据结构中使用泛型。
- 不要将接口传递给泛型函数。