<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [go-ladder:每个人都是golang大师](#go-ladder%E6%AF%8F%E4%B8%AA%E4%BA%BA%E9%83%BD%E6%98%AFgolang%E5%A4%A7%E5%B8%88)
- [小试牛刀](#%E5%B0%8F%E8%AF%95%E7%89%9B%E5%88%80)
- [:point_right:helloworld](#point_righthelloworld)
- [程序结构](#%E7%A8%8B%E5%BA%8F%E7%BB%93%E6%9E%84)
  - [:point_right:package - 包](#point_rightpackage---%E5%8C%85)
- [安全编码规范](#%E5%AE%89%E5%85%A8%E7%BC%96%E7%A0%81%E8%A7%84%E8%8C%83)
- [算法达人](#%E7%AE%97%E6%B3%95%E8%BE%BE%E4%BA%BA)
- [原理透析](#%E5%8E%9F%E7%90%86%E9%80%8F%E6%9E%90)
- [面试题记](#%E9%9D%A2%E8%AF%95%E9%A2%98%E8%AE%B0)
- [徐徐前行](#%E5%BE%90%E5%BE%90%E5%89%8D%E8%A1%8C)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# go-ladder:每个人都是golang大师

整理知识，传播智慧，好好学习，天天向上。

TODO：Simple, Poetic, Pithy

gopher：石鸟路遇  stonebirdjx <1245863260@qq.com,g1245863260@gmail.com>

> 文档为个人学习或工作积累，仅供学习和参考，不涉及商用，如有不正确的地方，请邮件指正。

# 小试牛刀

[安装golang](https://go.dev/doc/install)

[官方入门地址](https://tour.go-zh.org/welcome/1)

> 如果你是没有其他语言基础的初学者，建议学习2-3遍。

要求了解go语言的语法结构、数据类型、变量、常量、运算符、条件分支、循环语句、函数、作用域、结构体、指针、数组、切片、map、接口、并发等。达到熟练使用的地步。

# :point_right:helloworld

> 程序从 `main` 包开始运行。

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, 世界")
}
```

完成并运行上面代码，你现在已经是一个golang程序员 ( `gopher` ) 了.

> 函数体外每个语句应该以关键字开始，如：`var`、`func`、`type`、`const`、`import`、`package`

# 程序结构

## :point_right:package - 包

package定义包

- 包名原则上只由小写字面和数字组成。不包含大写字母和下划线等字符（测试包除外）
- 一般包名和目录保持一致

```go
package zip
package sha256
package sha256_test

// 目录名：port-obj
// 对应的包名：portobj
```

包内以大写字母开头(函数、变量、常量、结构体...)，那么它就是已导出(外部可调用)。

在导入一个包时，你只能引用其中已导出的名字。任何“未导出”的名字在该包外均无法访问。



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

强者总是比较孤独,请坚持学习！送上一句黑鸡汤：

> "The last thing you want is to look back on your life and wonder... if only." --by caicloud

