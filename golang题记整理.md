<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [1. 下面这段代码输出的内容](#1-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E7%9A%84%E5%86%85%E5%AE%B9)
- [2. 下面这段代码输出什么，说明原因。](#2-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E8%AF%B4%E6%98%8E%E5%8E%9F%E5%9B%A0)
- [3. 下面两段代码输出什么](#3-%E4%B8%8B%E9%9D%A2%E4%B8%A4%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [4. 下面这段代码有什么缺陷](#4-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E7%BC%BA%E9%99%B7)
- [5. new() 与 make() 的区别](#5-new-%E4%B8%8E-make-%E7%9A%84%E5%8C%BA%E5%88%AB)
- [6. 下面这段代码能否通过编译](#6-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E9%80%9A%E8%BF%87%E7%BC%96%E8%AF%91)
- [7. 下面这段代码能否通过编译](#7-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E9%80%9A%E8%BF%87%E7%BC%96%E8%AF%91)
- [8. 下面这段代码能否通过编译](#8-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E9%80%9A%E8%BF%87%E7%BC%96%E8%AF%91)
- [9. 下面这段代码能否通过编译](#9-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E9%80%9A%E8%BF%87%E7%BC%96%E8%AF%91)
- [10. 通过指针变量p访问其成员变量name,有哪几种方式](#10-%E9%80%9A%E8%BF%87%E6%8C%87%E9%92%88%E5%8F%98%E9%87%8Fp%E8%AE%BF%E9%97%AE%E5%85%B6%E6%88%90%E5%91%98%E5%8F%98%E9%87%8Fname%E6%9C%89%E5%93%AA%E5%87%A0%E7%A7%8D%E6%96%B9%E5%BC%8F)
- [11. 下面这段代码能否通过编译](#11-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E9%80%9A%E8%BF%87%E7%BC%96%E8%AF%91)
- [12. 以下代码输出什么](#12-%E4%BB%A5%E4%B8%8B%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [13.关于字符串连接，下面语法正确的是？](#13%E5%85%B3%E4%BA%8E%E5%AD%97%E7%AC%A6%E4%B8%B2%E8%BF%9E%E6%8E%A5%E4%B8%8B%E9%9D%A2%E8%AF%AD%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [14. 下面这段代码能否编译通过？如果可以，输出什么](#14-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E7%BC%96%E8%AF%91%E9%80%9A%E8%BF%87%E5%A6%82%E6%9E%9C%E5%8F%AF%E4%BB%A5%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [15. 下面赋值正确的是](#15-%E4%B8%8B%E9%9D%A2%E8%B5%8B%E5%80%BC%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [16. 关于init函数，下面说法正确的是](#16-%E5%85%B3%E4%BA%8Einit%E5%87%BD%E6%95%B0%E4%B8%8B%E9%9D%A2%E8%AF%B4%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [17. 下面这段代码输出什么以及原因](#17-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E4%BB%A5%E5%8F%8A%E5%8E%9F%E5%9B%A0)
- [18. 下面这段代码能否编译通过？如果可以，输出什么](#18-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E7%BC%96%E8%AF%91%E9%80%9A%E8%BF%87%E5%A6%82%E6%9E%9C%E5%8F%AF%E4%BB%A5%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [19. 关于channel，下面语法正确的是](#19-%E5%85%B3%E4%BA%8Echannel%E4%B8%8B%E9%9D%A2%E8%AF%AD%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [20. 下面这段代码输出什么](#20-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [21. 下面这段代码输出什么](#21-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [22. 下面这段代码输出什么](#22-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [23. 下面这段代码输出什么](#23-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [24. 下面这段代码输出什么](#24-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [25. 关于 cap() 函数的适用类型，下面说法正确的是](#25-%E5%85%B3%E4%BA%8E-cap-%E5%87%BD%E6%95%B0%E7%9A%84%E9%80%82%E7%94%A8%E7%B1%BB%E5%9E%8B%E4%B8%8B%E9%9D%A2%E8%AF%B4%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [26. 下面这段代码输出什么](#26-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [27. 下面这段代码输出什么](#27-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [28. 下面属于关键字的是](#28-%E4%B8%8B%E9%9D%A2%E5%B1%9E%E4%BA%8E%E5%85%B3%E9%94%AE%E5%AD%97%E7%9A%84%E6%98%AF)
- [29. 下面这段代码输出什么](#29-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [30. 下面这段代码输出什么](#30-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [31. 定义一个包内全局字符串变量，下面语法正确的是](#31-%E5%AE%9A%E4%B9%89%E4%B8%80%E4%B8%AA%E5%8C%85%E5%86%85%E5%85%A8%E5%B1%80%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%8F%98%E9%87%8F%E4%B8%8B%E9%9D%A2%E8%AF%AD%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [32. 下面这段代码输出什么](#32-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [33. 下面这段代码输出什么](#33-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [34. 下面代码输出什么](#34-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [35. 下面代码输出什么](#35-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [36. 对 add() 函数调用正确的是](#36-%E5%AF%B9-add-%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [37. 下面代码下划线处可以填入哪个选项以输出yes nil](#37-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E4%B8%8B%E5%88%92%E7%BA%BF%E5%A4%84%E5%8F%AF%E4%BB%A5%E5%A1%AB%E5%85%A5%E5%93%AA%E4%B8%AA%E9%80%89%E9%A1%B9%E4%BB%A5%E8%BE%93%E5%87%BAyes-nil)
- [38. 下面这段代码输出什么](#38-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [39. 下面这段代码输出什么](#39-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [40. 切片 a、b、c 的长度和容量分别是多少](#40-%E5%88%87%E7%89%87-abc-%E7%9A%84%E9%95%BF%E5%BA%A6%E5%92%8C%E5%AE%B9%E9%87%8F%E5%88%86%E5%88%AB%E6%98%AF%E5%A4%9A%E5%B0%91)
- [41. 下面代码中 A B 两处应该怎么修改才能顺利编译](#41-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E4%B8%AD-a-b-%E4%B8%A4%E5%A4%84%E5%BA%94%E8%AF%A5%E6%80%8E%E4%B9%88%E4%BF%AE%E6%94%B9%E6%89%8D%E8%83%BD%E9%A1%BA%E5%88%A9%E7%BC%96%E8%AF%91)
- [42. 下面代码输出什么](#42-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [43. 下面代码中，x 已声明，y 没有声明，判断每条语句的对错。](#43-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E4%B8%ADx-%E5%B7%B2%E5%A3%B0%E6%98%8Ey-%E6%B2%A1%E6%9C%89%E5%A3%B0%E6%98%8E%E5%88%A4%E6%96%AD%E6%AF%8F%E6%9D%A1%E8%AF%AD%E5%8F%A5%E7%9A%84%E5%AF%B9%E9%94%99)
- [44. 下面代码输出什么](#44-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [45. 下面代码输出什么](#45-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [46. f1()、f2()、f3() 函数分别返回什么](#46-f1f2f3-%E5%87%BD%E6%95%B0%E5%88%86%E5%88%AB%E8%BF%94%E5%9B%9E%E4%BB%80%E4%B9%88)
- [47. 下面代码段输出什么](#47-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%AE%B5%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [48. 下面这段代码正确的输出是什么](#48-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E6%AD%A3%E7%A1%AE%E7%9A%84%E8%BE%93%E5%87%BA%E6%98%AF%E4%BB%80%E4%B9%88)
- [49. 下面代码输出什么](#49-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [50. 下面的两个切片声明中有什么区别？哪个更可取](#50-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%B8%A4%E4%B8%AA%E5%88%87%E7%89%87%E5%A3%B0%E6%98%8E%E4%B8%AD%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%E5%93%AA%E4%B8%AA%E6%9B%B4%E5%8F%AF%E5%8F%96)
- [51. A、B、C、D 哪些选项有语法错误](#51-abcd-%E5%93%AA%E4%BA%9B%E9%80%89%E9%A1%B9%E6%9C%89%E8%AF%AD%E6%B3%95%E9%94%99%E8%AF%AF)
- [52. 下面 A、B 两处应该填入什么代码，才能确保顺利打印出结果](#52-%E4%B8%8B%E9%9D%A2-ab-%E4%B8%A4%E5%A4%84%E5%BA%94%E8%AF%A5%E5%A1%AB%E5%85%A5%E4%BB%80%E4%B9%88%E4%BB%A3%E7%A0%81%E6%89%8D%E8%83%BD%E7%A1%AE%E4%BF%9D%E9%A1%BA%E5%88%A9%E6%89%93%E5%8D%B0%E5%87%BA%E7%BB%93%E6%9E%9C)
- [53. 下面的代码有几处语法问题，各是什么](#53-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E5%87%A0%E5%A4%84%E8%AF%AD%E6%B3%95%E9%97%AE%E9%A2%98%E5%90%84%E6%98%AF%E4%BB%80%E4%B9%88)
- [54. return 之后的 defer 语句会执行吗，下面这段代码输出什么](#54-return-%E4%B9%8B%E5%90%8E%E7%9A%84-defer-%E8%AF%AD%E5%8F%A5%E4%BC%9A%E6%89%A7%E8%A1%8C%E5%90%97%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [55. 下面这段代码输出什么？为什么？](#55-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88)
- [56. 下面正确的结果是](#56-%E4%B8%8B%E9%9D%A2%E6%AD%A3%E7%A1%AE%E7%9A%84%E7%BB%93%E6%9E%9C%E6%98%AF)
- [57. 下面这段代码输出什么](#57-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [58. 下面这段代码输出什么](#58-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [59. 下面这段代码输出什么？为什么](#59-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88)
- [60. 下面这段代码输出什么？为什么](#60-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88)
- [61. 下面这段代码输出什么](#61-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [62. 下面这段代码输出什么？为什么](#62-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

学习整理，方便查询。题目摘抄自网络 [yqchilde](https://github.com/yqchilde)

# 1. 下面这段代码输出的内容

```go
package main

import "fmt"

func main() {
    defer_call()
}

func defer_call()  {
    defer func() {fmt.Println("打印前")}()
    defer func() {fmt.Println("打印中")}()
    defer func() {fmt.Println("打印后")}()

    panic("触发异常")
}
```

Output:

```bash
打印后
打印中
打印前
panic: 触发异常
```

<font color="red"> `defer` 的执行顺序是先进后出。出现panic语句的时候，会先按照 `defer` 的后进先出顺序执行，最后才会执行panic。</font>

# 2. 下面这段代码输出什么，说明原因。

```go
package main

import "fmt"

func main() {
	slice := []int{0, 1, 2, 3}
	m := make(map[int]*int)

	for key, val := range slice {
		m[key] = &val // val的地址
	}

	for k, v := range m {
		fmt.Println(k, "->", *v)
	}
}
```

Output:

```bash
# 注：key的顺序无法确定
0 -> 3
1 -> 3
2 -> 3
3 -> 3
```

`for range` 循环的时候会创建每个元素的副本，而不是每个元素的引用，所以 `m[key] = &val` 取的都是变量val的地址，所以最后 `map` 中的所有元素的值都是变量 `val` 的地址，因为最后 `val` 被赋值为3，所有输出的都是3。

<font color="red">注意取得是&地址</font>

# 3. 下面两段代码输出什么

```go
// 1.
func main() {
    s := make([]int, 5)
    s = append(s, 1, 2, 3)
    fmt.Println(s)
}

// 2.
func main() {
    s := make([]int, 0)
    s = append(s, 1, 2, 3, 4)
    fmt.Println(s)
}
```

Output:

```bash
# 1.
[0 0 0 0 0 1 2 3]

# 2.
[1 2 3 4]
```

使用 `append` 向 `slice` 中添加元素，第一题中slice容量为5，所以补5个0，第二题为0，所以不需要。

<font color="red">注意零值</font>

# 4. 下面这段代码有什么缺陷

```go
func funcMui(x, y int) (sum int, error) {
    return x + y, nil
}
```

在函数有多个返回值时，只要有一个返回值有命名，其他的也必须命名。如果有多个返回值必须加上括号();如果只有一个返回值且命名也需要加上括号()。这里的第一个返回值有命名sum，第二个没有命名，所以错误。

<font color="red">注意返回值要么都匿名，要么都具名，开发中尽量少用具名返回</font>

# 5. new() 与 make() 的区别

`new(T)` 和 `make(T, args)` 是Go语言内建函数，用来分配内存，但适用的类型不用。

```go
func new(Type) *Type
func make(t Type, size ...IntegerType) Type
```

- `new(T)` 会为了 `T` 类型的新值分配已置零的内存空间，并返回地址（指针），即类型为 `*T` 的值。换句话说就是，返回一个指针，该指针指向新分配的、类型为 `T` 的零值。适用于值类型，如 `数组` 、 `结构体` 等。
- `make(T, args)` 返回初始化之后的T类型的值，也不是指针 `*T` ，是经过初始化之后的T的引用。 `make()` 只适用于 `slice` 、 `map` 和 `channel` 。

<font color="red">new()适用于值类型，分配已置零的内存空间，返回指针。make()适用于引用类型，申请了空间大小，返回的不是指针</font>。

> 如果用new初始化引用类型，注意要重新申请一下内存

# 6. 下面这段代码能否通过编译

```go
func main() {
    list := new([]int) 
    list = append(list, 1)
    fmt.Println(list)
}
```

不能通过编译， `new([]int)` 之后的 `list` 是一个 `*int[]` 类型的指针，不能对指针执行 `append` 操作。可以使用 `make()` 初始化之后再用。同样的， `map` 和 `channel` 建议使用 `make()` 或字面量的方式初始化，不要用 `new` 。

用new需要改成以下

```go
func main() {
	list := new([]int)
	*list = make([]int, 0)
	*list = append(*list, 1)
	fmt.Println(*list)
}
```

<font color="red">append 第一个参数也不能是地址</font>

# 7. 下面这段代码能否通过编译

```go
func main() {
    s1 := []int{1, 2, 3}
    s2 := []int{4, 5}
    s1 = append(s1, s2)
    fmt.Println(s1)
}
```

不能通过编译，`append()` 的第二个参数不能直接使用 `slice` ，需使用 `...` 操作符，将一个切片追加到另一个切片上： `append(s1, s2...)` 。或者直接跟上元素，形如： `append(s1, 1, 2, 3)` 。

<font color="red">append 切片（相同类型，不能是数组），需要用到语法糖</font>

#  8. 下面这段代码能否通过编译

```go
var (
    size := 1024 // 函数外必须使用显示初始化
    max_size = size * 2 // 
)

func main() {
    fmt.Println(size, max_size)
}
```

不能通过编译，函数外必须使用显示初始化

<font color="red">`:=简短模式只能在函数内部使用`</font>

# 9. 下面这段代码能否通过编译

```go
package main

import "fmt"

func main() {
	sn1 := struct {
		age  int
		name string
	}{age: 11, name: "qq"}
	sn2 := struct {
		age  int
		name string
	}{age: 11, name: "11"}

	if sn1 == sn2 {
		fmt.Println("sn1 == sn2")
	}

	sm1 := struct {
		age int
		m   map[string]string
	}{age: 11, m: map[string]string{"a": "1"}}
	sm2 := struct {
		age int
		m   map[string]string
	}{age: 11, m: map[string]string{"a": "1"}}

	if sm1 == sm2 {
		fmt.Println("sm1 == sm2")
	}
}
```

不能通过编译，结构体比较如下

1. 结构体只能比较是否相等，但是不能比较大小；
2. 想同类型的结构体才能进行比较，结构体是否相同不但与属性类型有关，还与属性顺序相关；
3. 如果struct的所有成员都可以比较，则该struct就可以通过==或!=进行比较是否相同，比较时逐个项进行比较，如果每一项都相等，则两个结构体才相等，否则不相等；

<font color="red">值类型的能比较：bool、数值型、字符、指针、数组等<br>引用类型不能比较：slice、map、函数</font>

#  10. 通过指针变量p访问其成员变量name,有哪几种方式

```bash
A. p.name
B. (&p).name
C. (*p).name
D. p->name
```

A C   `&` 取址运算符， `*` 指针解引用

# 11. 下面这段代码能否通过编译

```go
package main

import "fmt"

type MyInt1 int
type MyInt2 = int

func main() {
	var i int = 0
	var i1 MyInt1 = i // var i1 MyInt1 = MyInt1(i)
	var i2 MyInt2 = i
	fmt.Println(i1, i2)
}
```

不能通过编译，Go是强类型语言.

# 12. 以下代码输出什么

```go
func main() {
    a := []int{7, 8, 9}
    fmt.Printf("%+v\n", a)
    ap(a)
    fmt.Printf("%+v\n", a)
    app(a)
    fmt.Printf("%+v\n", a)
}

func ap(a []int) {
    a = append(a, 10) // append 导致a的地址变了
}

func app(a []int) {
    a[0] = 1
}
```

Output：

```bash
[7 8 9]
[7 8 9]
[1 8 9]
```

因为append导致底层数组重新分配内存了，append中的a这个alice的底层数组和外面不是一个，并没有改变外面的。

#  13.关于字符串连接，下面语法正确的是？

```go
A. str := 'abc' + '123'
B. str := "abc" + "123"
C. str := '123' + "abc"
D. fmt.Sprintf("abc%d", 123)
```

B、D

# 14. 下面这段代码能否编译通过？如果可以，输出什么

```go
const (
    x = iota
    _
    y
    z = "zz"
    k
    p = iota
)

func main() {
    fmt.Println(x, y, z, k, p)
}
```

编译通过`0 2 zz zz 5`

iota初始值为0，所以x为0，_表示不赋值，但是iota是从上往下加1的，所以y是2，z是“zz”,k和上面一个同值也是“zz”,p是iota,从上0开始数他是5

<font color="red">iota 在const里面相当于数组的索引，如果中断了必须显示的恢复</font>

# 15. 下面赋值正确的是

```go
A. var x = nil
B. var x interface{} = nil
C. var x string = nil
D. var x error = nil
```

B、D

- A错在没有写类型
- C错在字符串的空值是 `""` 而不是nil。
- 知识点：nil只能赋值给指针、chan、func、interface、map、或slice、类型的变量。

<font color="red">nil只能赋值给指针、func、interface、引用类型 （chan、map、slice ）</font>

# 16. 关于init函数，下面说法正确的是

```bash
A. 一个包中，可以包含多个init函数；
B. 程序编译时，先执行依赖包的init函数，再执行main包内的init函数；
C. main包中，不能有init函数；
D. init函数可以被其他函数调用；
```

A、B

1. init()函数是用于程序执行前做包的初始化的函数，比如初始化包里的变量等；
1. 一个包可以出现多个init()函数，一个源文件也可以包含多个init()函数；
1. 同一个包中多个init()函数的执行顺序没有明确的定义，但是不同包的init函数是根据包导入的依赖关系决定的；
1. init函数在代码中不能被显示调用、不能被引用（赋值给函数变量），否则出现编译失败；
1. 一个包被引用多次，如A import B，C import B，A import C，B被引用多次，但B包只会初始化一次；
1. 引入包，不可出现死循环。即A import B，B import A，这种情况下编译失败；

<font color="red">init()一般用于初始化包内的变量。init()只会被初始化一次，导入包时会会执行包的init函数，只初始化可以占位符导入。</font>

# 17. 下面这段代码输出什么以及原因

```go
func hello() []string {
    return nil
}

func main() {
    h := hello
    if h == nil {
        fmt.Println("nil")
    } else {
        fmt.Println("not nil")
    }
}
```

Output:

```bash
not nil
```

这道题里面，是将 `hello()` 赋值给变量h，而不是函数的返回值，所以输出 `not nil`

# 18. 下面这段代码能否编译通过？如果可以，输出什么

```go
func GetValue() int {
    return 1
}

func main() {
    i := GetValue()
    switch i.(type) {
    case int:
        fmt.Println("int")
    case string:
        fmt.Println("string")
    case interface{}:
        fmt.Println("interface")
    default:
        fmt.Println("unknown")
    }
}
```

编译失败

只有接口类型才可以使用类型选择。

<font color="red"> i.(type)，其中i是接口，type是固定关键字</font>

# 19. 关于channel，下面语法正确的是

```go
A. var ch chan int
B. ch := make(chan int)
C. <-ch
D. ch<- // 写入chan必须带上值
```

A、B、C

A、B都是申明channel；C读取channel；写channel是必须带上值，所以D错误。

#  20. 下面这段代码输出什么

```go
type person struct {
    name string
}

func main() {
    var m map[person]int
    p := person{"make"}
    fmt.Println(m[p])
}
```

Output：

```go
0
```

打印一个map中不存在的值时，返回元素类型的零值。 

<font color="red"> 未申请空间的map也可以取值，只是不能添加元素</font>

# 21. 下面这段代码输出什么

```go
func hello(num ...int) {
    num[0] = 18
}

func main() {
    i := []int{5, 6, 7}
    hello(i...)
    fmt.Println(i[0])
}
```

Output:

```go
18
```

<font color="red"> 传的切片、底层数组共享</font>

# 22. 下面这段代码输出什么

```go
func main() {  
    a := 5 // 默认推导为int
    b := 8.1 // 默认推导为float
    fmt.Println(a + b)
}
```

compilation error  

`a` 的类型是`int` ，`b` 的类型是`float` ，两个不同类型的数值不能相加，编译报错。

# 23. 下面这段代码输出什么

```go
package main

import (  
    "fmt"
)

func main() {  
    a := [5]int{1, 2, 3, 4, 5}
    t := a[3:4:4]
    fmt.Println(t[0]) // 切片 t 为 [4]，长度和容量都是 1。
}
```

Output:

```bash
4
```

截取操作符还可以有第三个参数，形如 [i,j,k]，第三个参数 k 用来限制新切片的容量，但不能超过原数组（切片）的底层数组大小。截取获得的切片的<font color="red">长度和容量分别是：**j-i、k-i**</font>。

# 24. 下面这段代码输出什么

```go
func main() {
    a := [2]int{5, 6}
    b := [3]int{5, 6}
    if a == b {
        fmt.Println("equal")
    } else {
        fmt.Println("not equal")
    }
}
```

compilation error  

 `a` 和 `b` 是不同的类型数组，是不能比较的，所以编译错误。

# 25. 关于 cap() 函数的适用类型，下面说法正确的是

```bash
A. array
B. slice
C. map
D. channel
```

A、B、D

<font color="red">cap() 函数不适用 map</font>

# 26. 下面这段代码输出什么

```go
func main() {  
    var i interface{}
    if i == nil {
        fmt.Println("nil")
        return
    }
    fmt.Println("not nil")
}
```

Output:

```bash
nil
```

<font color="red">当且仅当接口的动态值和动态类型都为 nil 时，接口类型值才为 nil</font>

# 27. 下面这段代码输出什么

```go
func main() {  
    s := make(map[string]int)
    delete(s, "h")
    fmt.Println(s["h"])
}
```

Output:

```bash
0
```

<font color="red">删除 map 不存在的键值对时，不会报错，相当于没有任何作用；获取不存在的减值对时，返回值类型对应的零值，所以返回 0。</font>

# 28. 下面属于关键字的是

```go
A. func
B. struct
C. class
D. defer
```

A、B、D

# 29. 下面这段代码输出什么

```go
func main() {  
    i := -5
    j := +5
    fmt.Printf("%+d %+d", i, j)
}
```

Output:

```bash
 -5 +5
```

`%d`表示输出十进制数字，`+`表示输出数值的符号。这里不表示取反。

# 30. 下面这段代码输出什么

```go
type People struct{}

func (p *People) ShowA() {
    fmt.Println("showA")
    p.ShowB()
}
func (p *People) ShowB() {
    fmt.Println("showB")
}

type Teacher struct {
    People
}

func (t *Teacher) ShowB() {
    fmt.Println("teacher showB")
}

func main() {
    t := Teacher{}
    t.ShowB()
}
```

Output:

```bash
teacher showB
```

结构体嵌套，<font color="red">外部类型还可以定义自己的属性和方法，甚至可以定义与内部相同的方法，这样内部类型的方法就会被“屏蔽”。</font>

# 31. 定义一个包内全局字符串变量，下面语法正确的是

```go
A. var str string
B. str := ""  // 函数内
C. str = ""   // 未使用关键字
D. var str = ""
```

A、D

# 32. 下面这段代码输出什么

```go
func hello(i int) {  
    fmt.Println(i)
}
func main() {  
    i := 5
    defer hello(i)
    i = i + 10
}
```

Output:

```bash
5
```

hello() 函数的参数在执行 defer 语句的时候会保存一份副本，在实际调用 hello() 函数时用，所以是 5.

# 33. 下面这段代码输出什么

```go
type People struct{}

func (p *People) ShowA() {
    fmt.Println("showA")
    p.ShowB()
}
func (p *People) ShowB() {
    fmt.Println("showB")
}

type Teacher struct {
    People
}

func (t *Teacher) ShowB() {
    fmt.Println("teacher showB")
}

func main() {
    t := Teacher{}
    t.ShowA()
}
```

Output:

```bash
showA
showB
```

匿名结构体嵌套，Teacher 没有自己 ShowA()，所以调用内部类型 People 的同名方法，

# 34. 下面代码输出什么

```go
func main() {
    str := "hello"
    str[0] = 'x' // 字符串在只读段上，元素不可以寻址
    fmt.Println(str)
}
```

compilation error

Go 语言中的字符串是只读的。属于常量

# 35. 下面代码输出什么

```go
func incr(p *int) int {
    *p++
    return *p
}

func main() {
    p :=1
    incr(&p)
    fmt.Println(p)
}
```

Output：

```go
2
```

指针,，incr() 返回自增后的结果

# 36. 对 add() 函数调用正确的是

```go
func add(args ...int) int {

    sum := 0
    for _, arg := range args {
        sum += arg
    }
    return sum
}

A. add(1, 2)
B. add(1, 3, 7)
C. add([]int{1, 2})
D. add([]int{1, 3, 7}…)
```

可变参数传入切片，需要用`...`语法糖解开

# 37. 下面代码下划线处可以填入哪个选项以输出yes nil

```go
func main() {
    var s1 []int // nil
    var s2 = []int{} // not nil 
    if __ == nil {
        fmt.Println("yes nil")
    }else{
        fmt.Println("no nil")
    }
}

A. s1
B. s2
C. s1、s2 都可以
```

A

# 38. 下面这段代码输出什么

```go
func main() {  
    i := 65 // 推荐使用 `var i byte = 65` 或 `var i uint8 = 65` 替代
    fmt.Println(string(i))
}
```

Output：

```bash
A  # ascill 
```

# 39. 下面这段代码输出什么

```go
type A interface {
    ShowA() int
}

type B interface {
    ShowB() int
}

type Work struct {
    i int
}

func (w Work) ShowA() int {
    return w.i + 10
}

func (w Work) ShowB() int {
    return w.i + 20
}

func main() {
    c := Work{3}
    var a A = c
    var b B = c
    fmt.Println(a.ShowA())
    fmt.Println(b.ShowB())
}
```

Output：

```bash
13 23
```

所以接口变量 a、b 调用各自的方法 ShowA() 和 ShowB()，输出 13、23。

# 40. 切片 a、b、c 的长度和容量分别是多少

```go
func main() {
    s := [3]int{1, 2, 3}
    a := s[:0]
    b := s[:2]
    c := s[1:2:cap(s)]
}
```

Output：

```bash
a: len=0 cap=3
b: len=2 cap=3
c: len=1 cap=2  # j-i,k-i
```

# 41. 下面代码中 A B 两处应该怎么修改才能顺利编译

```go
func main() {
    var m map[string]int        // 初始化map需要申请空间
    m["a"] = 1
    if v := m["b"]; v != nil {  // 取值的时候需要判断key存不存在
        fmt.Println(v)
    }
}
```

修改如下

```go
func main() {
    var m = make(map[string]int)       
    m["a"] = 1
    if v,ok := m["b"]; ok { 
        fmt.Println(v)
    }
}
```

# 42. 下面代码输出什么

```go
type A interface {
    ShowA() int
}

type B interface {
    ShowB() int
}

type Work struct {
    i int
}

func (w Work) ShowA() int {
    return w.i + 10
}

func (w Work) ShowB() int {
    return w.i + 20
}

func main() {
    c := Work{3}
    var a A = c
    var b B = c
    fmt.Println(a.ShowB())  // a 没有showb
    fmt.Println(b.ShowA()) // b 没有showA
}
```

compilation error

<font color="red">注意方法是否实现</font>

# 43. 下面代码中，x 已声明，y 没有声明，判断每条语句的对错。

```go
x, _ := f() // 错， x 已声明 应该改成 x, _ = f()
x, _ = f()
x, y := f()
x, y = f() // 错 y 没有声明 x, y := f()
```

# 44. 下面代码输出什么

```go
func increaseA() int {
    var i int
    defer func() {
        i++
    }()
    return i
}

func increaseB() (r int) {
    defer func() {
        r++
    }()
    return r
}

func main() {
    fmt.Println(increaseA())
    fmt.Println(increaseB())
}
```

Output：

```bash
0
1
```

increaseA() 的返回参数是匿名，increaseB() 是具名。

# 45. 下面代码输出什么

```go
type A interface {
    ShowA() int
}

type B interface {
    ShowB() int
}

type Work struct {
    i int
}

func (w Work) ShowA() int {
    return w.i + 10
}

func (w Work) ShowB() int {
    return w.i + 20
}

func main() {
    var a A = Work{3}
    s := a.(Work)
    fmt.Println(s.ShowA())
    fmt.Println(s.ShowB())
}
```

Output：

```bash
13
23
```

i.(type) 类型断言

# 46. f1()、f2()、f3() 函数分别返回什么

```go
func f1() (r int) {
    defer func() {
        r++ // r是最开始的地址
    }()
    return 0
}

func f2() (r int) {
    t := 5
    defer func() {
        t = t + 5 
    }()
    return t // t=r覆盖了r的地址。
}

func f3() (r int) {
    defer func(r int) {
        r = r + 5  // r当参数传递 与外面的r不是同一个r
    }(r)
    return 1
}
```

Output：

```bash
1 5 1
```

`defer`函数的执行顺序。

# 47. 下面代码段输出什么

```go
type Person struct {
    age int
}

func main() {
    person := &Person{28}

    defer fmt.Println(person.age) // 28


    defer func(p *Person) {// 缓存的是结构体Person{28} 的地址
        fmt.Println(p.age) // 29 
    }(person)  

    defer func() {
        fmt.Println(person.age) // 闭包引用 29
    }()

    person.age = 29
}
```

Output:

```bash
 29 29 28
```

1.person.age 此时是将 28 当做 defer 函数的参数，会把 28 缓存在栈中，等到最后执行该 defer 语句的时候取出，即输出 28；

2.defer 缓存的是结构体 Person{28} 的地址，最终 Person{28} 的 age 被重新赋值为 29，所以 defer 语句最后执行的时候，依靠缓存的地址取出的 age 便是 29，即输出 29；

3.闭包引用，输出 29

# 48. 下面这段代码正确的输出是什么

```go
func f() {
    defer fmt.Println("D")
    fmt.Println("F")
}

func main() {
    f()
    fmt.Println("M")
}
```

Output:

```bash
F D M
```

# 49. 下面代码输出什么

```go
type Person struct {
    age int
}

func main() {
    person := &Person{28}
    defer fmt.Println(person.age) //28 

    defer func(p *Person) { //缓存的是结构体Person{28} 的地址
        fmt.Println(p.age)
    }(person) // 之前的地址 28

    defer func() {
        fmt.Println(person.age) // 29 
    }()

    person = &Person{29} // 指针变了
}
```

Output:

```go
29 28 28
```

处.person.age 这一行代码跟之前含义是一样的，此时是将 28 当做 defer 函数的参数，会把 28 缓存在栈中，等到最后执行该 defer 语句的时候取出，即输出 28；

2处.defer 缓存的是结构体 Person{28} 的地址，这个地址指向的结构体没有被改变，最后 defer 语句后面的函数执行的时候取出仍是 28；

3处.闭包引用，person 的值已经被改变，指向结构体 `Person{29}`，所以输出 29.

由于 defer 的执行顺序为先进后出，即 3 2 1，所以输出 29 28 28。

# 50. 下面的两个切片声明中有什么区别？哪个更可取

```go
var a []int // 推荐优先
a := []int{}
```

A 声明的是 nil 切片；B 声明的是长度和容量都为 0 的空切片。第一种切片声明不会分配内存，优先选择。

# 51. A、B、C、D 哪些选项有语法错误

```go
type S struct {
}

func f(x interface{}) {
}

func g(x *interface{}) { // 指针类型也使用interface{}，
}

func main() {
    s := S{}
    p := &s
    f(s) //A
    g(s) //B
    f(p) //C
    g(p) //D
}
```

B、D

函数参数为 interface{} 时可以接收任何类型的参数，包括用户自定义类型等，即使是接收指针类型也用 interface{}，而不是使用 *interface{}。尽量不要使用 *interface{}

```go
func main() {
	var i interface{} = 10
	testInterface(&i)
}

func testInterface(i *interface{}) {
	fmt.Println(*i)
}
```

# 52. 下面 A、B 两处应该填入什么代码，才能确保顺利打印出结果

```go
type S struct {
    m string
}

func f() *S {
    return __  //A
}

func main() {
    p := __    //B
    fmt.Println(p.m) //print "foo"
}
```

anser

```go
&S{"foo"} 
*f() 或者 f()
```

A处，f() 函数返回参数是指针类型，所以可以用 & 取结构体的指针；

B 处，如果填`*f()`，则 p 是 S 类型；如果填 `f()`，则 p 是 *S 类型，不过都可以使用 `p.m`取得结构体的成员。

# 53. 下面的代码有几处语法问题，各是什么

```go
package main

import (
    "fmt"
)

func main() {
    var x string = nil //  var x string = "", string零值为""
    if x == nil {  // if x == "" {
        x = "default"
    }
    fmt.Println(x)
}
```

# 54. return 之后的 defer 语句会执行吗，下面这段代码输出什么

```go
var a bool = true
func main() {
    defer func(){
        fmt.Println("1")
    }()
  
    if a == true {
        fmt.Println("2")
        return
    }
  
    defer func(){
        fmt.Println("3")
    }()
}
```

Output:

```bash
2 1
```

defer 关键字后面的函数或者方法想要执行必须先注册，return 之后的 defer 是不能注册的， 也就不能执行后面的函数或方法。

<font color="red">return开始  -> defer操作（注意具名返回） -> 最终retrun </font>

# 55. 下面这段代码输出什么？为什么？

```go
func main() {
    s1 := []int{1, 2, 3}
    s2 := s1[1:] // 与s1 共享底层[2,3]数据
    s2[1] = 4
    fmt.Println(s1)
    s2 = append(s2, 5, 6, 7) // s2 地址已经更改
    fmt.Println(s1)
}
```

Output:

```go
[1 2 4]
[1 2 4]
```

# 56. 下面正确的结果是

```go
func main() {
    if a := 1; false {
    } else if b := 2; false {
    } else {
        println(a, b)
    }
}
```

Output:

```bash
1 2
```

代码块和变量作用域。

# 57. 下面这段代码输出什么

```go
func main() {
    m := map[int]string{0:"zero",1:"one"}
    for k,v := range m {
        fmt.Println(k,v)
    }
}
```

Output:

```bash
# map 的输出是无序的。
0 zero
1 one
```

# 58. 下面这段代码输出什么

```go
func main() {
    a := 1
    b := 2
    defer calc("1", a, calc("10", a, b))
    a = 0
    defer calc("2", a, calc("20", a, b))
    b = 1
}

func calc(index string, a, b int) int {
    ret := a + b
    fmt.Println(index, a, b, ret)
    return ret
}
```

Output:

```bash
10 1 2 3
20 0 2 2
2 0 2 2
1 1 3 4
```

<font color="red">`defer f(f1(f2()))`  只有最外层f()会被延迟执行，但是f1(f2())会立即执行</font>

# 59. 下面这段代码输出什么？为什么

```go
func (i int) PrintInt ()  {
    fmt.Println(i)
}

func main() {
    var i int = 1
    i.PrintInt()
}
```

 compilation error

基于类型创建的方法必须定义在同一个包内，用内置类型int 类型创建了 PrintInt() 方法，由于 int 类型和方法 PrintInt() 定义在不同的包内，所以编译出错。解决的办法可以定义一种新的类型

```go
type Myint int

func (i Myint) PrintInt ()  {
    fmt.Println(i)
}

func main() {
    var i Myint = 1
    i.PrintInt()
}
```

# 60. 下面这段代码输出什么？为什么

```go
type People interface {
    Speak(string) string
}

type Student struct{}

func (stu *Student) Speak(think string) (talk string) { // 指针类型的方法
    if think == "speak" {
        talk = "speak"
    } else {
        talk = "hi"
    }
    return
}

func main() {
    var peo People = Student{}
    think := "speak"
    fmt.Println(peo.Speak(think))
}
```

compilation error

值类型 `Student` 没有实现接口的 `Speak()` 方法，而是指针类型 `*Student` 实现该方法。

<font color="red">指针类型的结构体能用值类型的方法，值类型不能使用指针类型的方法</font>>

# 61. 下面这段代码输出什么

```go
const (
    a = iota
    b = iota
)
const (
    name = "name"
    c    = iota
    d    = iota
)

func main() {
    fmt.Println(a)
    fmt.Println(b)
    fmt.Println(c)
    fmt.Println(d)
}
```

Output:

```bash
0 1 1 2
```

iota 是 golang 语言的常量计数器，只能在常量的表达式中使用。

iota 在 const 关键字出现时将被重置为0，const中每新增一行常量声明将使 iota 计数一次。

<font color="red">iota 在每个conts组中相当于数组的索引</font>

# 62. 下面这段代码输出什么？为什么

```go
type People interface {
    Show()
}

type Student struct{}

func (stu *Student) Show() {
}

func main() {

    var s *Student
    if s == nil {
        fmt.Println("s is nil")
    } else {
        fmt.Println("s is not nil")
    }
    var p People = s
    if p == nil {
        fmt.Println("p is nil")
    } else {
        fmt.Println("p is not nil")
    }
}
```

Output:

```bash
s is nil
p is not nil
```

我们分配给变量 p 的值明明是 nil，然而 p 却不是 nil。记住一点，**当且仅当动态值和动态类型都为 nil 时，接口类型值才为 nil**。<font color="red">给变量 p 赋值之后，p 的动态值是 nil，但是动态类型却是 *Student，是一个 nil 指针</font>，所以相等条件不成立。
