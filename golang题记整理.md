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
- [63. 下面这段代码输出什么](#63-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [64. 下面代码输出什么](#64-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [65. 下面的代码有什么问题](#65-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [66. 下面这段代码输出什么？如果编译错误的话，为什么](#66-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E5%A6%82%E6%9E%9C%E7%BC%96%E8%AF%91%E9%94%99%E8%AF%AF%E7%9A%84%E8%AF%9D%E4%B8%BA%E4%BB%80%E4%B9%88)
- [67. 下面这段代码能否正常结束](#67-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E6%AD%A3%E5%B8%B8%E7%BB%93%E6%9D%9F)
- [68. 下面这段代码输出什么？为什么](#68-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88)
- [69. 下面这段代码输出什么](#69-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [70. 下面这段代码输出什么](#70-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [71. 下面这段代码输出什么](#71-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [72. 下面这段代码输出什么](#72-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [73. 下面这段代码输出结果正确吗](#73-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E7%BB%93%E6%9E%9C%E6%AD%A3%E7%A1%AE%E5%90%97)
- [74. 下面代码里的 counter 的输出值](#74-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E9%87%8C%E7%9A%84-counter-%E7%9A%84%E8%BE%93%E5%87%BA%E5%80%BC)
- [75. 关于协程，下面说法正确是](#75-%E5%85%B3%E4%BA%8E%E5%8D%8F%E7%A8%8B%E4%B8%8B%E9%9D%A2%E8%AF%B4%E6%B3%95%E6%AD%A3%E7%A1%AE%E6%98%AF)
- [76. 关于循环语句，下面说法正确的有](#76-%E5%85%B3%E4%BA%8E%E5%BE%AA%E7%8E%AF%E8%AF%AD%E5%8F%A5%E4%B8%8B%E9%9D%A2%E8%AF%B4%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%9C%89)
- [77. 下面代码输出正确的是](#77-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [78. 关于类型转化，下面选项正确的是](#78-%E5%85%B3%E4%BA%8E%E7%B1%BB%E5%9E%8B%E8%BD%AC%E5%8C%96%E4%B8%8B%E9%9D%A2%E9%80%89%E9%A1%B9%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [79. 关于switch语句，下面说法正确的有](#79-%E5%85%B3%E4%BA%8Eswitch%E8%AF%AD%E5%8F%A5%E4%B8%8B%E9%9D%A2%E8%AF%B4%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%9C%89)
- [80. 如果 Add() 函数的调用代码为](#80-%E5%A6%82%E6%9E%9C-add-%E5%87%BD%E6%95%B0%E7%9A%84%E8%B0%83%E7%94%A8%E4%BB%A3%E7%A0%81%E4%B8%BA)
- [81. 关于 bool 变量 b 的赋值，下面错误的用法是](#81-%E5%85%B3%E4%BA%8E-bool-%E5%8F%98%E9%87%8F-b-%E7%9A%84%E8%B5%8B%E5%80%BC%E4%B8%8B%E9%9D%A2%E9%94%99%E8%AF%AF%E7%9A%84%E7%94%A8%E6%B3%95%E6%98%AF)
- [82. 关于变量的自增和自减操作，下面语句正确的是](#82-%E5%85%B3%E4%BA%8E%E5%8F%98%E9%87%8F%E7%9A%84%E8%87%AA%E5%A2%9E%E5%92%8C%E8%87%AA%E5%87%8F%E6%93%8D%E4%BD%9C%E4%B8%8B%E9%9D%A2%E8%AF%AD%E5%8F%A5%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [83. 关于GetPodAction定义,下面赋值正确的是](#83-%E5%85%B3%E4%BA%8Egetpodaction%E5%AE%9A%E4%B9%89%E4%B8%8B%E9%9D%A2%E8%B5%8B%E5%80%BC%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [84. 关于函数声明，下面语法正确的是](#84-%E5%85%B3%E4%BA%8E%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E%E4%B8%8B%E9%9D%A2%E8%AF%AD%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [85. 关于整型切片的初始化，下面正确的是](#85-%E5%85%B3%E4%BA%8E%E6%95%B4%E5%9E%8B%E5%88%87%E7%89%87%E7%9A%84%E5%88%9D%E5%A7%8B%E5%8C%96%E4%B8%8B%E9%9D%A2%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [86. 下面代码会触发异常吗？请说明。](#86-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E4%BC%9A%E8%A7%A6%E5%8F%91%E5%BC%82%E5%B8%B8%E5%90%97%E8%AF%B7%E8%AF%B4%E6%98%8E)
- [87. 关于channel的特性，下面说法正确的是](#87-%E5%85%B3%E4%BA%8Echannel%E7%9A%84%E7%89%B9%E6%80%A7%E4%B8%8B%E9%9D%A2%E8%AF%B4%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [88. 下面代码有什么问题？](#88-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [89. 下面代码能否编译通过？如果通过，输出什么](#89-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E7%BC%96%E8%AF%91%E9%80%9A%E8%BF%87%E5%A6%82%E6%9E%9C%E9%80%9A%E8%BF%87%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [90. 关于异常的触发，下面说法正确的是](#90-%E5%85%B3%E4%BA%8E%E5%BC%82%E5%B8%B8%E7%9A%84%E8%A7%A6%E5%8F%91%E4%B8%8B%E9%9D%A2%E8%AF%B4%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [91. 下面代码输出什么](#91-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [92. 下面这段代码能否编译通过？如果通过，输出什么](#92-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E7%BC%96%E8%AF%91%E9%80%9A%E8%BF%87%E5%A6%82%E6%9E%9C%E9%80%9A%E8%BF%87%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [93. 关于无缓冲和有冲突的channel，下面说法正确的是](#93-%E5%85%B3%E4%BA%8E%E6%97%A0%E7%BC%93%E5%86%B2%E5%92%8C%E6%9C%89%E5%86%B2%E7%AA%81%E7%9A%84channel%E4%B8%8B%E9%9D%A2%E8%AF%B4%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [94. 下面代码是否能编译通过？如果通过，输出什么](#94-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%98%AF%E5%90%A6%E8%83%BD%E7%BC%96%E8%AF%91%E9%80%9A%E8%BF%87%E5%A6%82%E6%9E%9C%E9%80%9A%E8%BF%87%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [95. 下面代码输出什么](#95-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [96. 关于select机制，下面说法正确的是](#96-%E5%85%B3%E4%BA%8Eselect%E6%9C%BA%E5%88%B6%E4%B8%8B%E9%9D%A2%E8%AF%B4%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [97. 下面的代码有什么问题](#97-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [98. 下面这段代码存在什么问题](#98-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E5%AD%98%E5%9C%A8%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [99. 下面代码编译能通过吗](#99-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E7%BC%96%E8%AF%91%E8%83%BD%E9%80%9A%E8%BF%87%E5%90%97)
- [100. 下面这段代码输出什么](#100-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [101. 下面这段代码输出什么](#101-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [102. 请指出下面代码的错误](#102-%E8%AF%B7%E6%8C%87%E5%87%BA%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E7%9A%84%E9%94%99%E8%AF%AF)
- [103. 下面代码输出什么](#103-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [104. 下面代码输出什么](#104-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [105. 下面的代码有什么问题](#105-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [106. 下面代码输出什么](#106-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [107. 下面代码有几处错误的地方？请说明原因](#107-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%9C%89%E5%87%A0%E5%A4%84%E9%94%99%E8%AF%AF%E7%9A%84%E5%9C%B0%E6%96%B9%E8%AF%B7%E8%AF%B4%E6%98%8E%E5%8E%9F%E5%9B%A0)
- [108. 下面代码有什么问题](#108-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [109. 下面的代码有什么问题](#109-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [110. 下面代码能编译通过吗](#110-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%83%BD%E7%BC%96%E8%AF%91%E9%80%9A%E8%BF%87%E5%90%97)
- [111. 下面代码有什么错误](#111-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%94%99%E8%AF%AF)
- [112. 下面代码有什么问题](#112-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [113. 下面代码输出什么](#113-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [114. 下面的代码有什么问题](#114-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [115. 下面代码输出什么](#115-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [116. 下面代码有什么问题](#116-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [117. 下面的代码有什么问题](#117-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [118. 下面代码最后一行输出什么](#118-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%9C%80%E5%90%8E%E4%B8%80%E8%A1%8C%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [119. 下面代码有什么问题](#119-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [120. 下面的代码输出什么](#120-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [121. 下面代码输出什么](#121-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [122. 下面这段代码输出什么](#122-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [123. 下面这段代码输出什么](#123-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [124. 下面代码输出什么](#124-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [125. 下面的代码能否正确输出](#125-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%83%BD%E5%90%A6%E6%AD%A3%E7%A1%AE%E8%BE%93%E5%87%BA)
- [126. 下面代码输出什么](#126-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [127. 下面的代码有什么问题](#127-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [128. 下面代码有什么不规范的地方吗](#128-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E4%B8%8D%E8%A7%84%E8%8C%83%E7%9A%84%E5%9C%B0%E6%96%B9%E5%90%97)
- [129. 关于 channel 下面描述正确的是](#129-%E5%85%B3%E4%BA%8E-channel-%E4%B8%8B%E9%9D%A2%E6%8F%8F%E8%BF%B0%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [130. 下面的代码有几处问题？请详细说明](#130-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E5%87%A0%E5%A4%84%E9%97%AE%E9%A2%98%E8%AF%B7%E8%AF%A6%E7%BB%86%E8%AF%B4%E6%98%8E)
- [131. 下面的代码有什么问题](#131-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [132. 下面的代码输出什么](#132-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [133. 关于 channel 下面描述正确的是](#133-%E5%85%B3%E4%BA%8E-channel-%E4%B8%8B%E9%9D%A2%E6%8F%8F%E8%BF%B0%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [134. 下面的代码有什么问题](#134-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [135. 下面的代码有什么问题](#135-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [136. 下面代码输出什么](#136-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [137. 下面哪一行代码会 panic，请说明原因](#137-%E4%B8%8B%E9%9D%A2%E5%93%AA%E4%B8%80%E8%A1%8C%E4%BB%A3%E7%A0%81%E4%BC%9A-panic%E8%AF%B7%E8%AF%B4%E6%98%8E%E5%8E%9F%E5%9B%A0)
- [138. 下面的代码输出什么](#138-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [139. 下面的代码输出什么](#139-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [140. 下面哪一行代码会 panic，请说明原因](#140-%E4%B8%8B%E9%9D%A2%E5%93%AA%E4%B8%80%E8%A1%8C%E4%BB%A3%E7%A0%81%E4%BC%9A-panic%E8%AF%B7%E8%AF%B4%E6%98%8E%E5%8E%9F%E5%9B%A0)
- [141. 下面的代码输出什么](#141-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [142. 下面哪一行代码会 panic，请说明原因](#142-%E4%B8%8B%E9%9D%A2%E5%93%AA%E4%B8%80%E8%A1%8C%E4%BB%A3%E7%A0%81%E4%BC%9A-panic%E8%AF%B7%E8%AF%B4%E6%98%8E%E5%8E%9F%E5%9B%A0)
- [143. 下面哪一行代码会 panic，请说明原因](#143-%E4%B8%8B%E9%9D%A2%E5%93%AA%E4%B8%80%E8%A1%8C%E4%BB%A3%E7%A0%81%E4%BC%9A-panic%E8%AF%B7%E8%AF%B4%E6%98%8E%E5%8E%9F%E5%9B%A0)
- [144. 下面的代码有什么问题](#144-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [145. 下面这段代码输出什么](#145-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [146. 下面代码输出什么](#146-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [147. 下面哪一行代码会 panic，请说明](#147-%E4%B8%8B%E9%9D%A2%E5%93%AA%E4%B8%80%E8%A1%8C%E4%BB%A3%E7%A0%81%E4%BC%9A-panic%E8%AF%B7%E8%AF%B4%E6%98%8E)
- [148. 下面代码输出什么](#148-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [149. 下面选项正确的是](#149-%E4%B8%8B%E9%9D%A2%E9%80%89%E9%A1%B9%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [150. 下面的代码输出什么](#150-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [151. 下面列举的是 recover() 的几种调用方式，哪些是正确的](#151-%E4%B8%8B%E9%9D%A2%E5%88%97%E4%B8%BE%E7%9A%84%E6%98%AF-recover-%E7%9A%84%E5%87%A0%E7%A7%8D%E8%B0%83%E7%94%A8%E6%96%B9%E5%BC%8F%E5%93%AA%E4%BA%9B%E6%98%AF%E6%AD%A3%E7%A1%AE%E7%9A%84)
- [152. 下面代码输出什么，请说明](#152-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E8%AF%B7%E8%AF%B4%E6%98%8E)
- [153. flag 是 bool 型变量，下面 if 表达式符合编码规范的是](#153-flag-%E6%98%AF-bool-%E5%9E%8B%E5%8F%98%E9%87%8F%E4%B8%8B%E9%9D%A2-if-%E8%A1%A8%E8%BE%BE%E5%BC%8F%E7%AC%A6%E5%90%88%E7%BC%96%E7%A0%81%E8%A7%84%E8%8C%83%E7%9A%84%E6%98%AF)
- [154. 下面的代码输出什么，请说明](#154-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E8%AF%B7%E8%AF%B4%E6%98%8E)
- [155. 下面的代码输出什么](#155-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [156. 下面的代码输出什么](#156-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [157. 下面的代码输出什么](#157-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [158. 下面的代码输出什么](#158-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [159. 下面代码有什么问题吗](#159-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98%E5%90%97)
- [160. 下面代码输出什么，请说明](#160-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E8%AF%B7%E8%AF%B4%E6%98%8E)
- [161. 关于 slice 或 map 操作，下面正确的是](#161-%E5%85%B3%E4%BA%8E-slice-%E6%88%96-map-%E6%93%8D%E4%BD%9C%E4%B8%8B%E9%9D%A2%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [162. 下面代码输出什么](#162-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [163. 关于字符串连接，下面语法正确的是](#163-%E5%85%B3%E4%BA%8E%E5%AD%97%E7%AC%A6%E4%B8%B2%E8%BF%9E%E6%8E%A5%E4%B8%8B%E9%9D%A2%E8%AF%AD%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [164. 下面代码能编译通过吗？可以的话，输出什么](#164-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%83%BD%E7%BC%96%E8%AF%91%E9%80%9A%E8%BF%87%E5%90%97%E5%8F%AF%E4%BB%A5%E7%9A%84%E8%AF%9D%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [165. 判断题：对变量x的取反操作是 ~x](#165-%E5%88%A4%E6%96%AD%E9%A2%98%E5%AF%B9%E5%8F%98%E9%87%8Fx%E7%9A%84%E5%8F%96%E5%8F%8D%E6%93%8D%E4%BD%9C%E6%98%AF-x)
- [166. 下面代码输出什么，请说明原因](#166-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E8%AF%B7%E8%AF%B4%E6%98%8E%E5%8E%9F%E5%9B%A0)
- [167. 下面的代码输出什么，请说明](#167-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E8%AF%B7%E8%AF%B4%E6%98%8E)
- [168. 下面的代码输出什么，请说明](#168-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E8%AF%B7%E8%AF%B4%E6%98%8E)
- [169. 下面代码输出什么](#169-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [170. 下面的代码能编译通过吗？可以的话输出什么，请说明](#170-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%83%BD%E7%BC%96%E8%AF%91%E9%80%9A%E8%BF%87%E5%90%97%E5%8F%AF%E4%BB%A5%E7%9A%84%E8%AF%9D%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E8%AF%B7%E8%AF%B4%E6%98%8E)
- [171. 下面代码有什么问题，请说明](#171-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98%E8%AF%B7%E8%AF%B4%E6%98%8E)
- [172. 假设 x 已声明，y 未声明，下面 4 行代码哪些是正确的。错误的请说明原因](#172-%E5%81%87%E8%AE%BE-x-%E5%B7%B2%E5%A3%B0%E6%98%8Ey-%E6%9C%AA%E5%A3%B0%E6%98%8E%E4%B8%8B%E9%9D%A2-4-%E8%A1%8C%E4%BB%A3%E7%A0%81%E5%93%AA%E4%BA%9B%E6%98%AF%E6%AD%A3%E7%A1%AE%E7%9A%84%E9%94%99%E8%AF%AF%E7%9A%84%E8%AF%B7%E8%AF%B4%E6%98%8E%E5%8E%9F%E5%9B%A0)
- [173. 下面的代码有什么问题，请说明](#173-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98%E8%AF%B7%E8%AF%B4%E6%98%8E)
- [174. 下面代码输出什么，为什么](#174-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E4%B8%BA%E4%BB%80%E4%B9%88)
- [175. 下面这段代码输出什么](#175-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [176. 下面的代码有什么问题](#176-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [177. 关于 cap 函数适用下面哪些类型](#177-%E5%85%B3%E4%BA%8E-cap-%E5%87%BD%E6%95%B0%E9%80%82%E7%94%A8%E4%B8%8B%E9%9D%A2%E5%93%AA%E4%BA%9B%E7%B1%BB%E5%9E%8B)
- [178. 下面代码输出什么](#178-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [179. 关于 switch 语句，下面说法正确的是](#179-%E5%85%B3%E4%BA%8E-switch-%E8%AF%AD%E5%8F%A5%E4%B8%8B%E9%9D%A2%E8%AF%B4%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [180. 下面代码能编译通过吗？可以的话，输出什么](#180-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%83%BD%E7%BC%96%E8%AF%91%E9%80%9A%E8%BF%87%E5%90%97%E5%8F%AF%E4%BB%A5%E7%9A%84%E8%AF%9D%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [181. interface{} 是可以指向任意对象的 Any 类型，是否正确](#181-interface-%E6%98%AF%E5%8F%AF%E4%BB%A5%E6%8C%87%E5%90%91%E4%BB%BB%E6%84%8F%E5%AF%B9%E8%B1%A1%E7%9A%84-any-%E7%B1%BB%E5%9E%8B%E6%98%AF%E5%90%A6%E6%AD%A3%E7%A1%AE)
- [182. 下面的代码有什么问题](#182-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [183. 定义一个包内全局字符串变量，下面语法正确的是](#183-%E5%AE%9A%E4%B9%89%E4%B8%80%E4%B8%AA%E5%8C%85%E5%86%85%E5%85%A8%E5%B1%80%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%8F%98%E9%87%8F%E4%B8%8B%E9%9D%A2%E8%AF%AD%E6%B3%95%E6%AD%A3%E7%A1%AE%E7%9A%84%E6%98%AF)
- [184. 下面的代码有什么问题](#184-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)
- [185. 下面的代码输出什么](#185-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [186. 下面代码中的指针 p 为野指针，因为返回的栈内存在函数结束时会被释放](#186-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E4%B8%AD%E7%9A%84%E6%8C%87%E9%92%88-p-%E4%B8%BA%E9%87%8E%E6%8C%87%E9%92%88%E5%9B%A0%E4%B8%BA%E8%BF%94%E5%9B%9E%E7%9A%84%E6%A0%88%E5%86%85%E5%AD%98%E5%9C%A8%E5%87%BD%E6%95%B0%E7%BB%93%E6%9D%9F%E6%97%B6%E4%BC%9A%E8%A2%AB%E9%87%8A%E6%94%BE)
- [187. 下面这段代码输出什么](#187-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [188. 下面代码输出什么](#188-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [190. 下面的代码有什么问题，请说明](#190-%E4%B8%8B%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E6%9C%89%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98%E8%AF%B7%E8%AF%B4%E6%98%8E)
- [191. 函数执行时，如果由于 panic 导致了异常，则延迟函数不会执行。这一说法是否正确](#191-%E5%87%BD%E6%95%B0%E6%89%A7%E8%A1%8C%E6%97%B6%E5%A6%82%E6%9E%9C%E7%94%B1%E4%BA%8E-panic-%E5%AF%BC%E8%87%B4%E4%BA%86%E5%BC%82%E5%B8%B8%E5%88%99%E5%BB%B6%E8%BF%9F%E5%87%BD%E6%95%B0%E4%B8%8D%E4%BC%9A%E6%89%A7%E8%A1%8C%E8%BF%99%E4%B8%80%E8%AF%B4%E6%B3%95%E6%98%AF%E5%90%A6%E6%AD%A3%E7%A1%AE)
- [192. 下面代码输出什么](#192-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)
- [193. 下面这段代码输出什么？请简要说明](#193-%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88%E8%AF%B7%E7%AE%80%E8%A6%81%E8%AF%B4%E6%98%8E)
- [194. 下面代码输出什么？](#194-%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81%E8%BE%93%E5%87%BA%E4%BB%80%E4%B9%88)

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

<font color="red">初始化interface时，指针类型的结构体能用值类型的方法，值类型不能使用指针类型的方法</font>

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

# 63. 下面这段代码输出什么

```go
type Direction int

const (
    North Direction = iota
    East
    South
    West
)

func (d Direction) String() string {
    return [...]string{"North", "East", "South", "West"}[d]
}

func main() {
    fmt.Println(South)
}
```

South

<font color="red">iota 的用法、类型的 String() 方法</font>。

# 64. 下面代码输出什么

```go
type Math struct {
    x, y int
}

var m = map[string]Math{
    "foo": Math{2, 3},
}

func main() {
    m["foo"].x = 4
    fmt.Println(m["foo"].x)
}
```

 compilation error

 map 的 value 本身是不可寻址的,有两个解决办法：

**使用临时变量**

```go
type Math struct {
    x, y int
}

var m = map[string]Math{
    "foo": Math{2, 3},
}

func main() {
    tmp := m["foo"]
    tmp.x = 4
    m["foo"] = tmp
    fmt.Println(m["foo"].x)
}
```

**修改数据结构,使用指针**

```go
type Math struct {
    x, y int
}

var m = map[string]*Math{
    "foo": &Math{2, 3},
}

func main() {
    m["foo"].x = 4
    fmt.Println(m["foo"].x)
    fmt.Printf("%#v", m["foo"])   // %#v 格式化输出详细信息
}
```

# 65. 下面的代码有什么问题

```go
func main() {
    fmt.Println([...]int{1} == [2]int{1}) // 数组类型不同，不能比较
    fmt.Println([]int{1} == []int{1}) // 引用类型不能比较
}
```

# 66. 下面这段代码输出什么？如果编译错误的话，为什么

```go
var p *int

func foo() (*int, error) {
    var i int = 5
    return &i, nil
}

func bar() {
    //use p
    fmt.Println(*p) // 全局变量的p
}

func main() {
    p, err := foo() // 重新声明了一个p
    if err != nil {
        fmt.Println(err)
        return
    }
    bar() // p==nil 解析*p 报错
    fmt.Println(*p)
}
```

runtime error

main() 函数里的 p 是新定义的变量，会遮住全局变量 p，导致执行到`bar()`时程序，全局变量 p 依然还是 nil，程序随即 Crash

正确的做法是将 main() 函数修改为：

```go
func main() {
    var err error
    p, err = foo()
    if err != nil {
        fmt.Println(err)
        return
    }
    bar()
    fmt.Println(*p)
}
```

# 67. 下面这段代码能否正常结束

```go
func main() {
    v := []int{1, 2, 3}
    for i := range v {
        v = append(v, i)
    }
}
```

不会出现死循环，能正常结束

循环次数在循环开始前就已经确定，循环内改变切片的长度，不影响循环次数。

<font color="red">切片循环前会保留一份副本，改变切片对循环次数没有影响，对map的增删改查影响循环次数</font>>

# 68. 下面这段代码输出什么？为什么

```go
func main() {

    var m = [...]int{1, 2, 3}

    for i, v := range m {
        go func() {
            fmt.Println(i, v)
        }()
    }

    time.Sleep(time.Second * 3)
}
```

output

```go
// 大概率上是
2 3
2 3
2 3
```

for range 使用短变量声明(:=)的形式迭代变量，需要注意的是，变量 i、v 在每次循环体中都会被重用，而不是重新声明。两种解决办法

**使用函数变量传递**

```go
for i, v := range m {
    go func(i,v int) {
        fmt.Println(i, v)
    }(i,v)
}
```

**使用零食变量**

```go
for i, v := range m {
    i := i           // 这里的 := 会重新声明变量，而不是重用
    v := v
    go func() {
        fmt.Println(i, v)
    }()
}
```

# 69. 下面这段代码输出什么

```go
func f(n int) (r int) {
    defer func() {
        r += n  // r = r+4
        recover()
    }()

    var f func()

    defer f() // panic
    f = func() {
        r += 2
    }
    return n + 1 // r = n + 1 =4
}

func main() {
    fmt.Println(f(3))
}
```

Output

```go
7
```

第一步执行`r = n +1`，接着执行第二个 defer，由于此时 f() 未定义，引发异常，随即执行第一个 defer，异常被 recover()，程序正常执行，最后 return。

#  70. 下面这段代码输出什么

```go
func main() {
    var a = [5]int{1, 2, 3, 4, 5}
    var r [5]int

    for i, v := range a {
        if i == 0 {
            a[1] = 12
            a[2] = 13
        }
        r[i] = v
    }
    fmt.Println("r = ", r)
    fmt.Println("a = ", a)
}
```

output

```bash
r =  [1 2 3 4 5]
a =  [1 12 13 4 5]
```

range 数组表达式是副本参与循环，就是说例子中参与循环的是 a 的副本，而不是真正的 a。

如果想要 r 和 a 一样输出，修复办法：

```go
// 使用指针
func main() {
    var a = [5]int{1, 2, 3, 4, 5}
    var r [5]int

    for i, v := range &a {
        if i == 0 {
            a[1] = 12
            a[2] = 13
        }
        r[i] = v
    }
    fmt.Println("r = ", r)
    fmt.Println("a = ", a)
}

// 重新取值
func main() {
	var a = [5]int{1, 2, 3, 4, 5}
	var r [5]int

	for i := range a {
		if i == 0 {
			a[1] = 12
			a[2] = 13
		}
		r[i] = a[i]
	}
	fmt.Println("r = ", r)
	fmt.Println("a = ", a)
}
```

# 71. 下面这段代码输出什么

```go
func change(s ...int) {
    s = append(s,3)
}

func main() {
    slice := make([]int,5,5)
    slice[0] = 1
    slice[1] = 2
    change(slice...)
    fmt.Println(slice)
    change(slice[0:2]...) // slice[0:2]用得是底层地址
    fmt.Println(slice)
}
```

Output:

```bash
[1 2 0 0 0]
[1 2 3 0 0]
```

# 72. 下面这段代码输出什么

```go
func main() {
    var a = []int{1, 2, 3, 4, 5}
    var r [5]int

    for i, v := range a {
        if i == 0 {
            // a = a[:2]  range a中的a没有影响
            a[1] = 12
            a[2] = 13
        }
        r[i] = v
    }
    fmt.Println("r = ", r)
    fmt.Println("a = ", a)
```

Output:

```bash
r =  [1 12 13 4 5]
a =  [1 12 13 4 5]
```

<font color="red">当 range 切片表达式发生修改时，副本的指针依旧指向原底层数组，所以对切片的修改都会反应到底层数组上，所以通过 v 可以获得修改后的数组元素</font>

<font color="red">当 range 切片表达式发生删除时，对range a中的a没有影响</font>

# 73. 下面这段代码输出结果正确吗

```go
type Foo struct {
    bar string
}
func main() {
    s1 := []Foo{
        {"A"},
        {"B"},
        {"C"},
    }
    s2 := make([]*Foo, len(s1))
    for i, value := range s1 {
        s2[i] = &value
    }
    fmt.Println(s1[0], s1[1], s1[2])
    fmt.Println(s2[0], s2[1], s2[2])
}

// output
{A} {B} {C}
&{A} &{B} &{C} // 应该是&{C} &{C} &{C}
```

s2 的输出结果错误 for range 使用短变量声明(:=)的形式迭代变量时，变量 i、value 在每次循环体中都会被重用，而不是重新声明。所以 s2 每次填充的都是临时变量 value 的地址,s2 输出的时候显示出了三个 &{c}。

# 74. 下面代码里的 counter 的输出值

```go
func main() {
    var m = map[string]int{
        "A": 21,
        "B": 22,
        "C": 23,
    }
    counter := 0
    for k, v := range m {
        if counter == 0 {
            delete(m, "A")
        }
        counter++
        fmt.Println(k, v)
    }
    fmt.Println("counter is ", counter)
}
```

 `2 或 3`

for range map 是无序的，如果第一次循环到 A，则输出 3；否则输出 2。

# 75. 关于协程，下面说法正确是

```go
A. 协程和线程都可以实现程序的并发执行；
B. 线程比协程更轻量级；
C. 协程不存在死锁问题；
D. 通过 channel 来进行协程间的通信；
```

A D

# 76. 关于循环语句，下面说法正确的有

```go
A. 循环语句既支持 for 关键字，也支持 while 和 do-while；
B. 关键字 for 的基本使用方法与 C/C++ 中没有任何差异；
C. for 循环支持 continue 和 break 来控制循环，但是它提供了一个更高级的 break，可以选择中断哪一个循环；
D. for 循环不支持以逗号为间隔的多个赋值语句，必须使用平行赋值的方式来初始化多个变量；
```

C D

# 77. 下面代码输出正确的是

```go
func main() {
    i := 1
    s := []string{"A", "B", "C"}
    i, s[i-1] = 2, "Z" // `i, s[0] = 2, "Z"`
    fmt.Printf("s: %v \n", s)
}
```

Output:

```go
s: [Z,B,C]
```

多重赋值分为两个步骤，有先后顺序：

- 计算等号左边的索引表达式和取址表达式，接着计算等号右边的表达式

- 赋值；

# 78. 关于类型转化，下面选项正确的是

```go
A.
type MyInt int
var i int = 1
var j MyInt = i

B.
type MyInt int
var i int = 1
var j MyInt = (MyInt)i

C.
type MyInt int
var i int = 1
var j MyInt = MyInt(i)

D.
type MyInt int
var i int = 1
var j MyInt = i.(MyInt)
```

C

# 79. 关于switch语句，下面说法正确的有

```go
A. 条件表达式必须为常量或者整数；
B. 单个case中，可以出现多个结果选项；
C. 需要用break来明确退出一个case；
D. 只有在case中明确添加fallthrough关键字，才会继续执行紧跟的下一个case；
```

B D

# 80. 如果 Add() 函数的调用代码为

则Add函数定义正确的是()

```go
func main() {
    var a Integer = 1
    var b Integer = 2
    var i interface{} = &a
    sum := i.(*Integer).Add(b)
    fmt.Println(sum)
}

A.
type Integer int
func (a Integer) Add(b Integer) Integer {
        return a + b
}

B.
type Integer int
func (a Integer) Add(b *Integer) Integer {
        return a + *b
}

C.
type Integer int
func (a *Integer) Add(b Integer) Integer {
        return *a + b
}

D.
type Integer int
func (a *Integer) Add(b *Integer) Integer {
        return *a + *b
}
```

A C

类型断言、方法集。

# 81. 关于 bool 变量 b 的赋值，下面错误的用法是

```go
A. b = true
B. b = 1
C. b = bool(1)
D. b = (1 == 2)
```

B C

# 82. 关于变量的自增和自减操作，下面语句正确的是

```go
A.
i := 1
i++

B.
i := 1
j = i++

C.
i := 1
++i

D.
i := 1
i--
```

A D 

i++ 和 i-- 在 Go 语言中是语句，不是表达式，因此不能赋值给另外的变量。此外没有 ++i 和 --i。

# 83. 关于GetPodAction定义,下面赋值正确的是

```go
type Fragment interface {
        Exec(transInfo *TransInfo) error
}
type GetPodAction struct {
}
func (g GetPodAction) Exec(transInfo *TransInfo) error {
        ...
        return nil
}

A. var fragment Fragment = new(GetPodAction)
B. var fragment Fragment = GetPodAction
C. var fragment Fragment = &GetPodAction{}
D. var fragment Fragment = GetPodAction{}
```

A C D

# 84. 关于函数声明，下面语法正确的是

```go
A. func f(a, b int) (value int, err error)
B. func f(a int, b int) (value int, err error)
C. func f(a, b int) (value int, error)
D. func f(a int, b int) (int, int, error)
```

A B D

# 85. 关于整型切片的初始化，下面正确的是

```go
A. s := make([]int) // make 声明slice要指定长度
B. s := make([]int, 0)
C. s := make([]int, 5, 10)
D. s := []int{1, 2, 3, 4, 5}
```

B C D

# 86. 下面代码会触发异常吗？请说明。

```go
func main() {
    runtime.GOMAXPROCS(1)
    int_chan := make(chan int, 1)
    string_chan := make(chan string, 1)
    int_chan <- 1
    string_chan <- "hello"
    select {
    case value := <-int_chan:
        fmt.Println(value)
    case value := <-string_chan:
        panic(value)
    }
}
```

`select` 会随机选择一个可用通道做收发操作，所以可能触发异常，也可能不会。

# 87. 关于channel的特性，下面说法正确的是

```go
A. 给一个 nil channel 发送数据，造成永远阻塞
B. 从一个 nil channel 接收数据，造成永远阻塞
C. 给一个已经关闭的 channel 发送数据，引起 panic
D. 从一个已经关闭的 channel 接收数据，如果缓冲区中为空，则返回一个零值
E. 无缓冲的channel 是同步的, 有缓冲的channel是异步的
```

A B C D E

<font color="red">nil chan 会永远阻塞</font>

# 88. 下面代码有什么问题？

```go
const i = 100
var j = 123

func main() {
    fmt.Println(&j, j)
    fmt.Println(&i, i) // 常量不能取地址
}
```

编译报错

常量不同于变量的在运行期分配内存，常量通常会被编译器在预处理阶段直接展开，作为指令数据使用，所以常量无法寻址

# 89. 下面代码能否编译通过？如果通过，输出什么

```go
func GetValue(m map[int]string, id int) (string, bool) {
    if _, exist := m[id]; exist {
        return "exist", true
    }
    return nil, false // string 默认值不是nil
}
func main() {
    intmap := map[int]string{
        1: "a",
        2: "b",
        3: "c",
    }

    v, err := GetValue(intmap, 3)
    fmt.Println(v, err)
}
```

不能通过编译，nil 可以用作 interface、function、pointer、map、slice 和 channel 的“空值”

# 90. 关于异常的触发，下面说法正确的是

```go
A. 空指针解析；
B. 下标越界；
C. 除数为0；
D. 调用panic函数；
```

A B C D

# 91. 下面代码输出什么

```go
func main() {
    x := []string{"a", "b", "c"}
    for v := range x {
        fmt.Print(v)
    }
}
```

Output

```bash
012
```

区别以下代码

```go
func main() {
    x := []string{"a", "b", "c"}
    for _, v := range x {
        fmt.Print(v)     //输出 abc
    }
}
```

# 92. 下面这段代码能否编译通过？如果通过，输出什么

```go
type User struct{}
type User1 User 	//  User 创建了新类型 User1
type User2 = User  //  User 的类型别名 User2

func (i User) m1() {
    fmt.Println("m1")
}
func (i User) m2() {
    fmt.Println("m2")
}

func main() {
    var i1 User1
    var i2 User2
    i1.m1() // type User1 has no field or method m1
    i2.m2()
}
```

不能编译,因为 User1 没有定义该方法

# 93. 关于无缓冲和有冲突的channel，下面说法正确的是

```go
A. 无缓冲的channel是默认的缓冲为1的channel；
B. 无缓冲的channel和有缓冲的channel都是同步的；
C. 无缓冲的channel和有缓冲的channel都是非同步的；
D. 无缓冲的channel是同步的，而有缓冲的channel是非同步的；
```

D

# 94. 下面代码是否能编译通过？如果通过，输出什么

```go
func Foo(x interface{}) {
     if x == nil {
         fmt.Println("empty interface")
         return
     }
     fmt.Println("non-empty interface")
 }

 func main() {
     var x *int = nil
     Foo(x)
}
```

non-empty interface

当且仅当动态值和动态类型都为 nil 时，接口类型值才为 nil。这里的 x 的动态类型是 `*int`，所以 x 不为 nil。

# 95. 下面代码输出什么

```go
func main() {
     ch := make(chan int, 100)
     // A
     go func() {              
         for i := 0; i < 10; i++ {
             ch <- i
         }
     }()
     // B
    go func() {
        for {
            a, ok := <-ch
            if !ok {
                fmt.Println("close")
                return
            }
            fmt.Println("a: ", a)
        }
    }()
    close(ch)
    fmt.Println("ok")
    time.Sleep(time.Second * 10)
}
```

程序抛异常 `panic: send on closed channel`

当 A 协程还没起时，主协程已经将 channel 关闭了

# 96. 关于select机制，下面说法正确的是

```go
A. select机制用来处理异步IO问题；
B. select机制最大的一条限制就是每个case语句里必须是一个IO操作；
C. golang在语言级别支持select关键字；
D. select关键字的用法与switch语句非常类似，后面要带判断条件；
```

A B C 

# 97. 下面的代码有什么问题

```go
unc Stop(stop <-chan bool) {
    close(stop)
}
```

编译错误，有方向的 消费 channel不能被关闭

<font color="red">由于 `close()` 函数只能接受 `chan<- T` 类型的 channel，如果我们尝试在接收方关闭 channel</font>

# 98. 下面这段代码存在什么问题

```go
type Param map[string]interface{}
 
type Show struct {
    *Param
}

func main() {
    s := new(Show) // Param 是map 需要被初始化才能使用
    s.Param["day"] = 2 //指针不支持索引。修复代码如下：
}
```

修改如下

```go
func main() {
    s := new(Show)
    // 修复代码
    p := make(Param)
    p["day"] = 2
    s.Param = &p
    tmp := *s.Param
    fmt.Println(tmp["day"])
}
```

# 99. 下面代码编译能通过吗

```go
func main()  
{  // { 应该在函数的右上方，不能放在单独的一行
    fmt.Println("hello world")
}
```

# 100. 下面这段代码输出什么

```go
var x = []int{2: 2, 3,8:8,7 0: 1}

func main() {
    fmt.Println(x)
}
```

Output:

```bash
[1 0 2 3 0 0 0 0 8 7]
```

<font color="red">字面量初始化切片时候，可以指定索引，没有指定索引的元素会在前一个索引基础之上加一</font>

# 101. 下面这段代码输出什么

```go
func incr(p *int) int {
    *p++
    return *p
}
func main() {
    v := 1
    incr(&v)
    fmt.Println(v)
}
```

Output:

```bash
2
```

# 102. 请指出下面代码的错误

```go
package main

var gvar int 

func main() {  
    var one int   
    two := 2      
    var three int 
    three = 3

    func(unused string) {
        fmt.Println("Unused arg. No compile error")
    }("what?")
}
```

<font color="red">报错：局部变量 one、two 和 three 声明未使用</font>

>  全局变量没使用不会报错

# 103. 下面代码输出什么

```go
type ConfigOne struct {
    Daemon string
}

func (c *ConfigOne) String() string {
    return fmt.Sprintf("print: %v", c)
}

func main() {
    c := &ConfigOne{}
    c.String()
}
```

运行时错误`stack overflow`

如果类型实现 String() 方法，当格式化输出时会自动使用 String() 方法。上面这段代码是在该类型的 String() 方法内使用格式化输出，导致递归调用，最后抛错。

# 104. 下面代码输出什么

```go
func main() {
    var a = []int{1, 2, 3, 4, 5}
    var r = make([]int, 0)

    for i, v := range a {
        if i == 0 {
            a = append(a, 6, 7)
        }
        r = append(r, v)
    }
    fmt.Println(r)
}
```

Output

```go
[1 2 3 4 5]
```

 for range 时会使用 a 的副本 a' 参与循环，副本的 len 依旧是 5，因此 for range 只会循环 5 次，也就只获取 a 对应的底层数组的前 5 个元素。

# 105. 下面的代码有什么问题

```go
import (  
    "fmt"
    "log"
    "time"
)
func main() {  
}
```

导入的包没有被使用

# 106. 下面代码输出什么

```go
func main() {
    x := interface{}(nil)
    y := (*int)(nil)
    a := y == x // false
    b := y == nil // true
    _, c := x.(interface{}) //false 
    println(a, b, c)
}
```

false true false

类型断言语法：i.(Type)，其中 i 是接口，Type 是类型或接口。编译时会自动检测 i 的动态类型与 Type 是否一致。但是，如果动态类型不存在，则断言总是失败。

# 107. 下面代码有几处错误的地方？请说明原因

```go
func main() {
    var s []int
    s = append(s,1)

    var m map[string]int // 不能对 nil 的 map 直接赋值，需要使用 make() 初始化。
    m["one"] = 1 
}
```

一处

# 108. 下面代码有什么问题

```go
func main() {
    m := make(map[string]int,2) //2 会被忽略
    cap(m) // 不能使用 cap() 获取 map 的容量
}
```

不能使用 cap() 获取 map 的容量

使用 make 创建 map 变量时可以指定第二个参数，不过会被忽略。

<font color="red">cap() 函数适用于数组、数组指针、slice 和 channel，不适用于 map，可以使用 len() 返回 map 的元素个数。</font>

#  109. 下面的代码有什么问题

```go
func main() {  
    var x = nil // 推导错误 var x interface{} = nil
    _ = x
}
```

# 110. 下面代码能编译通过吗

```go
type info struct {
    result int
}

func work() (int,error) {
    return 13,nil
}

func main() {
    var data info
	
    data.result, err := work() // 提前定义error
    fmt.Printf("info: %+v\n",data)
}
```

不能使用短变量声明设置结构体字段值

# 111. 下面代码有什么错误

```go
func main() {
    one := 0
    one := 1 
}
```

不能在单独的声明中重复声明一个变量，但在多变量声明的时候是可以的，但必须保证至少有一个变量是新声明的。

```go
func main() {  
    one := 0
    one, two := 1,2
    one,two = two,one
}
```

# 112. 下面代码有什么问题

```go
func main() {
    x := []int{
        1,
        2  // 少个逗号 
    }
    _ = x
}
```

# 113. 下面代码输出什么

```go
func test(x byte)  {
    fmt.Println(x)
}

func main() {
    var a byte = 0x11 
    var b uint8 = a
    var c uint8 = a + b
    test(c)
}
```

34

byte 是 uint8 的别名，别名类型无序转换，可直接转换

# 114. 下面的代码有什么问题

```go
func main() {
    const x = 123
    const y = 1.23
    fmt.Println(x)
}
```

编译可以通过

常量是一个简单值的标识符，在程序运行时，不会被修改的量。不像局部变量，常量未使用是能编译通过的。

# 115. 下面代码输出什么

```go
const (
    x uint16 = 120
    y
    s = "abc"
    z
)

func main() {
    fmt.Printf("%T %v\n", y, y)
    fmt.Printf("%T %v\n", z, z)
}
```

Output:

```go
uint16 120
string abc
```

<font color="red">常量组中如不指定类型和初始化值，则与上一行非空常量右值相同</font>

# 116. 下面代码有什么问题

```go
func main() {  
    var x string = nil //将 nil 分配给 string 类型的变量

    if x == nil { 
        x = "default"
    }
```

# 117. 下面的代码有什么问题

```go
func main() {
    data := []int{1,2,3}
    i := 0
    ++i // golang 没有++i
    fmt.Println(data[i++])
}
```

# 118. 下面代码最后一行输出什么

```go
func main() {
    x := 1
    fmt.Println(x)
    {
        fmt.Println(x)
        i,x := 2,2
        fmt.Println(i,x)
    }
    fmt.Println(x)  // print ?
}
```

1

代码块儿作用域

# 119. 下面代码有什么问题

```go
type foo struct {
    bar int
}

func main() {
    var f foo
    f.bar, tmp := 1, 2 // `:=` 操作符不能用于结构体字段赋值。
}
```

# 120. 下面的代码输出什么

```go
func main() {  
    fmt.Println(~2) 
}
```

很多语言都是采用 `~` 作为按位取反运算符，Go 里面采用的是` ^` 。按位取反之后返回一个每个 bit 位都取反的数，对于有符号的整数来说，是按照补码进行取反操作的（快速计算方法：对数 a 取反，结果为 -(a+1) ）

```go
func main() {
    var a int8 = 3
    var b uint8 = 3
    var c int8 = -3

    fmt.Printf("^%b=%b %d\n", a, ^a, ^a) // ^11=-100 -4
    fmt.Printf("^%b=%b %d\n", b, ^b, ^b) // ^11=11111100 252
    fmt.Printf("^%b=%b %d\n", c, ^c, ^c) // ^-11=10 2
}
```

如果作为二元运算符，^ 表示按位异或，即：对应位相同为 0，相异为 1

```go
func main() {
    var a int8 = 3
    var c int8 = 5

    fmt.Printf("a: %08b\n",a)
    fmt.Printf("c: %08b\n",c)
    fmt.Printf("a^c: %08b\n",a ^ c)
}
```

# 121. 下面代码输出什么

```go
func main() {
    var ch chan int
    select {
    case v, ok := <-ch:
        println(v, ok)
    default:
        println("default") 
    }
}
```

default

ch 为 nil，读写都会阻塞

# 122. 下面这段代码输出什么

```go
type People struct {
    name string `json:"name"` // name 应该大写
}

func main() {
    js := `{
        "name":"seekload"
    }`
    var p People
    err := json.Unmarshal([]byte(js), &p)
    if err != nil {
        fmt.Println("err: ", err)
        return
    }
  fmt.Println(p) // {}
}
```

结构体访问控制，因为 name 首字母是小写，导致其他包不能访问，所以输出为空结构体。

# 123. 下面这段代码输出什么

```go
type T struct {
    ls []int
}

func foo(t T) {
    t.ls[0] = 100
}

func main() {
    var t = T{
        ls: []int{1, 2, 3},
    }

    foo(t)
    fmt.Println(t.ls[0])
}
```

100

调用 foo() 函数时虽然是传值，但 foo() 函数中，<font color="red">字段 ls 依旧可以看成是指向底层数组的指针</font>。

# 124. 下面代码输出什么

```go
func main() {
    isMatch := func(i int) bool {
        switch(i) {
        case 1:
        case 2:
            return true
        }
        return false
    }

    fmt.Println(isMatch(1))
    fmt.Println(isMatch(2))
}
```

false true

case 完成程序会默认 break

# 125. 下面的代码能否正确输出

```go
func main() {
    var fn1 = func() {}
    var fn2 = func() {}

    if fn1 != fn2 { // 函数只能与 nil 比较。
        println("fn1 not equal fn2")
    }
}
```

编译错误

函数只能与 nil 比较。

# 126. 下面代码输出什么

```go
type T struct {
    n int
}

func main() {
    m := make(map[int]T)
    m[0].n = 1
    fmt.Println(m[0].n)
}
```

编译错误

map[key]值类型是不可寻址的，所以无法直接赋值。

# 127. 下面的代码有什么问题

```go
type X struct {}

func (x *X) test()  {
    println(x)
}

func main() {
    var a *X
    a.test()
    X{}.test() // X{} 是不可寻址的，不能直接调用方法
}
```

X{} 是不可寻址的，不能直接调用方法

修复代码：

```go
func main() {

    var a *X
    a.test()    // 相当于 test(nil)

    var x = X{}
    x.test()
}
```

# 128. 下面代码有什么不规范的地方吗

```go
func main() {
    x := map[string]string{"one":"a","two":"","three":"c"}

    if v := x["two"]; v == "" {  // 最可靠的操作是使用访问 map 时返回的第二个值。
        fmt.Println("no entry")
    }
}
```

最可靠的操作是使用访问 map 时返回的第二个值

```go
func main() {  
    x := map[string]string{"one":"a","two":"","three":"c"}

    if _,ok := x["two"]; !ok {
        fmt.Println("no entry")
    }
}
```

# 129. 关于 channel 下面描述正确的是

```go
A. 向已关闭的通道发送数据会引发 panic；
B. 从已关闭的缓冲通道接收数据，返回已缓冲数据或者零值；
C. 无论接收还是接收，nil 通道都会阻塞；
```

A B C

# 130. 下面的代码有几处问题？请详细说明

```go
type T struct {
    n int
}

func (t *T) Set(n int) {
    t.n = n
}

func getT() T {
    return T{}
}

func main() {
    getT().Set(1)
}
```

直接返回的 T{} 不可寻址

不可寻址的结构体不能调用带结构体指针接收者的方法；

# 131. 下面的代码有什么问题

```go
type N int

func (n N) value(){
    n++
    fmt.Printf("v:%p,%v\n",&n,n)
}

func (n *N) pointer(){
    *n++
    fmt.Printf("v:%p,%v\n",n,*n)
}


func main() {

    var a N = 25

    p := &a
    p1 := &p // 不能使用多级指针调用方法。

    p1.value()
    p1.pointer()
}
```

编译错误

# 132. 下面的代码输出什么

```go
type N int

func (n N) test(){
    fmt.Println(n)
}

func main()  {
    var n N = 10
    fmt.Println(n)

    n++
    f1 := N.test
    f1(n)

    n++
    f2 := (*N).test
    f2(&n)
}
```

Output

```go
10 11 12
```

（*T）可以使用T的方法

# 133. 关于 channel 下面描述正确的是

```go
A. close() 可以用于只接收通道；
B. 单向通道可以转换为双向通道；
C. 不能在单向通道上做逆向操作（例如：只发送通道用于接收）；
```

C

# 134. 下面的代码有什么问题

```go
type T struct {
    n int
}

func getT() T {
    return T{}
}

func main() {
    getT().n = 1 // 直接返回的 T{} 无法寻址，不可直接赋值
}
```

编译错误

 T{} 无法寻址

# 135. 下面的代码有什么问题

```go
package main

import "fmt"

func main() {
    s := make([]int, 3, 9)
    fmt.Println(len(s)) // 3
    s2 := s[4:8] // 派生出的子切片的长度可能大于基础切片的长度 0 <= low <= high <= cap(baseSlice)
    fmt.Println(len(s2)) // 4
}
```

Output

```go
3 4 
```



从一个基础切片派生出的子切片的长度可能大于基础切片的长度

假设基础切片是 baseSlice，使用操作符 [low,high]，有如下规则：<font color="red">0 <= low <= high <= cap(baseSlice)</font>，只要上述满足这个关系，下标 low 和 high 都可以大于 len(baseSlice)。

# 136. 下面代码输出什么

```go
type N int

func (n N) test(){
    fmt.Println(n)
}

func main()  {
    var n N = 10
    p := &n

    n++  // 11
    f1 := n.test

    n++ //12
    f2 := p.test 

    n++
    fmt.Println(n) /13

    f1() // 11
    f2() // 12
}
```

Output

```go
13 11 12
```

# 137. 下面哪一行代码会 panic，请说明原因

```go
package main

func main() {
  var x interface{}
  var y interface{} = []int{3, 5}
  _ = x == x  // nil == nil
  _ = x == y  // nil == []int{3, 5}
  _ = y == y   // 切片属于引用类型不能比较
}
```

_ = y == y

引用类型为不可比较类型。

# 138. 下面的代码输出什么

```go
var o = fmt.Print

func main() {
    c := make(chan int, 1)
    for range [3]struct{}{} {
        select {
        default:
            o(1)
        case <-c:
            o(2)
            c = nil
        case c <- 1:
            o(3)
        }
    }
}
```

Output

```go
3 2 1
```

第一次循环，写操作已经准备好，执行 o(3)，输出 3；

第二次，读操作准备好，执行 o(2)，输出 2 并将 c 赋值为 nil；

第三次，由于 c 为 nil，走的是 default 分支，输出 1。

# 139. 下面的代码输出什么

```go
type T struct {
    x int
    y *int
}

func main() {
    i := 20
    t := T{10,&i}

    p := &t.x

    *p++
    *p--

    t.y = p

    fmt.Println(*t.y)
}
```

Output

```bash
10
```

递增运算符 ++ 和递减运算符 -- 的优先级低于解引用运算符 * 和取址运算符 &，解引用运算符和取址运算符的优先级低于选择器 . 中的属性选择操作符。

# 140. 下面哪一行代码会 panic，请说明原因

```go
package main

func main() {
    x := make([]int, 2, 10)
    _ = x[6:10] 
    _ = x[6:] // 截取符号 [i:j]，如果 j 省略，默认是原切片或者数组的长度，x 的长度是 2，小于起始下标 6 ，所以 panic。
    _ = x[2:]
}
```

础切片是 baseSlice，使用操作符 [low,high]，有如下规则：<font color="red">0 <= low <= high <= cap(baseSlice)</font>

截取符号 [i:j]，如果 j 省略，默认是原切片或者数组的长度

# 141. 下面的代码输出什么

```go
type N int

func (n *N) test(){
    fmt.Println(*n)
}

func main()  {
    var n N = 10
    p := &n

    n++
    f1 := n.test

    n++
    f2 := p.test

    n++
    fmt.Println(n) // 13

    f1()
    f2()
}
```

13 13 13

当目标方法的接收者是指针类型时，那么被复制的就是指针。

# 142. 下面哪一行代码会 panic，请说明原因

```go
package main

func main() {
  var m map[int]bool // nil
  _ = m[123]
  var p *[5]string // nil
  for range p {
    _ = len(p)
  }
  var s []int // nil
  _ = s[:]
  s, s[0] = []int{1, 2}, 9 //  s 为 nil。
}
```

 s 为 nil s[0] panic。

# 143. 下面哪一行代码会 panic，请说明原因

```go
package main

type T struct{}

func (*T) foo() {
}

func (T) bar() {
}

type S struct {
  *T
}

func main() {
  s := S{}
  _ = s.foo
  s.foo()
  _ = s.bar // s.T 是空指针
}
```

因为 s.bar 将被展开为 (*s.T).bar，而 s.T 是个空指针，解引用会 panic。

# 144. 下面的代码有什么问题

```go
type data struct {
    sync.Mutex
}

func (d data) test(s string)  { // Mutex 禁止复制
    d.Lock()
    defer d.Unlock()

    for i:=0;i<5 ;i++  {
        fmt.Println(s,i)
        time.Sleep(time.Second)
    }
}


func main() {

    var wg sync.WaitGroup
    wg.Add(2)
    var d data

    go func() {
        defer wg.Done()
        d.test("read")
    }()

    go func() {
        defer wg.Done()
        d.test("write")
    }()

    wg.Wait()
}
```

将 Mutex 作为匿名字段时，相关的方法必须使用指针接收者，否则会导致锁机制失效。

修复代码：

```go
func (d *data) test(s string)  {     // 指针接收者
    d.Lock()
    defer d.Unlock()

    for i:=0;i<5 ;i++  {
        fmt.Println(s,i)
        time.Sleep(time.Second)
    }
}
```

或者可以通过嵌入 `*Mutex` 来避免复制的问题，但需要初始化。

```go
type data struct {
    *sync.Mutex     // *Mutex
}

func (d data) test(s string) {    // 值方法
    d.Lock()
    defer d.Unlock()

    for i := 0; i < 5; i++ {
        fmt.Println(s, i)
        time.Sleep(time.Second)
    }
}

func main() {

    var wg sync.WaitGroup
    wg.Add(2)

    d := data{new(sync.Mutex)}   // 初始化

    go func() {
        defer wg.Done()
        d.test("read")
    }()

    go func() {
        defer wg.Done()
        d.test("write")
    }()

    wg.Wait()
}
```

# 145. 下面这段代码输出什么

```go
func main() {
    var k = 1
    var s = []int{1, 2}
    k, s[k] = 0, 3
    fmt.Println(s[0] + s[1])
}
```

Output

```go
4
```

# 146. 下面代码输出什么

```go
func main() {
    var k = 9
    for k = range []int{} {} // 跳过
    fmt.Println(k) // 9

    for k = 0; k < 3; k++ {
    }
    fmt.Println(k) //3


    for k = range (*[3]int)(nil) { 0 1 2
    }
    fmt.Println(k)
}
```

Output

```go
9 3 2
```

# 147. 下面哪一行代码会 panic，请说明

```go
func main() {
    nil := 123 // nil = 123
    fmt.Println(nil)
    var _ map[string]int = nil // 此时 nil 是 int 类型值
}
```

当前作用域中，预定义的 nil 被覆盖，此时 nil 是 int 类型值，不能赋值给 map 类型。

# 148. 下面代码输出什么

```go
func main() {
    var x int8 = -128
    var y = x/-1 //  128 溢出
    fmt.Println(y) 
}
```

Output

```go
-128
```

溢出 int8 -> ~128-127

# 149. 下面选项正确的是

```go
A. 类型可以声明的函数体内；
B. Go 语言支持 ++i 或者 --i 操作；
C. nil 是关键字； // nil 是常量
D. 匿名函数可以直接赋值给一个变量或者直接执行；
```

A D

<font color="red">nil 是常量</font>

# 150. 下面的代码输出什么

```go
func F(n int) func() int {
    return func() int {
        n++
        return n
    }
}

func main() {
    f := F(5)
    defer func() {
        fmt.Println(f()) // 8
    }()
    defer fmt.Println(f()) // 6
    i := f() //7
    fmt.Println(i) // 7
}
```

Output

```go
7 6 8
```

`闭包保存变量`

# 151. 下面列举的是 recover() 的几种调用方式，哪些是正确的

```go
A.
func main() {
    recover()
    panic(1)
}

B.
func main() {
    defer recover()
    panic(1)
}

C.
func main() {
    defer func() {
        recover()
    }()
    panic(1)
}

D.
func main() {
    defer func() {
        defer func() {
            recover()
        }()
    }()
    panic(1)
}
```

C

<font color="red">recover() 必须在 defer() func函数中直接调用才有效。</font>

直接调用 recover()、在 defer() 中直接调用 recover() 和 defer() 调用时多层嵌套 是无效的

# 152. 下面代码输出什么，请说明

```go
func main() {
    defer func() {
        fmt.Print(recover()) //1
    }()
    defer func() {
        defer fmt.Print(recover()) // 2
        panic(1)
    }()
    defer recover() 
    panic(2)
}
```

Output:

```go
2 1
```

# 153. flag 是 bool 型变量，下面 if 表达式符合编码规范的是

```go
A. if flag == 1
B. if flag
C. if flag == false
D. if !flag
```

B C D

# 154. 下面的代码输出什么，请说明

```go
func main() {
    defer func() {
        fmt.Print(recover()) // 2
    }()
    defer func() {
        defer func() {
            fmt.Print(recover()) // 1
        }()
        panic(1)
    }()
    defer recover()
    panic(2)
}
```

Output:

```go
1 2
```

# 155. 下面的代码输出什么

```go
type T struct {
    n int
}

func main() {
    ts := [2]T{}
    for i, t := range ts {
        switch i {
        case 0:
            t.n = 3
            ts[1].n = 9
        case 1:
            fmt.Print(t.n, " ") // 使用的是 数组 ts 的副本，所以 t.n = 3 的赋值操作不会影响原数组。
        }
    }
    fmt.Print(ts)
}
```

Output:

```go
0 [{0} {9}]
```

<font color="red">range 数组时，使用的数组的副本，数组数据隔离，修改数组的值不会影响取值</font>

<font color="red"> range 切片时，底层数据隔离，修改切片的值，影响取值</font>

# 156. 下面的代码输出什么

```go
type T struct {
    n int
}

func main() {
    ts := [2]T{}
    for i, t := range &ts { // 数组指针 ts的地址
        switch i {
        case 0:
            t.n = 3
            ts[1].n = 9
        case 1:
            fmt.Print(t.n, " ")
        }
    }
    fmt.Print(ts)
}
```

Output:

```go
9 [{0} {9}]
```

# 157. 下面的代码输出什么

```go
type T struct {
    n int
}

func main() {
    ts := [2]T{}
    for i := range ts[:] {
        switch i {
        case 0:
            ts[1].n = 9
        case 1:
            fmt.Print(ts[i].n, " ")
        }
    }
    fmt.Print(ts)
}
```

Output:

```bash
9 [{0} {9}]
```

for-range 切片时使用的是切片的副本，但不会复制底层数组，换句话说，此副本切片与原数组共享底层数组。

# 158. 下面的代码输出什么

```go
type T struct {
    n int
}

func main() {
    ts := [2]T{}
    for i := range ts[:] {
        switch t := &ts[i]; i {
        case 0:
            t.n = 3;
            ts[1].n = 9
        case 1:
            fmt.Print(t.n, " ")
        }
    }
    fmt.Print(ts)
}
```

Output:

```go
9 [{0} {9}]
```

# 159. 下面代码有什么问题吗

```go
func main()  {
    for i:=0;i<10 ;i++  {
    loop:
        println(i)
    }
    goto loop // goto 不能跳转到内层代码
}
```

编译错误

<font color="red">goto 不能跳转到其他函数或者内层代码</font>

# 160. 下面代码输出什么，请说明

```go
func main() {
    x := []int{0, 1, 2}
    y := [3]*int{}
    for i, v := range x {
        defer func() {
            print(v) // 2 2 2
        }()
        y[i] = &v
    }
    print(*y[0], *y[1], *y[2]) // 2 2 2
}
```

Output:

```go
222222
```

# 161. 关于 slice 或 map 操作，下面正确的是

```go
A.
var s []int
s = append(s,1)

B. // map 初始化需要申请空间
var m map[string]int
m["one"] = 1 

C.
var s []int
s = make([]int, 0)
s = append(s,1)

D.
var m map[string]int
m = make(map[string]int)
m["one"] = 1 
```

A C D

# 162. 下面代码输出什么

```go
func test(x int) (func(), func()) {
    return func() {
        println(x)
        x += 10
    }, func() {
        println(x)
    }
}

func main() {
    a, b := test(100)
    a() // 100
    b() // 110
}
```

Output:

```go
100 110
```

# 163. 关于字符串连接，下面语法正确的是

```go
A. str := 'abc' + '123'  //单引号表示 rune 类型
B. str := "abc" + "123"
C. str ：= '123' + "abc"
D. fmt.Sprintf("abc%d", 123)
```

B D

# 164. 下面代码能编译通过吗？可以的话，输出什么

```go
func main() {
    println(DeferTest1(1)) // 4
    println(DeferTest2(1)) // 3
}

func DeferTest1(i int) (r int) {
    r = i
    defer func() {
        r += 3
    }()
    return r
}

func DeferTest2(i int) (r int) {
    defer func() {
        r += i
    }()
    return 2
}
```

Output:

```go
4 3
```

# 165. 判断题：对变量x的取反操作是 ~x

Go 语言的取反操作是 `^`

# 166. 下面代码输出什么，请说明原因

```go
type Slice []int

func NewSlice() Slice {
    return make(Slice, 0)
}
func (s *Slice) Add(elem int) *Slice {
    *s = append(*s, elem)
    fmt.Print(elem)
    return s
}
func main() {
    s := NewSlice()
    defer s.Add(1).Add(2) // add(2) 会执行
    s.Add(3)
}
```

Output:

```go
2 3 1
```

# 167. 下面的代码输出什么，请说明

```go
type Slice []int

func NewSlice() Slice {
    return make(Slice, 0)
}
func (s *Slice) Add(elem int) *Slice {
    *s = append(*s, elem)
    fmt.Print(elem)
    return s
}
func main() {
    s := NewSlice()
    defer func() {
        s.Add(1).Add(2)
    }()
    s.Add(3)
}
```

Output:

```go
3 1 2
```

# 168. 下面的代码输出什么，请说明

```go
type Orange struct {
    Quantity int
}

func (o *Orange) Increase(n int) {
    o.Quantity += n
}

func (o *Orange) Decrease(n int) {
    o.Quantity -= n
}

func (o *Orange) String() string { 
   // String() 是指针方法，而不是值方法，所以使用 Println() 输出时不会调用到 String() 方法。
    return fmt.Sprintf("%#v", o.Quantity)
}

func main() {
    var orange Orange
    orange.Increase(10)
    orange.Decrease(5)
    fmt.Println(orange)
}
```

Output

```go
{5} // 结构体{5}
```

<font color="red"> 在结构体未赋值给 `interface`时, 指针类型方法可以接受值类型</font>

<font color="red"> fmt打印时调用的是指针方法String()</font>

# 169. 下面代码输出什么

```go
func test() []func() {
    var funs []func()
    for i := 0; i < 2; i++ {
        funs = append(funs, func() {
            println(&i, i)
        })
    }
    return funs
}

func main() {
    funs := test()
    for _, f := range funs {
        f()
    }
}
```

Output:

```go
// 每台设备上执行 地址一样 值一样
0x14000102000 2
0x14000102000 2
```

# 170. 下面的代码能编译通过吗？可以的话输出什么，请说明

```go
var f = func(i int) {
    print("x")
}

func main() {
    f := func(i int) {
        print(i)
        if i > 0 {
            f(i - 1) // 外层的f
        }
    }
    f(10)
}
```

Output:

```bash
10X
```

# 171. 下面代码有什么问题，请说明

```go
func main() {
    runtime.GOMAXPROCS(1)
  
    go func() {
        for i:=0;i<10 ;i++  {
            fmt.Println(i)
        }
    }()

    for {} // 1.14之前的版本 for {} 独占 CPU 资源导致其他 Goroutine 饿死，
}
```

go1.14版本之前(不含1.14版本): for {} 独占 CPU 资源导致其他 Goroutine 饿死。 可以改成 `select {}` 可以改成`nil chan` 取值

后面的版本改过了这个问题, 但是不推荐这样写

# 172. 假设 x 已声明，y 未声明，下面 4 行代码哪些是正确的。错误的请说明原因

```go
A.x, _ := f()  // x, _ = f() 
B.x, _ = f()  
C.x, y := f()  
D.x, y = f()  // x, y := f()
```

B C

# 173. 下面的代码有什么问题，请说明

```go
func main() {
    f, err := os.Open("file")
    defer f.Close()
    if err != nil {
        return
    }

    b, err := ioutil.ReadAll(f)
    println(string(b))
}
```

defer 应该放在err后面

# 174. 下面代码输出什么，为什么

```go
func f() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Printf("recover:%#v", r)
        }
    }()
    panic(1)
    panic(2)
}

func main() {
    f()
}
```

recover:1

当程序 panic 时就不会往下执行，可以使用 recover() 捕获 panic 的内容。

# 175. 下面这段代码输出什么

```go
type S1 struct{}

func (s1 S1) f() {
    fmt.Println("S1.f()")
}
func (s1 S1) g() {
    fmt.Println("S1.g()")
}

type S2 struct {
    S1
}

func (s2 S2) f() {
    fmt.Println("S2.f()")
}

type I interface {
    f()
}

func printType(i I) {

    fmt.Printf("%T\n", i)
    if s1, ok := i.(S1); ok {
        s1.f()
        s1.g()
    }
    if s2, ok := i.(S2); ok {
        s2.f()
        s2.g()
    }
}

func main() {
    printType(S1{})
    printType(S2{})
}
```

Output:

```go
main.S1
S1.f()
S1.g()

main.S2
S2.f()
S1.g()
```

# 176. 下面的代码有什么问题

```go
func main() {
    var wg sync.WaitGroup
    wg.Add(1)
  
    go func() {
        fmt.Println("1")
        wg.Done()
        wg.Add(1) // WaitGroup is reused before previous Wait has returned
    }()
    wg.Wait()
}
```

会 `panic`

<font color="red">wg.Add(num)的数量 在 goroutine 结束之后没有Done完也会报错,如下</font>

```go
func main() {
    var wg sync.WaitGroup
    wg.Add(12)
    go func() {
        fmt.Println("1")
        wg.Done()
    }()
    wg.Wait()
}

// fatal error: all goroutines are asleep - deadlock!
```

# 177. 关于 cap 函数适用下面哪些类型

```go
A. 数组
B. channel
C. map
D. slice
```

A B D

`cap()`函数的作用是：

* array 返回数组的元素个数
* slice 返回slice的最大容量
* channel 返回 channel的容量

# 178. 下面代码输出什么

```go
func hello(num ...int) {
    num[0] = 18
}

func Test13(t *testing.T) {
    i := []int{5, 6, 7}
    hello(i...)
    fmt.Println(i[0])
}

func main() {
    t := &testing.T{}
    Test13(t)
}
```

18

可变函数传递slice，与原底层数据共享

# 179. 关于 switch 语句，下面说法正确的是

```go
A. 单个 case 中，可以出现多个结果选项；
B. 需要使用 break 来明确退出一个 case;
C. 只有在 case 中明确添加 fallthrought 关键字，才会继续执行紧跟的下一个 case;
D. 条件表达式必须为常量或者整数；
```

A C

# 180. 下面代码能编译通过吗？可以的话，输出什么

```go
func alwaysFalse() bool {
    return false
}

func main() {
    switch alwaysFalse() // go语言断行规则
    {
    case true:
        println(true)
    case false:
        println(false)
    }
}
```

true

其实 Go 语言的断行规则定义如下：

1. 在 Go 代码中，注释除外，如果一个代码行的最后一个语法词段（ token ）为下列所示之一，则一个分号将自动插入在此字段后（即行尾）：

- 一个标识符；
- 一个整数、浮点数、虚部、码点或者字符串字面表示形式；
- 这几个跳转关键字之一：`break`、`continue`、`fallthrough`和`return`；
- 自增运算符`++`或者自减运算符`--`；
- 一个右括号：`)`、`]`或`}`。

# 181. interface{} 是可以指向任意对象的 Any 类型，是否正确

true

# 182. 下面的代码有什么问题

```go
type ConfigOne struct {
    Daemon string
}

func (c *ConfigOne) String() string {
    return fmt.Sprintf("print: %v", c)
}

func main() {
    c := &ConfigOne{}
    c.String()
}
```

无限递归循环，栈溢出

`指针类型`的 String() 方法

如果`指针类型`定义了 String() 方法，使用 Printf()、Print() 、 Println() 、 Sprintf() 等格式化输出时会自动使用 String() 方法。

# 183. 定义一个包内全局字符串变量，下面语法正确的是

```go
A. var str string
B. str := "" // func 
C. str = ""  // error
D. var str = ""
```

A D

# 184. 下面的代码有什么问题

```go
func main() {

    wg := sync.WaitGroup{}
    for i := 0; i < 5; i++ {
        go func(wg sync.WaitGroup, i int) { // 禁止拷贝 sync.WaitGroup 
            wg.Add(1)
            fmt.Printf("i:%d\n", i)
            wg.Done()
        }(wg, i)
    }

    wg.Wait()

    fmt.Println("exit")
}
```

- 在协程中使用 wg.Add()
- 使用了 sync.WaitGroup 副本

# 185. 下面的代码输出什么

```go
func main() {
    var a []int = nil
    a, a[0] = []int{1, 2}, 9 // a[0] error
    fmt.Println(a)
}
```

运行时错误 

# 186. 下面代码中的指针 p 为野指针，因为返回的栈内存在函数结束时会被释放

```go
type TimesMatcher struct {
    base int
}

func NewTimesMatcher(base int) *TimesMatcher  {
    return &TimesMatcher{base:base}
}

func main() {
    p := NewTimesMatcher(3)
    fmt.Println(p)
}
```

false

Go语言的内存回收机制规定，只要有一个指针指向引用一个变量，那么这个变量就不会被释放（内存逃逸），因此在 Go 语言中返回函数参数或临时变量是安全的。

# 187. 下面这段代码输出什么

```go
func main() {
    count := 0
    for i := range [256]struct{}{} {
        m, n := byte(i), int8(i)
        if n == -n { // 0 128 
            count++
        }
        if m == -m { // 0 128 
            count++
        }
    }
    fmt.Println(count)
}
```

4 

数值溢出。当 i 的值为 0、128 是会发生相等情况，注意 byte 是 uint8 的别名。

# 188. 下面代码输出什么

```go
const (
    azero = iota //0
    aone  = iota /1
)

const (
    info  = "msg"
    bzero = iota // 1
    bone  = iota // 2
)

func main() {
    fmt.Println(azero, aone)
    fmt.Println(bzero, bone)
}
```

0 1 1 2

iota 在conts中相当于数组的索引

# 190. 下面的代码有什么问题，请说明

```go
type data struct {
    name string
}

func (p *data) print() {
    fmt.Println("name:", p.name)
}

type printer interface {
    print()
}

func main() {
    d1 := data{"one"}
    d1.print()

    var in printer = data{"two"}
    in.print()
}
```

值类型的data 没有实现接口 printer的方法。知识点：接口。

# 191. 函数执行时，如果由于 panic 导致了异常，则延迟函数不会执行。这一说法是否正确

false

由 panic 引发异常以后，程序停止执行，然后调用延迟函数（defer），就像程序正常退出一样。

# 192. 下面代码输出什么

```go
func main() {
    a := [3]int{0, 1, 2}
    s := a[1:2] // len(1) cap(2)

    s[0] = 11  
    s = append(s, 12) // s的cap为2，与a共享下标
    s = append(s, 13)
    s[0] = 21

    fmt.Println(a) // [0 11 12]
    fmt.Println(s) // [21 12 13]
}
```

Output

```go
[0 11 12]
[21 12 13]
```

# 193. 下面这段代码输出什么？请简要说明

```go
func main() {
    fmt.Println(strings.TrimRight("ABBA", "BA"))
}
```

结果为""

strings.TrimRight的作用是把有包含第二个参数的组合项的对应字母都替换掉

# 194. 下面代码输出什么？

```go
func main() {
    var src, dst []int
    src = []int{1, 2, 3}
    copy(dst, src) 
    fmt.Println(dst)
}
```

输出结果为[]

copy函数实际上会返回一个int值，这个int是一个size，计算逻辑为size = min(len(dst), len(src))，这个size的大小

dst声明了，但是没有进行初始化，所以dst的len是0，因此实际没有从src上copy到任何元素给dst
