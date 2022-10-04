<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [前言](#%E5%89%8D%E8%A8%80)
- [HelloWorld](#helloworld)
- [小刀弑牛（三天）](#%E5%B0%8F%E5%88%80%E5%BC%91%E7%89%9B%E4%B8%89%E5%A4%A9)
- [程序设计](#%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)
  - [package - 包 (一天)](#package---%E5%8C%85-%E4%B8%80%E5%A4%A9)
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

# 小刀弑牛（三天）

[官方入门地址](https://tour.go-zh.org/welcome/1)

> 入门重要途径，切勿心急，建议手敲一遍
>
> 如果你是没有其他语言基础的初学者，建议学习2-3遍。

# 程序设计

## package - 包 (一天)

- 合理使用包名、成员名、导出符号、init函数。
- 了解相关环境变量。
- 注意包的依赖关系，导入包不要成环。
- 使用模块管理。

[package笔记](https://github.com/stonebirdjx/go-ladder/blob/master/%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/package-%E5%8C%85.md)

# 后续建议

强者总是比较孤独,请坚持学习！送上一句黑鸡汤：

> "The last thing you want is to look back on your life and wonder... if only." --by caicloud

# 参考资料

[雨痕大佬的go学习笔记](https://github.com/qyuhen/book)

[饶全成](https://github.com/qcrao)