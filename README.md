<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [go-ladder](#go-ladder)
- [断行规则](#%E6%96%AD%E8%A1%8C%E8%A7%84%E5%88%99)
- [从数组或者切片派生切片（取子切片）](#%E4%BB%8E%E6%95%B0%E7%BB%84%E6%88%96%E8%80%85%E5%88%87%E7%89%87%E6%B4%BE%E7%94%9F%E5%88%87%E7%89%87%E5%8F%96%E5%AD%90%E5%88%87%E7%89%87)
- [查看内存逃逸](#%E6%9F%A5%E7%9C%8B%E5%86%85%E5%AD%98%E9%80%83%E9%80%B8)
- [获取汇编代码](#%E8%8E%B7%E5%8F%96%E6%B1%87%E7%BC%96%E4%BB%A3%E7%A0%81)
- [喜欢的一些大牛](#%E5%96%9C%E6%AC%A2%E7%9A%84%E4%B8%80%E4%BA%9B%E5%A4%A7%E7%89%9B)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# go-ladder 

好好学习，天天向上。每个人都golang大师

强者总是比较孤独,请坚持学习！送上一句黑鸡汤：

> "The last thing you want is to look back on your life and wonder... if only." --by caicloud

# 断行规则

Go 语言的断行规则定义如下：

1. 在 Go 代码中，注释除外，如果一个代码行的最后一个语法词段（ token ）为下列所示之一，则一个分号将自动插入在此字段后（即行尾）：

- 一个标识符；
- 一个整数、浮点数、虚部、码点或者字符串字面表示形式；
- 这几个跳转关键字之一：`break`、`continue`、`fallthrough`和`return`；
- 自增运算符`++`或者自减运算符`--`；
- 一个右括号：`)`、`]`或`}`。

# 从数组或者切片派生切片（取子切片）

派生出来的切片的元素和基础切片（或者数组）的元素位于同一个内存片段上。或者说，派生出来的切片和基础切片（或者数组）将共享一些元素。

```go
baseContainer[low : high]       // 双下标形式
baseContainer[low : high : max] // 三下标形式 三下标形式是从Go 1.2开始支持的
```

取子切片表达式的语法形式中的下标必须满足下列关系，否则代码要么编译不通过，要么在运行时刻将造成恐慌。

```go
// 双下标形式
0 <= low <= high <= cap(baseContainer)
// 三下标形式
0 <= low <= high <= max <= cap(baseContainer)
```

不满足上述关系的取子切片表达式要么编译不通过，要么在运行时刻将导致一个恐慌。

注意：

- 只要上述关系均满足，下标`low`和`high`都可以大于`len(baseContainer)`。但是它们一定不能大于`cap(baseContainer)`。
- 如果`baseContainer`是一个零值nil切片，只要上面所示的子切片表达式中下标的值均为`0`，则这两个子切片表达式不会造成恐慌。 在这种情况下，结果切片也是一个nil切片。

在实践中，我们常常在子切片表达式中省略若干下标，以使代码看上去更加简洁。省略规则如下：

- 如果下标`low`为零，则它可被省略。此条规则同时适用于双下标形式和三下标形式。
- 如果下标`high`等于`len(baseContainer)`，则它可被省略。此条规则同时只适用于双下标形式。
- 三下标形式中的下标`max`在任何情况下都不可被省略。

比如，下面的子切片表达式都是相互等价的：

```go
baseContainer[0 : len(baseContainer)]
baseContainer[: len(baseContainer)]
baseContainer[0 :]
baseContainer[:]
baseContainer[0 : len(baseContainer) : cap(baseContainer)]
baseContainer[: len(baseContainer) : cap(baseContainer)]
```

> 子切片表达式的结果切片的长度为`high - low`、容量为`max - low`。 派生出来的结果切片的长度可能大于基础切片的长度，但结果切片的容量绝不可能大于基础切片的容量。

# 查看内存逃逸

```go
go build -gcflags='-m -m' main.go
```

# 获取汇编代码

```bash
go tool compile -S main.go
```

# 喜欢的一些大牛

[雨痕大佬的go学习笔记](https://github.com/qyuhen/book)

[饶全成](https://github.com/qcrao)

