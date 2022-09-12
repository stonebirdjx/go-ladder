<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [:point_right:前言](#point_right%E5%89%8D%E8%A8%80)
- [:point_right:注释](#point_right%E6%B3%A8%E9%87%8A)
  - [:point_right:文件头注释 - 版权信息](#point_right%E6%96%87%E4%BB%B6%E5%A4%B4%E6%B3%A8%E9%87%8A---%E7%89%88%E6%9D%83%E4%BF%A1%E6%81%AF)
  - [:point_right:包注释](#point_right%E5%8C%85%E6%B3%A8%E9%87%8A)
  - [代码注释](#%E4%BB%A3%E7%A0%81%E6%B3%A8%E9%87%8A)
    - [:point_right:外部引用的标识符都应该有注释](#point_right%E5%A4%96%E9%83%A8%E5%BC%95%E7%94%A8%E7%9A%84%E6%A0%87%E8%AF%86%E7%AC%A6%E9%83%BD%E5%BA%94%E8%AF%A5%E6%9C%89%E6%B3%A8%E9%87%8A)
    - [:point_right:注释位于上边或右边](#point_right%E6%B3%A8%E9%87%8A%E4%BD%8D%E4%BA%8E%E4%B8%8A%E8%BE%B9%E6%88%96%E5%8F%B3%E8%BE%B9)
    - [:point_right:避免无用的垃圾注释](#point_right%E9%81%BF%E5%85%8D%E6%97%A0%E7%94%A8%E7%9A%84%E5%9E%83%E5%9C%BE%E6%B3%A8%E9%87%8A)
    - [:point_right:正式代码不应该包含TODO/FIXME注释](#point_right%E6%AD%A3%E5%BC%8F%E4%BB%A3%E7%A0%81%E4%B8%8D%E5%BA%94%E8%AF%A5%E5%8C%85%E5%90%ABtodofixme%E6%B3%A8%E9%87%8A)
- [:point_right:命名](#point_right%E5%91%BD%E5%90%8D)
  - [:point_right:目录名](#point_right%E7%9B%AE%E5%BD%95%E5%90%8D)
  - [:point_right:包名](#point_right%E5%8C%85%E5%90%8D)
  - [:point_right:包名要和所在目录名一致](#point_right%E5%8C%85%E5%90%8D%E8%A6%81%E5%92%8C%E6%89%80%E5%9C%A8%E7%9B%AE%E5%BD%95%E5%90%8D%E4%B8%80%E8%87%B4)
  - [:point_right:文件名](#point_right%E6%96%87%E4%BB%B6%E5%90%8D)
  - [:point_right:避免使用go内置标识符](#point_right%E9%81%BF%E5%85%8D%E4%BD%BF%E7%94%A8go%E5%86%85%E7%BD%AE%E6%A0%87%E8%AF%86%E7%AC%A6)
  - [:point_right:避免使用包名作为前缀](#point_right%E9%81%BF%E5%85%8D%E4%BD%BF%E7%94%A8%E5%8C%85%E5%90%8D%E4%BD%9C%E4%B8%BA%E5%89%8D%E7%BC%80)
  - [:point_right:方法接受者应该简洁](#point_right%E6%96%B9%E6%B3%95%E6%8E%A5%E5%8F%97%E8%80%85%E5%BA%94%E8%AF%A5%E7%AE%80%E6%B4%81)
  - [:point_right:导出错误的变量采用ErrName的格式](#point_right%E5%AF%BC%E5%87%BA%E9%94%99%E8%AF%AF%E7%9A%84%E5%8F%98%E9%87%8F%E9%87%87%E7%94%A8errname%E7%9A%84%E6%A0%BC%E5%BC%8F)
  - [:point_right:专有名词的命名](#point_right%E4%B8%93%E6%9C%89%E5%90%8D%E8%AF%8D%E7%9A%84%E5%91%BD%E5%90%8D)
  - [:point_right:函数名应当是动词或动词短语](#point_right%E5%87%BD%E6%95%B0%E5%90%8D%E5%BA%94%E5%BD%93%E6%98%AF%E5%8A%A8%E8%AF%8D%E6%88%96%E5%8A%A8%E8%AF%8D%E7%9F%AD%E8%AF%AD)
  - [:point_right:结构体名应该是名词或名词短语](#point_right%E7%BB%93%E6%9E%84%E4%BD%93%E5%90%8D%E5%BA%94%E8%AF%A5%E6%98%AF%E5%90%8D%E8%AF%8D%E6%88%96%E5%90%8D%E8%AF%8D%E7%9F%AD%E8%AF%AD)
  - [:point_right:使用业界统一缩略语](#point_right%E4%BD%BF%E7%94%A8%E4%B8%9A%E7%95%8C%E7%BB%9F%E4%B8%80%E7%BC%A9%E7%95%A5%E8%AF%AD)
- [代码格式](#%E4%BB%A3%E7%A0%81%E6%A0%BC%E5%BC%8F)
  - [:point_right:使用工具对代码格式化](#point_right%E4%BD%BF%E7%94%A8%E5%B7%A5%E5%85%B7%E5%AF%B9%E4%BB%A3%E7%A0%81%E6%A0%BC%E5%BC%8F%E5%8C%96)
  - [:point_right:行宽不超过120个字符](#point_right%E8%A1%8C%E5%AE%BD%E4%B8%8D%E8%B6%85%E8%BF%87120%E4%B8%AA%E5%AD%97%E7%AC%A6)
  - [:point_right:相对独立的程序块或者语句之间用空行分割](#point_right%E7%9B%B8%E5%AF%B9%E7%8B%AC%E7%AB%8B%E7%9A%84%E7%A8%8B%E5%BA%8F%E5%9D%97%E6%88%96%E8%80%85%E8%AF%AD%E5%8F%A5%E4%B9%8B%E9%97%B4%E7%94%A8%E7%A9%BA%E8%A1%8C%E5%88%86%E5%89%B2)
  - [:point_right:控制表达式括号](#point_right%E6%8E%A7%E5%88%B6%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%8B%AC%E5%8F%B7)
- [声明和初始化](#%E5%A3%B0%E6%98%8E%E5%92%8C%E5%88%9D%E5%A7%8B%E5%8C%96)
  - [:point_right:保证作用域尽量的小](#point_right%E4%BF%9D%E8%AF%81%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%B0%BD%E9%87%8F%E7%9A%84%E5%B0%8F)
  - [:point_right:禁止使用魔鬼值](#point_right%E7%A6%81%E6%AD%A2%E4%BD%BF%E7%94%A8%E9%AD%94%E9%AC%BC%E5%80%BC)
  - [:point_right:避免直接使用全局变量](#point_right%E9%81%BF%E5%85%8D%E7%9B%B4%E6%8E%A5%E4%BD%BF%E7%94%A8%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F)
  - [:point_right:变量使用时才声明并初始化](#point_right%E5%8F%98%E9%87%8F%E4%BD%BF%E7%94%A8%E6%97%B6%E6%89%8D%E5%A3%B0%E6%98%8E%E5%B9%B6%E5%88%9D%E5%A7%8B%E5%8C%96)
  - [:point_right:相关申明放在一个组内](#point_right%E7%9B%B8%E5%85%B3%E7%94%B3%E6%98%8E%E6%94%BE%E5%9C%A8%E4%B8%80%E4%B8%AA%E7%BB%84%E5%86%85)
  - [初始化结构体变量时，尽可能采用复合字面量的方式](#%E5%88%9D%E5%A7%8B%E5%8C%96%E7%BB%93%E6%9E%84%E4%BD%93%E5%8F%98%E9%87%8F%E6%97%B6%E5%B0%BD%E5%8F%AF%E8%83%BD%E9%87%87%E7%94%A8%E5%A4%8D%E5%90%88%E5%AD%97%E9%9D%A2%E9%87%8F%E7%9A%84%E6%96%B9%E5%BC%8F)
- [:point_right:整数安全](#point_right%E6%95%B4%E6%95%B0%E5%AE%89%E5%85%A8)
  - [:point_right:确保无符合整数运算时不会回绕](#point_right%E7%A1%AE%E4%BF%9D%E6%97%A0%E7%AC%A6%E5%90%88%E6%95%B4%E6%95%B0%E8%BF%90%E7%AE%97%E6%97%B6%E4%B8%8D%E4%BC%9A%E5%9B%9E%E7%BB%95)
  - [:point_right:确保有符合整数运算时不会出现溢出](#point_right%E7%A1%AE%E4%BF%9D%E6%9C%89%E7%AC%A6%E5%90%88%E6%95%B4%E6%95%B0%E8%BF%90%E7%AE%97%E6%97%B6%E4%B8%8D%E4%BC%9A%E5%87%BA%E7%8E%B0%E6%BA%A2%E5%87%BA)
  - [:point_right:确保参与移位的操作数的位数足够](#point_right%E7%A1%AE%E4%BF%9D%E5%8F%82%E4%B8%8E%E7%A7%BB%E4%BD%8D%E7%9A%84%E6%93%8D%E4%BD%9C%E6%95%B0%E7%9A%84%E4%BD%8D%E6%95%B0%E8%B6%B3%E5%A4%9F)
  - [:point_right:确保除法运算和模运算中的除数不为0](#point_right%E7%A1%AE%E4%BF%9D%E9%99%A4%E6%B3%95%E8%BF%90%E7%AE%97%E5%92%8C%E6%A8%A1%E8%BF%90%E7%AE%97%E4%B8%AD%E7%9A%84%E9%99%A4%E6%95%B0%E4%B8%8D%E4%B8%BA0)
  - [:point_right:确保整数转换不会造成数据截断或者符号错误](#point_right%E7%A1%AE%E4%BF%9D%E6%95%B4%E6%95%B0%E8%BD%AC%E6%8D%A2%E4%B8%8D%E4%BC%9A%E9%80%A0%E6%88%90%E6%95%B0%E6%8D%AE%E6%88%AA%E6%96%AD%E6%88%96%E8%80%85%E7%AC%A6%E5%8F%B7%E9%94%99%E8%AF%AF)
- [字符串安全](#%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%AE%89%E5%85%A8)
  - [需要字符串转义时优先使用raw string](#%E9%9C%80%E8%A6%81%E5%AD%97%E7%AC%A6%E4%B8%B2%E8%BD%AC%E4%B9%89%E6%97%B6%E4%BC%98%E5%85%88%E4%BD%BF%E7%94%A8raw-string)
- [数组和slice安全](#%E6%95%B0%E7%BB%84%E5%92%8Cslice%E5%AE%89%E5%85%A8)
  - [避免使用短声明定义一个空slice](#%E9%81%BF%E5%85%8D%E4%BD%BF%E7%94%A8%E7%9F%AD%E5%A3%B0%E6%98%8E%E5%AE%9A%E4%B9%89%E4%B8%80%E4%B8%AA%E7%A9%BAslice)
  - [:point_right:始终使用len(s)==0检查slice是否为空](#point_right%E5%A7%8B%E7%BB%88%E4%BD%BF%E7%94%A8lens0%E6%A3%80%E6%9F%A5slice%E6%98%AF%E5%90%A6%E4%B8%BA%E7%A9%BA)
  - [:point_right:取值时长度校验](#point_right%E5%8F%96%E5%80%BC%E6%97%B6%E9%95%BF%E5%BA%A6%E6%A0%A1%E9%AA%8C)
  - [:point_right:不使用slice作为函数入参](#point_right%E4%B8%8D%E4%BD%BF%E7%94%A8slice%E4%BD%9C%E4%B8%BA%E5%87%BD%E6%95%B0%E5%85%A5%E5%8F%82)
- [map 安全](#map-%E5%AE%89%E5%85%A8)
  - [:point_right:确保key读取到map的元素有效](#point_right%E7%A1%AE%E4%BF%9Dkey%E8%AF%BB%E5%8F%96%E5%88%B0map%E7%9A%84%E5%85%83%E7%B4%A0%E6%9C%89%E6%95%88)
- [表达式安全](#%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%AE%89%E5%85%A8)
  - [:point_right:表达式的比较，应该遵循左变量右值的原则](#point_right%E8%A1%A8%E8%BE%BE%E5%BC%8F%E7%9A%84%E6%AF%94%E8%BE%83%E5%BA%94%E8%AF%A5%E9%81%B5%E5%BE%AA%E5%B7%A6%E5%8F%98%E9%87%8F%E5%8F%B3%E5%80%BC%E7%9A%84%E5%8E%9F%E5%88%99)
- [:point_right:iota](#point_rightiota)
- [控制语句安全](#%E6%8E%A7%E5%88%B6%E8%AF%AD%E5%8F%A5%E5%AE%89%E5%85%A8)
  - [:point_right:删除不必要的else](#point_right%E5%88%A0%E9%99%A4%E4%B8%8D%E5%BF%85%E8%A6%81%E7%9A%84else)
  - [:point_right:尽量保持正常代码路径为最小缩进](#point_right%E5%B0%BD%E9%87%8F%E4%BF%9D%E6%8C%81%E6%AD%A3%E5%B8%B8%E4%BB%A3%E7%A0%81%E8%B7%AF%E5%BE%84%E4%B8%BA%E6%9C%80%E5%B0%8F%E7%BC%A9%E8%BF%9B)
  - [:point_right:如果只用到`range`中的索引部分，则去除冗余的元素](#point_right%E5%A6%82%E6%9E%9C%E5%8F%AA%E7%94%A8%E5%88%B0range%E4%B8%AD%E7%9A%84%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86%E5%88%99%E5%8E%BB%E9%99%A4%E5%86%97%E4%BD%99%E7%9A%84%E5%85%83%E7%B4%A0)
  - [:point_right:如果不使用`range`中的任何部分，则不要赋值给匿名变量](#point_right%E5%A6%82%E6%9E%9C%E4%B8%8D%E4%BD%BF%E7%94%A8range%E4%B8%AD%E7%9A%84%E4%BB%BB%E4%BD%95%E9%83%A8%E5%88%86%E5%88%99%E4%B8%8D%E8%A6%81%E8%B5%8B%E5%80%BC%E7%BB%99%E5%8C%BF%E5%90%8D%E5%8F%98%E9%87%8F)
  - [:point_right:循环必须有显示的退出条件](#point_right%E5%BE%AA%E7%8E%AF%E5%BF%85%E9%A1%BB%E6%9C%89%E6%98%BE%E7%A4%BA%E7%9A%84%E9%80%80%E5%87%BA%E6%9D%A1%E4%BB%B6)
  - [:point_right:避免在循环体中修改循环控制变量](#point_right%E9%81%BF%E5%85%8D%E5%9C%A8%E5%BE%AA%E7%8E%AF%E4%BD%93%E4%B8%AD%E4%BF%AE%E6%94%B9%E5%BE%AA%E7%8E%AF%E6%8E%A7%E5%88%B6%E5%8F%98%E9%87%8F)
  - [:point_right:慎用goto,goto只能向下跳转](#point_right%E6%85%8E%E7%94%A8gotogoto%E5%8F%AA%E8%83%BD%E5%90%91%E4%B8%8B%E8%B7%B3%E8%BD%AC)
  - [:point_right:Switch要有default分支](#point_rightswitch%E8%A6%81%E6%9C%89default%E5%88%86%E6%94%AF)
  - [:point_right:禁止使用浮点数作为循环计数器](#point_right%E7%A6%81%E6%AD%A2%E4%BD%BF%E7%94%A8%E6%B5%AE%E7%82%B9%E6%95%B0%E4%BD%9C%E4%B8%BA%E5%BE%AA%E7%8E%AF%E8%AE%A1%E6%95%B0%E5%99%A8)
  - [:point_right:不要在迭代集合数据结构的过程中修改或者删除元素](#point_right%E4%B8%8D%E8%A6%81%E5%9C%A8%E8%BF%AD%E4%BB%A3%E9%9B%86%E5%90%88%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E7%9A%84%E8%BF%87%E7%A8%8B%E4%B8%AD%E4%BF%AE%E6%94%B9%E6%88%96%E8%80%85%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
- [函数和方法](#%E5%87%BD%E6%95%B0%E5%92%8C%E6%96%B9%E6%B3%95)
  - [合理设计](#%E5%90%88%E7%90%86%E8%AE%BE%E8%AE%A1)
    - [:point_right:合理选择方法的接受者](#point_right%E5%90%88%E7%90%86%E9%80%89%E6%8B%A9%E6%96%B9%E6%B3%95%E7%9A%84%E6%8E%A5%E5%8F%97%E8%80%85)
    - [:point_right:函数功能要单一](#point_right%E5%87%BD%E6%95%B0%E5%8A%9F%E8%83%BD%E8%A6%81%E5%8D%95%E4%B8%80)
    - [:point_right:避免使用具名返回值](#point_right%E9%81%BF%E5%85%8D%E4%BD%BF%E7%94%A8%E5%85%B7%E5%90%8D%E8%BF%94%E5%9B%9E%E5%80%BC)
    - [:point_right:返回已被修改大小的slice对象](#point_right%E8%BF%94%E5%9B%9E%E5%B7%B2%E8%A2%AB%E4%BF%AE%E6%94%B9%E5%A4%A7%E5%B0%8F%E7%9A%84slice%E5%AF%B9%E8%B1%A1)
    - [:point_right:避免栈调用层次太深](#point_right%E9%81%BF%E5%85%8D%E6%A0%88%E8%B0%83%E7%94%A8%E5%B1%82%E6%AC%A1%E5%A4%AA%E6%B7%B1)
    - [:point_right:方法参数当传递的值为较大的结构体时，应传递指针，避免底层复制](#point_right%E6%96%B9%E6%B3%95%E5%8F%82%E6%95%B0%E5%BD%93%E4%BC%A0%E9%80%92%E7%9A%84%E5%80%BC%E4%B8%BA%E8%BE%83%E5%A4%A7%E7%9A%84%E7%BB%93%E6%9E%84%E4%BD%93%E6%97%B6%E5%BA%94%E4%BC%A0%E9%80%92%E6%8C%87%E9%92%88%E9%81%BF%E5%85%8D%E5%BA%95%E5%B1%82%E5%A4%8D%E5%88%B6)
  - [init函数](#init%E5%87%BD%E6%95%B0)
    - [:point_right:一个文件只定义一个init函数](#point_right%E4%B8%80%E4%B8%AA%E6%96%87%E4%BB%B6%E5%8F%AA%E5%AE%9A%E4%B9%89%E4%B8%80%E4%B8%AAinit%E5%87%BD%E6%95%B0)
    - [:point_right:一个包如果有多个init函数，不要存在任何依赖关系](#point_right%E4%B8%80%E4%B8%AA%E5%8C%85%E5%A6%82%E6%9E%9C%E6%9C%89%E5%A4%9A%E4%B8%AAinit%E5%87%BD%E6%95%B0%E4%B8%8D%E8%A6%81%E5%AD%98%E5%9C%A8%E4%BB%BB%E4%BD%95%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB)
    - [:point_right:注意init函数内进行初始化的场景限制](#point_right%E6%B3%A8%E6%84%8Finit%E5%87%BD%E6%95%B0%E5%86%85%E8%BF%9B%E8%A1%8C%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E5%9C%BA%E6%99%AF%E9%99%90%E5%88%B6)
  - [闭包](#%E9%97%AD%E5%8C%85)
    - [:point_right:禁止在闭包中直接使用循环控制变量](#point_right%E7%A6%81%E6%AD%A2%E5%9C%A8%E9%97%AD%E5%8C%85%E4%B8%AD%E7%9B%B4%E6%8E%A5%E4%BD%BF%E7%94%A8%E5%BE%AA%E7%8E%AF%E6%8E%A7%E5%88%B6%E5%8F%98%E9%87%8F)
- [结构体](#%E7%BB%93%E6%9E%84%E4%BD%93)
  - [:point_right:合理使用匿名嵌套](#point_right%E5%90%88%E7%90%86%E4%BD%BF%E7%94%A8%E5%8C%BF%E5%90%8D%E5%B5%8C%E5%A5%97)
- [接口](#%E6%8E%A5%E5%8F%A3)
  - [:point_right:避免接口过大](#point_right%E9%81%BF%E5%85%8D%E6%8E%A5%E5%8F%A3%E8%BF%87%E5%A4%A7)
  - [:point_right:从使用者角度设计接口](#point_right%E4%BB%8E%E4%BD%BF%E7%94%A8%E8%80%85%E8%A7%92%E5%BA%A6%E8%AE%BE%E8%AE%A1%E6%8E%A5%E5%8F%A3)
  - [:point_right:避免类型可能不为空的接口变量和nil直接进行比较](#point_right%E9%81%BF%E5%85%8D%E7%B1%BB%E5%9E%8B%E5%8F%AF%E8%83%BD%E4%B8%8D%E4%B8%BA%E7%A9%BA%E7%9A%84%E6%8E%A5%E5%8F%A3%E5%8F%98%E9%87%8F%E5%92%8Cnil%E7%9B%B4%E6%8E%A5%E8%BF%9B%E8%A1%8C%E6%AF%94%E8%BE%83)
- [类型断言](#%E7%B1%BB%E5%9E%8B%E6%96%AD%E8%A8%80)
  - [:point_right:必须处理类型断言的失败](#point_right%E5%BF%85%E9%A1%BB%E5%A4%84%E7%90%86%E7%B1%BB%E5%9E%8B%E6%96%AD%E8%A8%80%E7%9A%84%E5%A4%B1%E8%B4%A5)
- [包安全](#%E5%8C%85%E5%AE%89%E5%85%A8)
  - [:point_right:使用internal目录避免内部公开的Api对外暴露](#point_right%E4%BD%BF%E7%94%A8internal%E7%9B%AE%E5%BD%95%E9%81%BF%E5%85%8D%E5%86%85%E9%83%A8%E5%85%AC%E5%BC%80%E7%9A%84api%E5%AF%B9%E5%A4%96%E6%9A%B4%E9%9C%B2)
  - [:point_right:禁止使用 `.` 来简化导入包](#point_right%E7%A6%81%E6%AD%A2%E4%BD%BF%E7%94%A8--%E6%9D%A5%E7%AE%80%E5%8C%96%E5%AF%BC%E5%85%A5%E5%8C%85)
  - [:point_right:禁止使用相对路径导包](#point_right%E7%A6%81%E6%AD%A2%E4%BD%BF%E7%94%A8%E7%9B%B8%E5%AF%B9%E8%B7%AF%E5%BE%84%E5%AF%BC%E5%8C%85)
  - [:point_right:导入包的顺序按稳定度排序](#point_right%E5%AF%BC%E5%85%A5%E5%8C%85%E7%9A%84%E9%A1%BA%E5%BA%8F%E6%8C%89%E7%A8%B3%E5%AE%9A%E5%BA%A6%E6%8E%92%E5%BA%8F)
  - [:point_right:导入包时名称未冲突的情况下应避免使用别名](#point_right%E5%AF%BC%E5%85%A5%E5%8C%85%E6%97%B6%E5%90%8D%E7%A7%B0%E6%9C%AA%E5%86%B2%E7%AA%81%E7%9A%84%E6%83%85%E5%86%B5%E4%B8%8B%E5%BA%94%E9%81%BF%E5%85%8D%E4%BD%BF%E7%94%A8%E5%88%AB%E5%90%8D)
- [错误处理](#%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86)
  - [:point_right:使用恰当的错误处理机制](#point_right%E4%BD%BF%E7%94%A8%E6%81%B0%E5%BD%93%E7%9A%84%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86%E6%9C%BA%E5%88%B6)
  - [:point_right:确保正确处理函数的错误返回值](#point_right%E7%A1%AE%E4%BF%9D%E6%AD%A3%E7%A1%AE%E5%A4%84%E7%90%86%E5%87%BD%E6%95%B0%E7%9A%84%E9%94%99%E8%AF%AF%E8%BF%94%E5%9B%9E%E5%80%BC)
  - [:point_right:错误信息不应该大写，或者以标点结尾。](#point_right%E9%94%99%E8%AF%AF%E4%BF%A1%E6%81%AF%E4%B8%8D%E5%BA%94%E8%AF%A5%E5%A4%A7%E5%86%99%E6%88%96%E8%80%85%E4%BB%A5%E6%A0%87%E7%82%B9%E7%BB%93%E5%B0%BE)
- [异常处理](#%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86)
  - [:point_right:包外可见的函数禁止向外抛出panic](#point_right%E5%8C%85%E5%A4%96%E5%8F%AF%E8%A7%81%E7%9A%84%E5%87%BD%E6%95%B0%E7%A6%81%E6%AD%A2%E5%90%91%E5%A4%96%E6%8A%9B%E5%87%BApanic)
  - [:point_right:确保发生异常时程序尝试恢复到合理的状态并记录日志](#point_right%E7%A1%AE%E4%BF%9D%E5%8F%91%E7%94%9F%E5%BC%82%E5%B8%B8%E6%97%B6%E7%A8%8B%E5%BA%8F%E5%B0%9D%E8%AF%95%E6%81%A2%E5%A4%8D%E5%88%B0%E5%90%88%E7%90%86%E7%9A%84%E7%8A%B6%E6%80%81%E5%B9%B6%E8%AE%B0%E5%BD%95%E6%97%A5%E5%BF%97)
- [并发](#%E5%B9%B6%E5%8F%91)
  - [:point_right:避免数据竞争](#point_right%E9%81%BF%E5%85%8D%E6%95%B0%E6%8D%AE%E7%AB%9E%E4%BA%89)
  - [:point_right:避免goroutine被永久阻塞](#point_right%E9%81%BF%E5%85%8Dgoroutine%E8%A2%AB%E6%B0%B8%E4%B9%85%E9%98%BB%E5%A1%9E)
  - [:point_right:chan类型作函数参数时限定类型](#point_rightchan%E7%B1%BB%E5%9E%8B%E4%BD%9C%E5%87%BD%E6%95%B0%E5%8F%82%E6%95%B0%E6%97%B6%E9%99%90%E5%AE%9A%E7%B1%BB%E5%9E%8B)
  - [:point_right:使用chan时确保对chan的操作有效](#point_right%E4%BD%BF%E7%94%A8chan%E6%97%B6%E7%A1%AE%E4%BF%9D%E5%AF%B9chan%E7%9A%84%E6%93%8D%E4%BD%9C%E6%9C%89%E6%95%88)
  - [:point_right:禁止拷贝锁和同步的对象](#point_right%E7%A6%81%E6%AD%A2%E6%8B%B7%E8%B4%9D%E9%94%81%E5%92%8C%E5%90%8C%E6%AD%A5%E7%9A%84%E5%AF%B9%E8%B1%A1)
- [输入输出](#%E8%BE%93%E5%85%A5%E8%BE%93%E5%87%BA)
  - [:point_right:临时文件使用完必须及时删除](#point_right%E4%B8%B4%E6%97%B6%E6%96%87%E4%BB%B6%E4%BD%BF%E7%94%A8%E5%AE%8C%E5%BF%85%E9%A1%BB%E5%8F%8A%E6%97%B6%E5%88%A0%E9%99%A4)
  - [:point_right:创建文件时必须显示指定合适的文件访问权限](#point_right%E5%88%9B%E5%BB%BA%E6%96%87%E4%BB%B6%E6%97%B6%E5%BF%85%E9%A1%BB%E6%98%BE%E7%A4%BA%E6%8C%87%E5%AE%9A%E5%90%88%E9%80%82%E7%9A%84%E6%96%87%E4%BB%B6%E8%AE%BF%E9%97%AE%E6%9D%83%E9%99%90)
  - [:point_right:确保检验文件路径前对其标准化](#point_right%E7%A1%AE%E4%BF%9D%E6%A3%80%E9%AA%8C%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84%E5%89%8D%E5%AF%B9%E5%85%B6%E6%A0%87%E5%87%86%E5%8C%96)
  - [确保安全地从压缩包提取文件](#%E7%A1%AE%E4%BF%9D%E5%AE%89%E5%85%A8%E5%9C%B0%E4%BB%8E%E5%8E%8B%E7%BC%A9%E5%8C%85%E6%8F%90%E5%8F%96%E6%96%87%E4%BB%B6)
  - [:point_right:禁止直接使用外部数据构造格式化字符串](#point_right%E7%A6%81%E6%AD%A2%E7%9B%B4%E6%8E%A5%E4%BD%BF%E7%94%A8%E5%A4%96%E9%83%A8%E6%95%B0%E6%8D%AE%E6%9E%84%E9%80%A0%E6%A0%BC%E5%BC%8F%E5%8C%96%E5%AD%97%E7%AC%A6%E4%B8%B2)
  - [:point_right:禁止在日志中保存敏感数据](#point_right%E7%A6%81%E6%AD%A2%E5%9C%A8%E6%97%A5%E5%BF%97%E4%B8%AD%E4%BF%9D%E5%AD%98%E6%95%8F%E6%84%9F%E6%95%B0%E6%8D%AE)
- [外部数据校验](#%E5%A4%96%E9%83%A8%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C)
  - [:point_right:对所有外部数据进行合法性校验](#point_right%E5%AF%B9%E6%89%80%E6%9C%89%E5%A4%96%E9%83%A8%E6%95%B0%E6%8D%AE%E8%BF%9B%E8%A1%8C%E5%90%88%E6%B3%95%E6%80%A7%E6%A0%A1%E9%AA%8C)
  - [:point_right:禁止直接使用外部数据拼接sql语句](#point_right%E7%A6%81%E6%AD%A2%E7%9B%B4%E6%8E%A5%E4%BD%BF%E7%94%A8%E5%A4%96%E9%83%A8%E6%95%B0%E6%8D%AE%E6%8B%BC%E6%8E%A5sql%E8%AF%AD%E5%8F%A5)
  - [:point_right:禁止直接使用外部数据拼接命令参数](#point_right%E7%A6%81%E6%AD%A2%E7%9B%B4%E6%8E%A5%E4%BD%BF%E7%94%A8%E5%A4%96%E9%83%A8%E6%95%B0%E6%8D%AE%E6%8B%BC%E6%8E%A5%E5%91%BD%E4%BB%A4%E5%8F%82%E6%95%B0)
- [资源管理](#%E8%B5%84%E6%BA%90%E7%AE%A1%E7%90%86)
  - [:point_right:在性能敏感的情况下建议采用池化技术优化资源使用](#point_right%E5%9C%A8%E6%80%A7%E8%83%BD%E6%95%8F%E6%84%9F%E7%9A%84%E6%83%85%E5%86%B5%E4%B8%8B%E5%BB%BA%E8%AE%AE%E9%87%87%E7%94%A8%E6%B1%A0%E5%8C%96%E6%8A%80%E6%9C%AF%E4%BC%98%E5%8C%96%E8%B5%84%E6%BA%90%E4%BD%BF%E7%94%A8)
  - [:point_right:避免资源泄漏](#point_right%E9%81%BF%E5%85%8D%E8%B5%84%E6%BA%90%E6%B3%84%E6%BC%8F)
  - [:point_right:合理使用defer](#point_right%E5%90%88%E7%90%86%E4%BD%BF%E7%94%A8defer)
  - [:point_right:禁止重复释放资源](#point_right%E7%A6%81%E6%AD%A2%E9%87%8D%E5%A4%8D%E9%87%8A%E6%94%BE%E8%B5%84%E6%BA%90)
  - [:point_right:make申请的slice、map时，根据预估或校验范围大小来申请合适的内存。](#point_rightmake%E7%94%B3%E8%AF%B7%E7%9A%84slicemap%E6%97%B6%E6%A0%B9%E6%8D%AE%E9%A2%84%E4%BC%B0%E6%88%96%E6%A0%A1%E9%AA%8C%E8%8C%83%E5%9B%B4%E5%A4%A7%E5%B0%8F%E6%9D%A5%E7%94%B3%E8%AF%B7%E5%90%88%E9%80%82%E7%9A%84%E5%86%85%E5%AD%98)
  - [:point_right:避免过多的time.After函数调用导致消耗大量的资源](#point_right%E9%81%BF%E5%85%8D%E8%BF%87%E5%A4%9A%E7%9A%84timeafter%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8%E5%AF%BC%E8%87%B4%E6%B6%88%E8%80%97%E5%A4%A7%E9%87%8F%E7%9A%84%E8%B5%84%E6%BA%90)
- [内存访问](#%E5%86%85%E5%AD%98%E8%AE%BF%E9%97%AE)
  - [:point_right:禁止解引用空指针](#point_right%E7%A6%81%E6%AD%A2%E8%A7%A3%E5%BC%95%E7%94%A8%E7%A9%BA%E6%8C%87%E9%92%88)
  - [:point_right:禁止解引用空的接口和内置引用类型变量](#point_right%E7%A6%81%E6%AD%A2%E8%A7%A3%E5%BC%95%E7%94%A8%E7%A9%BA%E7%9A%84%E6%8E%A5%E5%8F%A3%E5%92%8C%E5%86%85%E7%BD%AE%E5%BC%95%E7%94%A8%E7%B1%BB%E5%9E%8B%E5%8F%98%E9%87%8F)
  - [:point_right:使用[]byte而不是string存储敏感数据、敏感数据使用完后应立即清0](#point_right%E4%BD%BF%E7%94%A8byte%E8%80%8C%E4%B8%8D%E6%98%AFstring%E5%AD%98%E5%82%A8%E6%95%8F%E6%84%9F%E6%95%B0%E6%8D%AE%E6%95%8F%E6%84%9F%E6%95%B0%E6%8D%AE%E4%BD%BF%E7%94%A8%E5%AE%8C%E5%90%8E%E5%BA%94%E7%AB%8B%E5%8D%B3%E6%B8%850)
  - [:point_right:避免使用unsafe包](#point_right%E9%81%BF%E5%85%8D%E4%BD%BF%E7%94%A8unsafe%E5%8C%85)
- [cgo](#cgo)
  - [最小化使用cgo范围](#%E6%9C%80%E5%B0%8F%E5%8C%96%E4%BD%BF%E7%94%A8cgo%E8%8C%83%E5%9B%B4)
  - [使用C虚拟包辅助函数C.CString和C.CBytes返回的类型转换参数使用完后需要手动释放](#%E4%BD%BF%E7%94%A8c%E8%99%9A%E6%8B%9F%E5%8C%85%E8%BE%85%E5%8A%A9%E5%87%BD%E6%95%B0ccstring%E5%92%8Cccbytes%E8%BF%94%E5%9B%9E%E7%9A%84%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2%E5%8F%82%E6%95%B0%E4%BD%BF%E7%94%A8%E5%AE%8C%E5%90%8E%E9%9C%80%E8%A6%81%E6%89%8B%E5%8A%A8%E9%87%8A%E6%94%BE)
  - [避免将Go内存指针直接传入C函数使用](#%E9%81%BF%E5%85%8D%E5%B0%86go%E5%86%85%E5%AD%98%E6%8C%87%E9%92%88%E7%9B%B4%E6%8E%A5%E4%BC%A0%E5%85%A5c%E5%87%BD%E6%95%B0%E4%BD%BF%E7%94%A8)
  - [避免使用C代码接管系统的信号](#%E9%81%BF%E5%85%8D%E4%BD%BF%E7%94%A8c%E4%BB%A3%E7%A0%81%E6%8E%A5%E7%AE%A1%E7%B3%BB%E7%BB%9F%E7%9A%84%E4%BF%A1%E5%8F%B7)
  - [避免直接将C代码嵌入在Go文件中](#%E9%81%BF%E5%85%8D%E7%9B%B4%E6%8E%A5%E5%B0%86c%E4%BB%A3%E7%A0%81%E5%B5%8C%E5%85%A5%E5%9C%A8go%E6%96%87%E4%BB%B6%E4%B8%AD)
  - [避免在CGO中使用C代码调用Go函数](#%E9%81%BF%E5%85%8D%E5%9C%A8cgo%E4%B8%AD%E4%BD%BF%E7%94%A8c%E4%BB%A3%E7%A0%81%E8%B0%83%E7%94%A8go%E5%87%BD%E6%95%B0)
- [系统接口](#%E7%B3%BB%E7%BB%9F%E6%8E%A5%E5%8F%A3)
  - [:point_right:命令执行检查](#point_right%E5%91%BD%E4%BB%A4%E6%89%A7%E8%A1%8C%E6%A3%80%E6%9F%A5)
- [Web跨域](#web%E8%B7%A8%E5%9F%9F)
  - [:point_right:跨域资源共享CORS限制请求来源](#point_right%E8%B7%A8%E5%9F%9F%E8%B5%84%E6%BA%90%E5%85%B1%E4%BA%ABcors%E9%99%90%E5%88%B6%E8%AF%B7%E6%B1%82%E6%9D%A5%E6%BA%90)
- [响应输出](#%E5%93%8D%E5%BA%94%E8%BE%93%E5%87%BA)
  - [:point_right:设置正确的HTTP响应包类型](#point_right%E8%AE%BE%E7%BD%AE%E6%AD%A3%E7%A1%AE%E7%9A%84http%E5%93%8D%E5%BA%94%E5%8C%85%E7%B1%BB%E5%9E%8B)
  - [添加安全响应头](#%E6%B7%BB%E5%8A%A0%E5%AE%89%E5%85%A8%E5%93%8D%E5%BA%94%E5%A4%B4)
  - [外部输入拼接到HTTP响应头中需进行过滤](#%E5%A4%96%E9%83%A8%E8%BE%93%E5%85%A5%E6%8B%BC%E6%8E%A5%E5%88%B0http%E5%93%8D%E5%BA%94%E5%A4%B4%E4%B8%AD%E9%9C%80%E8%BF%9B%E8%A1%8C%E8%BF%87%E6%BB%A4)
  - [外部输入拼接到response页面前进行编码处理](#%E5%A4%96%E9%83%A8%E8%BE%93%E5%85%A5%E6%8B%BC%E6%8E%A5%E5%88%B0response%E9%A1%B5%E9%9D%A2%E5%89%8D%E8%BF%9B%E8%A1%8C%E7%BC%96%E7%A0%81%E5%A4%84%E7%90%86)
- [会话管理](#%E4%BC%9A%E8%AF%9D%E7%AE%A1%E7%90%86)
  - [:point_right:安全维护session信息](#point_right%E5%AE%89%E5%85%A8%E7%BB%B4%E6%8A%A4session%E4%BF%A1%E6%81%AF)
  - [CSRF防护](#csrf%E9%98%B2%E6%8A%A4)
- [访问控制](#%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)
  - [默认鉴权](#%E9%BB%98%E8%AE%A4%E9%89%B4%E6%9D%83)
- [其他](#%E5%85%B6%E4%BB%96)
  - [:point_right:禁止使用math/rand包提供的函数产生安全用途的伪随机数](#point_right%E7%A6%81%E6%AD%A2%E4%BD%BF%E7%94%A8mathrand%E5%8C%85%E6%8F%90%E4%BE%9B%E7%9A%84%E5%87%BD%E6%95%B0%E4%BA%A7%E7%94%9F%E5%AE%89%E5%85%A8%E7%94%A8%E9%80%94%E7%9A%84%E4%BC%AA%E9%9A%8F%E6%9C%BA%E6%95%B0)
  - [禁止代码中包含公网地址](#%E7%A6%81%E6%AD%A2%E4%BB%A3%E7%A0%81%E4%B8%AD%E5%8C%85%E5%90%AB%E5%85%AC%E7%BD%91%E5%9C%B0%E5%9D%80)
  - [删除无效或者永不执行的代码](#%E5%88%A0%E9%99%A4%E6%97%A0%E6%95%88%E6%88%96%E8%80%85%E6%B0%B8%E4%B8%8D%E6%89%A7%E8%A1%8C%E7%9A%84%E4%BB%A3%E7%A0%81)
  - [:point_right:不用代码直接删除掉，不要注释掉](#point_right%E4%B8%8D%E7%94%A8%E4%BB%A3%E7%A0%81%E7%9B%B4%E6%8E%A5%E5%88%A0%E9%99%A4%E6%8E%89%E4%B8%8D%E8%A6%81%E6%B3%A8%E9%87%8A%E6%8E%89)
  - [避免调用os.Exit()和runtime.Goexit()退出函数](#%E9%81%BF%E5%85%8D%E8%B0%83%E7%94%A8osexit%E5%92%8Cruntimegoexit%E9%80%80%E5%87%BA%E5%87%BD%E6%95%B0)
- [参考资料](#%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# :point_right:前言

文档为个人平时生活收集和工作积累，不涉及商用，仅供学习和参考。请尊重信息、尊重开发者、勿在商业模式下传播。

软件设计原则：*可读性、可维护、安全、可靠、可测试、高效、可移植*。

:point_right:提高代码质量，希望大家好好学习，天天向上

# :point_right:注释

`//` 单行注释， ` /* */`多行注释, **通常建议采用 // Comments 注释**

```go
// 单行注释
/* 单行注释 */

/*
第一行注释
第二行注释
第三行注释
*/
```

- 注释和代码一样重要、应该按需注释，修改与维护
- 注释符合注释内容之间要保留一个空格
- 注释建议使用英文撰写

## :point_right:文件头注释 - 版权信息

文件头部首先应该包含版权说明。如果需要添加其他内容，可以在版权说明下面补充。

如文件功能说明、作者、日期、注意事项等。

**正例**

```go
// Copyright (c) 2018 hu. All rights reserved.
// Personal code, only for learning and communication.
// You can contact me in the following ways:
//    stonebirdjx <1245863260@qq.com, g1245863260@gmail.com>
//    https://www.hjxstbserver.xyz/
// File: hurl.go  2020/6/18 10:55.
// Desc:
```

## :point_right:包注释

包注释紧贴package语句之上，且**只在一个文件中加入**，内容较少的可以放在与包名同名的文件中，内容较多的放在`doc.go`文件中。

**正例**

```go
/*
Package zip provides support for reading and writing ZIP archives.
*/
package zip
```

以`Package name `开头，以`.` 结尾，包注释内容可在`godoc`上显示

## 代码注释

### :point_right:外部引用的标识符都应该有注释

所有导出符号的代码必须有注释

**正例**

```go
// FindByName find some info by enter the name.
func FindByName(name string) string{
    ...
}
```

### :point_right:注释位于上边或右边

如果注释位于单独一行，则需要以大写开头（缩略词或变量名除外）并以`.`结尾。

**正例**

```PHP
if l.flag&(Lshortfile|Llongfile) != 0 {
    // Release lock while getting caller info - it's expensive.
    l.mu.Unlock()
    var ok bool
    _, file, line, ok = runtime.Caller(calldepth)
    if !ok {
        file = "???"
        line = 0
    }
    l.mu.Lock()
}
```

如果位于语句后方，则不要以大写开头，且结尾不需要加入标点符号。

**正例**

```go
const Ldate = 1 << iota // the date in the local time zone: 2009/01/23
```

### :point_right:避免无用的垃圾注释

避免简单的注释，重复、无内容的注释

**反例**

```go
var age = 18 // definition age eq 18

// WriteData 
// name:WriteData  	重复
// args：		   没内容
// return：		   没内容
func WriteData(buf []byte,length int)(int error){}
```

### :point_right:正式代码不应该包含TODO/FIXME注释

**反例**

```go
// TODO: todo somethings
// FIXME: fixed some bugs
```

# :point_right:命名

使用统一的风格命名：大驼峰和小驼峰

| 类别                                   | 导出使用 | 包内使用 |
| -------------------------------------- | -------- | -------- |
| 结构体、接口、自定义别名和类型         | 大驼峰   | 小驼峰   |
| 函数、方法                             | 大驼峰   | 小驼峰   |
| 结构体成员变量                         | 大驼峰   | 小驼峰   |
| 常量                                   | 大驼峰   | 小驼峰   |
| 全局变量                               | 大驼峰   | 小驼峰   |
| 方法接受者、函数参数、返回值、局部变量 |          | 小驼峰   |
| break、continue、goto的标签            |          | 小驼峰   |

> *包内使用小驼峰、外部引用大驼峰*

## :point_right:目录名

目录名采用全小写的方式、允许包含数字、中横线 `-`（头尾不能是中横线）。

**正例**

```bash
hurl-cli
md5
```

**反例**

```bash
stone_bird
StoneBird
```

## :point_right:包名

原则如下：

- 只由小写字母组成。不包含大写字母和下划线等字符（测试包除外）。
- 简短并包含一定的上下文信息。例如`time`、`list`、`http`等。
- 不能是含义模糊的常用名，或者与标准库同名。例如不能使用`util`或者`strings`。
- 包名能够作为路径的 base name，在一些必要的时候，需要把不同功能拆分为子包。例如应该使用`encoding/base64`而不是`encoding_base64`或者`encodingbase64`。

**正例**

```go
package sha256
package sha256_test
```

**反例**

```go
package stone_bird
```

## :point_right:包名要和所在目录名一致

如果不一致，会给阅读带来困难，难以理解。

**正例**

```bash
目录名：context
对应的包名：context
目录名：port-obj
对应的包名：portobj
```

**反例**

```bash
目录名：Context
目录名：port_obj
```

## :point_right:文件名

文件名全部采用小写单词、允许包含数字和下划线（`_`）组合方式。

**正例**

```bash
sha256.go
http_cli.go
stone.pb.go  // 特殊例子protobuf 格式
```

## :point_right:避免使用go内置标识符

**反例**

```go
var string string = "stone bird"
```

## :point_right:避免使用包名作为前缀

定义类型函数等，应避免包名的上下文信息

**反例**

```go
// stream.go
package stream

type StreamBuffer struct{ 
    //不符合，var buf = stream.StreamBuffer过于累赘
}
```

**正例**

```go
package stream

type Buffer struct{ 
    // 符合 var buf = stream.Buffer 层次清晰
}
```

## :point_right:方法接受者应该简洁

接受者名称应当简洁（1-2个字母即可）

**正例**

```go
func (c *Controller) send(c <-chan struct{})
```

不要使用me、self、this这种泛指的名称，这会让你看起来不像go程序员

**反例**

```go
func (self *Controller) send(c <-chan struct{})
```

## :point_right:导出错误的变量采用ErrName的格式

**正例**

```go
type ReadError struct {
    ...
}

var ErrReading = errors.New("read err")
```

> *错误类型：使用NameError*
>
> *错误变量：ErrName*

## :point_right:专有名词的命名

应该全部大写或者全部小写

"ACL","API","ARPU","ASCII","BO","CPA","CPC","CPM","CPU","CSS","CPP","CPS","CPT","CQRS","CTR","CVR","DNS","DAU","DO","DSP","DTO","EOF","GUID","HTML","HTTP","HTTPS","ID","IO","IP","JSON","KPI","LBS","LHS","MAU","OKR","PC","PV","PO","POJO","QPS","RAM","RHS","RPC","SEM","SEO","SLA","SMTP","SNS","SPAM","SOHO","SQL","SSH","TCP","TLS","TTL","UDP","UGC","UED","UI","UID","UUID","URI","URL","UTF8","UV","VM","VO","VR","XML","XMPP","XSRF","XSS"

**正例**

```bash
urlArray, URLArray, userid  userID, UserID,  CPUInfo, cpuInfo
```

**反例**

```bash
UrlArray, UserId, CpuInfo
```

## :point_right:函数名应当是动词或动词短语

**正例**

```bash
postPayment、DeletePage、SaveOrder
```

## :point_right:结构体名应该是名词或名词短语

**正例**

```bash
Customer、WikiPage、Account
```

## :point_right:使用业界统一缩略语

并和业界常用的缩略语保持一致，例如

**正例**

```bash
temp => tmp
flag => flg
statistic => stat
increment => inc
message => msg
buffer => buf
error => err
argument => arg
parameters => params
initialize => init
index => idx
context => ctx
clear => clr
pointer => ptr
previous => prev
request => req
response => resp|rsp
maximum => max
minimum => min
result => ret/res
```

# 代码格式

## :point_right:使用工具对代码格式化

使用 `gofmt` 对代码进行格式化，会将GO源码按标准风格进行缩进、对齐、保留注释等。

使用 `goimports`，与`gofmt`拥有相同格式化功能，还能自动删除和引用包。

可以用`goland`的工具格式化代码。

```bash
find ./ *.go | grep -v vendor | xargs gofmt -w
```

## :point_right:行宽不超过120个字符

> 除非超过能带来显著的上下文信息。

## :point_right:相对独立的程序块或者语句之间用空行分割

减少空行使用，一般一个空行即可，最多两个空行。

**正例**

```go
func first(){}

func second(){}
```

**反例**

```go
err := do()

if err != nil {
 // 不是相对独立的代码，不应该用空行隔开
}
```

## :point_right:控制表达式括号

- *对于if、for、switch控制表达式，不使用()*
- *对于复杂表达式，整体最外层不使用()；里面单个简单表达式使用()明确优先级*

**正例**

```bash
if n > 2 {
}

switch n {
case n > 2:
}

if (a > 2) && (c > 3) {
}
```

**反例**

```go
if (n > 2) {
}

switch (n) {
case (n > 2):
}

if ((a > 2) & (c > 3)) {
}

if a > 2 & c > 3 {
}
```

# 声明和初始化

## :point_right:保证作用域尽量的小

作用域的变量、函数、类型等在程序内可引用的越大，引起错误的可能性就越高。

不能导出的常量、变量、函数、或闭包只在包内使用

最小化作用域的方法：

> ​	*1、尽量推迟变量定义，在变量使用时才能被初始化*
>
> ​	*2、把相关语句放在一起或单独提取到子函数，是函数尽量小而集中，功能单一*

**正例**

```go
for i := 0; i < 10; i++ {
    ...
}
```

**反例**

```go
// 不符合，循环之后不需要访问i。而且i容易被篡改 
i := 0 
for ; i < 10; i++ {
	...
}
```

## :point_right:禁止使用魔鬼值

不要使用难以理解的字面量，禁止使用魔鬼字符串、魔鬼数字等

**正例**

```Go
const Deleted 1
const XyzType 100
if xxx == XyzType {
  ...
}
return Deleted,nil

```

**反例**

```go
if xxx == 100 {
  ...
}
return 1001

// 下面的数字字面量，不容易理解其具体含义
switch curType {
	case 1:
	case 2:
	case 3:
	default:		
}
```

## :point_right:避免直接使用全局变量

禁止将全局变量导出，使用全局变量会导致业务代码和全局变量间产生耦合，并且难以追踪数据的变化。应尽量避免使用全局变量。

包内使用全局变量，应该对其进行封装，并注意避免因并发而出现的数据竞争

**反例**

```go
// 不符合，导出了全局变量,容易被他出修改
var SumValue = 0 
```

**正例**

```go
// 当前模块确保数据不存在竞争
var total = 0 

func GetTotal() int {
	return total
}
```

## :point_right:变量使用时才声明并初始化

遵循变量作用域最小原则与就近声明原则，在变量使用时才声明并初始化，使得代码更容易阅读。

**正例**

```go
var name = "stone bird"
```

**反例**

```go
var name string
...
name = "stone bird"
```

## :point_right:相关申明放在一个组内

相同用途的常量或变量应该放在一个常量组或变量组。相关性高的放置在同一个组里面，以提升代码的可读性。

**正例**

```go
type Duration int

const (
	Nanosecond Duration = 1
	MicroSecond  = 1000 * Nanosecond
	...
)
```

## 初始化结构体变量时，尽可能采用复合字面量的方式

初始化结构体变量时，优先采用复合字面量的方式，并指定字段名称，以提升代码可读性。应该尽量用每一行一个`字段名：初始化值`的形式。

> *数组和slice的成员是结构体的情况除外。*

**反例**

```go
type stoneBird struct {
	name string
	age int
	language string
}

sb := new(stoneBird)
sb.name = "stone"
sb.age = 18
sb.language = "go"
```

**正例**

```go
func New() stoneBird {
	return stoneBird {
		name:     "stone",
		age:      18,
		language: "go",
	}
}
```

**例外**

```go
sbs := []stoneBird {
    {"stone",18,"go"},
    {"stone",19,"go"},
    {"stone",20,"go"},
}
```

# :point_right:整数安全

以下场景必须严格进行长度限制：

- 作为数组索引
- 作为对象的长度或者大小
- 作为数组的边界（如作为循环计数器）

## :point_right:确保无符合整数运算时不会回绕

所谓回绕是指无法用无符号整数表示运算结果。这个结果会根据该类型可以表示的最大值加1执行求模操作。

> +、-、*、++、--、+=、-=、<<=、<<

**反例**

```go
func add(a, b uint64) {
	c := a + b // 如果 a + b > math.MaxUnit64 会存在无符号整数回绕（从0开始计算）
}
```

**正例**

```go
// 加法 判断是否大于 math.MaxUint64，其他类型类推
func add(a, b uint64) (uint64, error) {
	if a > math.MaxUint64-b {
		return 0,errors.New("lager num")
	}
	return a + b, nil
}

// 减法 判断前者是否小于后者
func sub(a, b uint64) (uint64, error) {
	if a < b {
		return 0, errors.New("回绕")
	}
	return a - b, nil
}
```

## :point_right:确保有符合整数运算时不会出现溢出

**整数溢出**是一种未定义的行为，意味着编译器处理器有符合整数溢出时具有很多选择，有符合整数溢出会发生在如下操作中

> +、-、*、/、++、--、+=、-=、/=、%=、<<=、<<

后续代码如以此作为分配内存的大小，将导致申请的内存比实际所需的过小、或者过大，从而导致内存不足的问题。

**反例**

```go
func add(a, b int32) {
	c := a + b // a + b > math.MaxInt32 时两者相加会产生有符合整数溢出
}
```

**正例**

```go
// 正例,需要先计算会不会溢出（从负值开始计算）
func doInt(a, b int32) {
	var c int32
	if (b > 0 && a > (math.MaxInt32-b)) ||
		(b < 0 && a < (math.MaxInt32-b)){
		// 错误处理
	}
	c = a + b
}
```

## :point_right:确保参与移位的操作数的位数足够

移位的位数应该是无符号整数，go对无类型的常数依照参与表达式运算数据类型进行推导

**反例**

```go
func foo(num uint16,bit uint8) bool {
	if num > (1 << bit) { // 不符合：这里（1 << bit）被推导成了uint16，可被移位操作回绕
		return true
	}
	return false
}
```

**正例**

```go
func foo(num uint16,bit uint8) bool {
	if uint32(num) > (uint32(1) << bit) { // 强制类型转换后，应满足函数设计要求
		return true
	}
	return false
}
```

## :point_right:确保除法运算和模运算中的除数不为0

> 运行时会报错

**正例**

```go
func div(total,a int) bool {
	if a == 0 {
		return false
	}
	avg = total / a
}
```

## :point_right:确保整数转换不会造成数据截断或者符号错误

当一个较大类型转换为较小类型时，并且该数原值超出较小整型的表示范围，就会发生截断错误。

有符号整型向无符号整型转换时，最高位会丧失作为符号位的功能

**反例**

```go
var a = math.MaxInt32
b := int16(a) // 不符合，不同类型强制转化数据会发生数据截断

var a = math.MinInt32
b := uint32(a) // 不符合，产生符号丢失
```

**正例**

```go
func int32216(a int32){
	if (a < math.MinInt16) || (a > math.MaxInt16) {
    	// 错误处理
	}
    int16(a)
}

func int2uint(a int32){
    if a < 0 {
    	// 错误处理
	}
    b := uint32(a)
}
```

- 带符号整数值为非负时，向无符号整型转换后，值不变
- *带符号整数值为负数时，向无符号整型转换后，结果通常是一个非常大的整数*

# 字符串安全

## 需要字符串转义时优先使用raw string

**反例**

```go
const prefix = "name=\"stonebird\""
```

**正例**

```go
const prefix = `name="stonebird"`
```

# 数组和slice安全

## 避免使用短声明定义一个空slice

使用var 关键字时定义空slice更清晰，并且不容易出错。

**反例**

```go
filter = []int{} // 不符合：避免短声明定义一个空的slice
```

**正例**

```go
var filter []int
```

## :point_right:始终使用len(s)==0检查slice是否为空

要检查slice是否为空（没有元素），必须始终检查其长度是否为0，

长度为0的slice不一定是nil 如 `filter := []int{}`,当如果slice ==nil长度度一定为0.

**反例**

```go
func isEmpty(s []string) bool {
    return  s == nil // 不符合：申请了空间的slice长度为0，不是nil 如[]int{}
}
```

**正例**

```go
func isEmpty(s []string) bool {
    return  len(s) == 0 
}
```

## :point_right:取值时长度校验

在对slice进行操作时，必须判断长度是否合法，防止程序panic

**反例**

```go
func foo() {
	var slice = []int{0, 1, 2, 3, 4, 5, 6}
	fmt.Println(slice[:10])
}
```

**正例**

```go
func decode(data []byte) bool {
	if len(data) == 6 {
		fmt.Println(data[5])
	}
	return false
}
```

## :point_right:不使用slice作为函数入参

slice在作为函数入参时，函数内对slice的修改可能会影响原始数据

**反例**

```go
func modify(array []int) {
    array[0] = 10 // 对入参slice的元素修改会影响原始数据
}
  
func main() {
	array := []int{1, 2, 3, 4, 5}
  
    modify(array)
    fmt.Println(array) // output：[10 2 3 4 5]
}
```

**正例**

```go
// 数组作为函数入参，而不是slice
func modify(array [5]int) {
    array[0] = 10
}

func main() {
    // 传入数组，注意数组与slice的区别
    array := [5]int{1, 2, 3, 4, 5}

    modify(array)
    fmt.Println(array)
}
```

# map 安全

## :point_right:确保key读取到map的元素有效

key如果不存在，会返回零值。使用if ok ，判断key是否存在

**正例**

```go
if v, ok := mp["key"]; ok {
    ...
}
```

# 表达式安全

## :point_right:表达式的比较，应该遵循左变量右值的原则

当变量或方法调用与常量比较时，如果常量在左边，不符合阅读习惯，难以理解

**反例**

```go
if nil != p && Max > p.v {
    ...
}
```

**正例**

```go
if p != nil && p.v > Max {
     ...
}
```

**例外**

```go
if v > min && v < max{
    ...
}

if min < v && v < max{
    ...
}
```

# :point_right:iota

`iota`只用于定义系统内部常量，**不建议**在业务代码中使用。

枚举类型需要在`const`组中声明，被赋值的变量应该使用自定义类型。

每个`iota`只在某个`const`组中自动枚举，如果定义在新的`const`组中，每次都会从`0`重新开始。

**正例**

```Go
const (
   signaturePKCS1v15 uint8 = iota + 225
   signatureRSAPSS
   signatureECDSA
   signatureEd25519
)
//...
type ClientAuthType int

const (
   NoClientCert ClientAuthType = iota
   RequestClientCert
   RequireAnyClientCert
   VerifyClientCertIfGiven
   RequireAndVerifyClientCert
)
```

# 控制语句安全

## :point_right:删除不必要的else

如果两个分支中都包含`return`语句，则可以去除冗余的`else`。能不要else的时候就不用else

**反例**

```Go
if foo {
   return x
} else {
   return nil
}
```

**正例**

```go
if foo {
   return x
}
return nil
```

## :point_right:尽量保持正常代码路径为最小缩进

优先处理并缩进错误分支，增加可读性。

**反例**

```go
func aFunc() error {
    err := doSomething()
    if err == nil {
        err := doAnotherThing()
        if err == nil {
            return nil // 正常代码
        }
        return err
    }
    return err
}
```

**正例**

```go
func aFunc() error {
    if err := doSomething(); err != nil {
        return err    
    }
    if err := doAnotherThing(); err != nil {
        return err    
    }
    return nil // 正常代码
}
```

## :point_right:如果只用到`range`中的索引部分，则去除冗余的元素

**反例**

```go
for i, _ := range s {
   print(i)
}
```

**正例**

```go
for i := range s {
   print(i)
}
```

## :point_right:如果不使用`range`中的任何部分，则不要赋值给匿名变量

**反例**

```go
for _ = range s {
   ...
}
```

**正例**

```go
for range s {
   // ...
}
```

## :point_right:循环必须有显示的退出条件

程序无法正常结束是一种缺陷，特别是它无法响应外部对他的控制。如果循环没有退出条件，循环在任何或预计时间内将完不成执行，导致死循环。

应尽量避免使用空for，如果无法避免，需要在循环过程中设置能退出的条件，并使用break退出。

**反例**

```go
for {
    ... // 不符合，没有显示的退出条件
}
```

**正例**

```go
for {
    if condition {
        break
    }
}
```

## :point_right:避免在循环体中修改循环控制变量

如果在循环体内修改循环控制参数的数值，可能导致死循环，或者循环次数达不到预期

**反例**

```go
for i := 0 ; i < 10 ; i++ {
    i -= 1
}
```

**正例**

```go
for i := 0 ; i < 10 ; i++ {
    ...
}
```

## :point_right:慎用goto,goto只能向下跳转

goto语句会破坏程序的结构性，非必要不使用goto，使用goto时，也只允许跳转到本函数内的goto语句之后的标签（向下跳转）。

**反例**

```go
unsafe:
if done {
    goto unsafe // 不符合：goto往上跳转标签
}
```

**正例**

```go
if done{
    goto label
}
label:
```

## :point_right:Switch要有default分支

每个switch，都应该包含一个default分支，即使default分支，没有业务逻辑代码。

> 枚举类型除外

**正例**

```go
switch str {
case "a":
default:
}
```

**例外**

```go
switch color {
case red:
case green:   
case blue:
}
```

## :point_right:禁止使用浮点数作为循环计数器

存储浮点数的精度有限，精度问题可能导致条件判断不准确，导致循环达不到预期。

**反例**

```go
for i := float32(2000000001);i < float32(2000000002);i++ {
    ... // 2e+9 不会进入循环
}
```

## :point_right:不要在迭代集合数据结构的过程中修改或者删除元素

使用 `for range`或 `for` 循环可以便捷的访问map和slice中的元素。

- 对slice是修改不会影响循环次数，但会影响结果。
- 对map的key的增加或删除会影响循环次数。

**反例**

```go
func main() {
	a := []int{0, 2, 1, 3, 5}
	for _, val := range a { // 不符合，在迭代原有的slice时，同事删除了索引为2的元素。
		fmt.Println(val)
		if len(a) > 4{
			a = append(a[0:2],a[3:]...)
		}
	}
}
// output 0 2 3 5 5

for k , v := range mp {
	mp["answer"] = 10 // 不符合：增加了key会影响对mp的变量
}
```

# 函数和方法

## 合理设计

### :point_right:合理选择方法的接受者

合理选择方法接受者的类型，对代码的可读性，和维护性都有不同的影响。

原则如下：

- 如果方法不会改变接受者的内容，使用值类型，表明方法的const定义。
- 如果接收者是大型结构体或者数组，可考虑指针类型，提高效率。
- 如果接受者包含sync.Mutex或类似同步字段的结构体，则必须使用指针，以免被复制。
- 如果接收者是map、func、或者chan，不要使用指针类型。
- 如果方法接收者是slice，且方法中不会修改接受者容量，或者重新分配内存，不要使用指针类型。

### :point_right:函数功能要单一

函数功能要单一，过长的函数往往意味着函数功能不单一。可以进一步拆分或分层。而且过于复杂的函数往往不利于阅读，难以维护。

原则如下：

- 函数行数，建议不超过50行（非空非注释）
- 函数的参数个数，建议不超过5个，多了建议用结构体替代
- 最大代码块嵌套深度，建议不超过4层。
- 函数的返回值不超过3个。

### :point_right:避免使用具名返回值

优先使用返回值，而且错误值和返回值不要复用一个返回值。

**反例**

```go
func newInt(i int) (i int) {  // 不符合，通过传入的指针来修改外部的实参值
	i++
}
```

**正例**

```go
func newInt(i int) int {
	return i++
}
```

### :point_right:返回已被修改大小的slice对象

slice 的append 操作有可能修改slice指向的地址，如果函数被设计用于更新传入的slice参数，则他的实现需要满足：

如函数包含的slice的参数（不是slice指针），并对其append操作，必须返回修改后的slice。

**反例**

```go
func appendSlice(s []int){ // 不符合：修改了slice大小，因此必须返回slice
    s[0] = 10 // 符合：只是修改内容没有修改大小
    s = append(s,11) // 不符合：修改了大小，如函数不返回s则不符合规则。
}
```

**正例**

```go
func appendSlice(s []int) []int { 
    s[0] = 10 
    s2 = append(s,11)
	return s2
}
```

### :point_right:避免栈调用层次太深

调用层次太深，在栈上分配大量内存，容易导致出现栈溢出错误。GO语音有一些容易让函数调用陷入无限递归的情况，需要避免。

- 指定嵌入类型成员调用嵌入类型的方法。
- 确保String()方法不会被无限递归。

**反例**

```go
func (j *job) Printf(str string) {
    j.Printf(str string) // 不符合：直接调用j.Printf会导致无限递归调用
}
```

**正例**

```go
func (j *job) Printf(str string) {
    j.Logger.Printf(str string) 
}
```

### :point_right:方法参数当传递的值为较大的结构体时，应传递指针，避免底层复制

参数是map，slice，chan不要传递指针，因为map，slice，chan是引用类型，当不需要修改map，slice内容时不要传递指针的指针。

## init函数

> 尽量减少init函数使用，一般每个包只有一个

### :point_right:一个文件只定义一个init函数

一个文件只定义一个init函数，同时把init函数尽量放在源文件靠前的位置，这样有助于代码阅读。

### :point_right:一个包如果有多个init函数，不要存在任何依赖关系

不要在多个init函数之间存在依赖变量或资源的初始化顺序。

### :point_right:注意init函数内进行初始化的场景限制

由于init函数没有返回值，如果init函数初始化失败，只能通过异常反馈错误。因此应该慎用init函数进行复杂的初始化操作。

注意如下：

- 避免在init函数中进行资源申请、打开文件、连接数据库等可能失败的操作。
- 避免在init函数中调用可能抛出异常的函数。

## 闭包

### :point_right:禁止在闭包中直接使用循环控制变量

**反例**

```go
for i := 0 ;i < 5 ; i++ {
    wg.Add(1)
    go func (){ // 不符合：避免在闭包中直接使用i
        defer wg.Done()
        fmt.Println(i)
    }()
}
// output  55555
```

**正例**

```go
// 正例
for i := 0 ;i < 5 ; i++ {
    wg.Add(1)
    go func (j int) {
        defer wg.Done()
        fmt.Println(j)
    }(i)
}
```

# 结构体

## :point_right:合理使用匿名嵌套

**反例**

```go
type MyStack struct {
    Mylist // 不符合：此匿名嵌入暴露了内部的实现
}
```

**正例**

```go
// 正例
type MyStack struct {
    list Mylist // 符合：不使用匿名嵌套，使得嵌入实现的方法不会被暴露出去
}
```

# 接口

## :point_right:避免接口过大

golang推荐使用组合的方式来写程序。go开发者一般会嵌入其他已有接口类型的方式来构建新的接口。

小结构体的主要优势：

- 接口越小，抽象程度越高，越彻底，被人接受的程度越好，其实接口的实现和现实中情况是一样的，最小的接口就是`interface{}`
- 小接口有较少的方法，一般仅仅一个方法。要实现这个接口，开发者仅仅实现一个或少数几个方法就可以了。单元测试尤为重要，可以付出较少的成本来验证你的程序是否有bug。
- 职责单一，容易组合。

可以参考io标准库的设计

## :point_right:从使用者角度设计接口

应该从使用者而不是实现者的角度设计接口，实现者应返回具体类型。

一般来说 interface 不能在实现的地方（同一个文件里）定义，应该单独的定义在某个位置

**正例**

```go
type Producer interface {DoSomething()} 

// 其他位置实现
package impl

type MyProducer struct{...}

func (mp MyProducer)DoSomething(){...}
func NewProducer() MyProducer {
    return MyProducer{}
} 
```

## :point_right:避免类型可能不为空的接口变量和nil直接进行比较

在go语言底层，interface定义的变量有两个成员变量，一个类型和一个值，只有当两个都为nil的情况下的这个接口变量才是nil。

如果某个具体的对象赋值给接口变量，无论赋值的对象是不是nil，这个接口变量都不可能是nil，因此不存在在nil值赋值给接口变量。

- 确保赋值给接口变量的对象的指针有效，否则应赋予无类型的nil值
- 必须使用error作为函数的错误返回值类型，对非错误必须返回无类型的nil。

**正例**

```go
if err := do() ;err != nil {
    return err
}
return nil
```

# 类型断言

## :point_right:必须处理类型断言的失败

类型断言可以判断接口变量封装的是否是期望类型；也可以判断接口变量封装的类型是否实现了目标接口。在对接口变量进行类型断言时，必须判断断言是否成功，如果失败仍执行会panic。

类型断言使用场景有两种：if ok 和 switch，switch分支使用default分支来处理其他情况

**正例**

```go
if v ,ok := a.(string) ; !ok {
	// 错误处理
}

switch t := t.(type) {
case int:
case bool: 
default: // 必须使用默认分支来处理失败的情况
}
```

# 包安全

## :point_right:使用internal目录避免内部公开的Api对外暴露

在设计项目或模块业务代码时，应该合理的使用internal目录，把内部公开的代码放在internal中，以提供对外合理的api

比如 example/a/internal只能被example/a开始的包导入，不能被example/e、 example/f等其他包导入。

## :point_right:禁止使用 `.` 来简化导入包

可以直接使用导入包里东西，这种写法不利于阅读

**反例**

```go
import . "example/a"  // 不符合：使用了 . 来简化导入包
```

**例外**
在测试文件_test.go中，为了避免循环依赖时方可使用

## :point_right:禁止使用相对路径导包

**反例**

```go
import "../net" 
```

## :point_right:导入包的顺序按稳定度排序

建议导入的顺序是 ：标准库、第三方库、本项目的库，不同类型之间用空行隔开

## :point_right:导入包时名称未冲突的情况下应避免使用别名

**反例**

```go
import （
	rtace "runtime/trace" // 不符合：未冲突，尽量避免使用别名
）
```

**正例**

```go
import （
	rtace "runtime/trace" 
	xtrace "golang.net/x/trace"
）
```

# 错误处理

##  :point_right:使用恰当的错误处理机制

错误处理机制有:

- `in-band`错误提示：通过返回的内容，区别是正常值还是错误值
- 使用异常panic机制
- 使用error接口的方式返回错误值 （推荐使用）

##  :point_right:确保正确处理函数的错误返回值

任何时候都不要忽略返回的error类型

主函数需要关闭文件之类的除外

**反例**

```go
b, _ : = get() // v ,err
b.bar() // 未正常处理错误，b可能为nil
```

**正例**

```go
if b,err := get() ;err != nil {
    b.bar()
}
```

## :point_right:错误信息不应该大写，或者以标点结尾。

错误字符串要求：除专有名词或者缩写词外，避免字符串以大写字面开头，或以标点结尾

**反例**

```go
fmt.Errorf("Something bad.") // 不符合，上下文拼接后，字符串中间会有大写字面开头的场景
```

**正例**

```go
fmt.Errorf("something good")
```

# 异常处理

## :point_right:包外可见的函数禁止向外抛出panic

包外可见的函数禁止向外抛出panic，如果导出函数本身或者调用其他函数可能抛出panic，需要内部recover并转换成error返回给外包调用者。

**正例**

```go
func justDo() (err any) {
	defer func() {
		err = recover()
	}()
    panic("err")
	return err
}
```

## :point_right:确保发生异常时程序尝试恢复到合理的状态并记录日志

当函数/方法发生异常时，一般对象需要恢复到原来状态，安全关键的对象必须恢复到原来的状态。保持对象一致性常用手段包括：

- *输入校验，如校验方法的参数*
- *调整逻辑顺序，使可能发生异常的代码在对象修改之前执行。*
- *对业务操作失败时回滚*
- *对临时副本对象操作，直到操作完成，才把更新提交到原始对象上。*
- *编码修改对象状态*

# 并发

## :point_right:避免数据竞争

map 、slice不是并发安全的，所以在并发下共享数据访问存在数据竞争（不单指map和slice）。并发时优先推荐消息传递数据，而不是共享内存。

go1.9 之后可以用sync.Map满足并发安全的map需求.

可以使用go 的race工具分析代码是否存在数据竞争。

```go
go test -race pkgname
go run -race src.go
go build -race cmds
go install -race pkgname
```

## :point_right:避免goroutine被永久阻塞

避免goroutine永久阻塞导致内存泄漏；goroutine永久阻塞一般是由于锁或者chan未正确使用造成的。

确保select的不会被长时间或者永久阻塞，可添加default分支：

需要遵循以下要求：

- 确保读取chan时，chan存在数据，否则会被阻塞。
- 不向值为nil（var ch chan int）的chan发送或接收数据，否则将导致goroutine永久阻塞
- 避免死锁导致goroutine阻塞。

## :point_right:chan类型作函数参数时限定类型

chan类型作为函数参数传递时限定类型，根据函数创建最新权限得channel，以增加代码的可读性。

**正例**

```go
func readOnly (in <-chan int) {
    ...
}

func writeOnly(out chan<- int){
    ...
}

func readAndWrite(ch chan int){
    ...
}
```

## :point_right:使用chan时确保对chan的操作有效

确保chan使用时已被初始化（使用make申请过空间），禁止对nil和closed的chan进行操作，否则可能导致以下问题

- *关闭closed的channel会产生panic*
- *关闭 nil 的 chan 会产生panic*
- *向closed的chan发送数据会panic*
- *当chan没数据时，取数据会返回零值。必须通过if ok判断是否正常取出数据。*

**正例**

```go
select {
case _, ok := <- ch:
    if !ok {
        return
    }
}
```

## :point_right:禁止拷贝锁和同步的对象

使用锁机制时，应避免死锁。复制锁或同步的对象本身没有同步保护，因此复制结果可能不完整（不是有原值得快照），即使锁是完整的它也会和已有锁存在冲突。

应确保：

- *正确的使用锁，程序中不存在死锁；注意使用相同的顺序请求和释放锁来避免死锁。*
- *确保加锁的代码范围合理；禁止在循环语句中使用defer语句释放锁。禁止多次释放同一个锁*
- *禁止拷贝包含锁或同步的数据结构（一般在sync包里定义了锁或同步的类型结构），只能通过指针或接口等引用类型来传递此类数据结构的引用。*

**反例**

```go
for i := 0 ; i < len(s) ; i++ {
    m.Lock()
    defer m.Unlock() // 不符合：不能在循环中使用defer来释放锁
}

type Counter struct{
   sync.Mutex
   n int64
}

func (c Counter) Value() int64 { 
    ... // 不符合：方法接受者为值类型，当它被调用时，Couner被复制。简介导致Counter中的Mutex被复制
}

```

**正例**

```go
for i := 0 ; i < len(s) ; i++ {
  	func() {
		 m.Lock()
    	 defer m.Unlock()
	}()
}

func (c *Counter) Value() int64 { // 有锁的对象，使用指针传递。 
    ...
}
```

# 输入输出

## :point_right:临时文件使用完必须及时删除

程序员经常会创建临时文件，如在/var/tmp。这样的目录可能会被定期清理。但未被清理前还是可访问的。每个程序都有责任确保删除已使用完毕的临时文件。

## :point_right:创建文件时必须显示指定合适的文件访问权限

多用户的文件系统是通过权限和许可模型来保护文件可访问的。多用户系统中的文件通常归属于某个特定的用户。攻击者可能通过其他用户攻击。因此创建文件时就为其指定访问权限，以防止未授权的文件访问。

最好指定0600，或者0644的权限

 **反例**

```go
f ,err := os.Creat("/tmp/test.txt") // 不符合：没有显示指定文件权限
```

正例

```go
f ,err := os.OpenFile("/tmp/test.txt",os.O_CREATE|OS_WRONLY|OS_TRUNC,0600) 
```

## :point_right:确保检验文件路径前对其标准化

绝对路径名或者相对路径名中可能包含文件链接（软链接、硬链接、快捷方式、影子文件、别名等）。或者包含特殊字符（如.或..），者使得验证文件路径变得困难。如不检验，工具者可以通过路径，达到攻击的目的。

在文件操作之前，必须对文件路径进行验证，对文件路径标准化，会使得验证文件路径更简单安全。

**反例**

```go
func validate(path string) bool {
    pattern := "^/opt/pwm"  // 不符合：未把文件路径标准化，当path为 /opt/pwd/../../etc/passwd时,攻击者可以绕过验证。
    reg := regexp.MustComplie(pattern)
    return reg.MatchString(path)
}
```

**正例**

```go
func validate(path string) bool {
    realPath,err := filepath.Abs(path)
    if err != nil {
        return false
    }
    pattern := "^/opt/pwm"  
    reg := regexp.MustComplie(pattern)
    return reg.MatchString(realPSath)
}
```

## 确保安全地从压缩包提取文件

解压文件时有两个问题需要避免

- 提取文件的标准路径是否落在解压目录之外
- 提取的文件是否消耗过多的系统资源

需要增加文件数量、文件大小、路径标准化等检验操作

## :point_right:禁止直接使用外部数据构造格式化字符串

当攻击者可以直接控制格式化字符串时，可导致信息泄漏、拒绝服务、系统功能异常等风险。

**反例**

```go
func checked(user string) {
    msg := fmt.Sprintf("%s ddd",user)
    fmt.Printf(msg)  // 不符合：msg存在为验证的外部数据，存在格式化漏洞，当指定外部格式为%p，%d会导致格式化出现非预期的结果
}

```

## :point_right:禁止在日志中保存敏感数据

在日中禁止输出口令、秘钥和其他保护信息。口令包括明文口令和密文口令，对于敏感信息采用以下方法：

- 不在日志中打印敏感信息
- 不将敏感信息输出到控制台和串口
- 如因特殊原因必须打印日志，使用固定长度的*代替敏感信息
- 序列化或者输出敏感信息应该先加密后，输出到外部设备或者文件中
- 禁止在错误或异常处理中泄漏敏感信息

# 外部数据校验

## :point_right:对所有外部数据进行合法性校验

编程人员处理外部数据过程中必须时刻保持这种思维意识，不要做任何外部数据符合预期的假设。外部数据必须经过严格判断后才能使用

外部数据来源包括但不限于：网络、用户输入、命令行、文件（包括程序配置文件）、环境变量、用户态数据（对于内核程序）、进程间通信（包括管道、消息、共享内存、socket、RPC等）、API参数、全局变量等

典型场景如下：

- 作为数组索引
- 作为内存偏移地址
- 作为内存分配的尺寸参数
- 作为循环条件
- 作为除数
- 作为命令行参数
- 作为数据库查询语句参数
- 作为输入、输出格式化字符串参数
- 作为文件路径

输入校验包括但不限于：

- API接口参数合法性
- 检验数据长度
- 检验数据范围
- 校验数据类型和格式
- 检验输入只包含可接收的字符（'白名单形式'），尤其是需要注意一些特殊字符

原则：信任边界、外部数据校验

## :point_right:禁止直接使用外部数据拼接sql语句

SQL注入是指外部数据构造的SQL语句所代表的数据库操作与预期不符。这样的操作可能导致信息泄漏或者数据被篡改

防护措施如下：

- 使用参数化查询：最有效的防护手段，但对sql语句中的表名、字段名等不适用
- 对外部数据进行白名单校验：适用于拼接SQL中的表名、字段名
- 对外部数据中SQL注入相关的特殊字符进行转义：使用与拼接sql场景

**反例**

```go
func queryById(id,pwd string){
    // 不符合：直接使用sql拼接
    var sqlStr = "select name from user where id = '"+id+"' and pwd='"+pwd+"'"
}
```

正例

```go
func queryById(id,pwd string){
    // 不符合：直接使用sql拼接
    var sqlStr = "select name from user where id = ? and pwd= ?"
    db.Query(sqlStr,id,pwd)
}
```

## :point_right:禁止直接使用外部数据拼接命令参数

使用未经检验的不可信输入作为系统命令的参数或命令的一部分，可能会导致命令注入漏洞。

需要注意以下：

- 命令执行的字符串不要取拼接输入的参数。如果必须拼接时，需要对输入参数进行白名单过滤。
- 对传入的参数数据要做类型校验，例如整数数据。
- 保证格式化字符串的正确性。例如int类型的参数拼接要用%d，不要用%s。

# 资源管理

## :point_right:在性能敏感的情况下建议采用池化技术优化资源使用

一般而言，重用一个已经创建的对象比创建一个新对象快，除非确实需要重新创建。

go语音申请的变量内存需要gc。gc回收需要额外的开销。大量的内存回收可能会导致GC期间CPU消耗过多。业务处理能力下降。

其原则是尽可能少申明变量：

- 局部变量尽量利于
- 小变量合并，如果小变量过多，可以把这些变量放在一个结构体内。降低扫描和回收时间
- 高并发任务中通过goroutine池，避免调度和GC带来的冲击；goroutine虽然轻量级，但对于高并发的轻量级任务下，频繁创建groutine来执行，效率也非常低。
- 在合理的情况下使用struct{}空结构体。来避免数据值所占用的空间，如：配合map实现set集合。chan传递空消息等。

## :point_right:避免资源泄漏

在程序每个可能走到的流程中，应确保完整的释放文件句柄、DB连接等资源。特别是在异常发生时，若申请的资源（如文件句柄、数据库连接、内存资源）未得到释放，会引起资源异常占用。

实际操作中能够用defer释放的尽量用defer释放。不能强制使用defer

应确保：

- 主程序结束，所有的goroutine默认都会退出。要有安全的通知goroutine结束机制如（context包）
- 主程序没有结束，有些goroutine已经不在使用，要确保及时退出。

**正例**

```go
f,err := os.OpenFile("/test.txt",OS_RDONLY,0644)
if err != nil {
    return
}
defer func(){
    err = f.Close()
    if err != nil {
    	// 异常处理
	}
}()
```

## :point_right:合理使用defer

不要再defer中修改返回值（尤其是具名返回的情况下），使得代码不易维护。

避免在循环中使用defer

推荐使用defer的两种场景

- 资源释放
- 如果代码可能导致pannic，使用defer func recover

**反例**

```go
func tryChange() int {
    var result  int
    defer func() {
       result++ // 不符合：试图修改返回值（实际并不能） 
    }()
}
```

## :point_right:禁止重复释放资源

重复释放一般处于错误的流程判断中。

要求：

- 禁止重复关闭释放的文件、数据库连接、或网络连接等资源。
- 禁止重复释放chan等系统内置类型的对象资源

**反例**

```go
func foo(c chan int) {
    defer close(c)
    for {
        err := get()
        if err != nil {
            c <- 0
            close(c) // 不符合：重复释放channel
        }
    }
}
```

## :point_right:make申请的slice、map时，根据预估或校验范围大小来申请合适的内存。

为避免扩容带来的性能消耗，在初始化slice和map时，根据预估范围来申请合适的容量。

```go
var mp = make(map[string]string 1000)
```

## :point_right:避免过多的time.After函数调用导致消耗大量的资源

如果在某个时间段频繁的多次调用，则可能导致很多未过期的Timer值，从而导致大量的内存和计算消耗。

反例

```go
func longRunning(message <-chan string){
    for {
        select{
            case <-time.After(time.Minute):
            	return // 会一直堆积，造成资源消耗
            case msg := <- message:
             	do()
        }
    }
}
```

**正例**

```go
func longRunning(message <-chan string){
    timer := time.NewTimer(time.Minute)
    for {
        select{
                case <-time.C:
                    return // 会一直堆积，造成资源消耗
                case msg := <- message:
                    do()
                    if !timer.stop(){
                        <-timer.C
                    }
         }
        // 必须重置以复用
        timer.Rest(time.Mintue)
     }
}
```

# 内存访问

## :point_right:禁止解引用空指针

go中，对值类型，声明即默认初始化。对引用类型，申明后其值是空值（nil）。使用引用类型前，必须对其有效的构建并初始化好。

**反例**

```go
// 反例
func main() {
    var s *student
    if command {
        s = &student{}
    }
    s.Foo() // 不符合：s可能为nil
}
```

**正例**

```go
func main() {
    var s *student
    if command {
        s = &student{}
    }
    if s == nil { // nil 直接return
        return
    }
    s.Foo()
}
```

## :point_right:禁止解引用空的接口和内置引用类型变量

对chan、map内置引用类型变量，访问元素前，必须使用make显式的初始化。

对interface类型变量，在解引用或调用变量之前，应判断接口变量不为空。

**反例**

```go
// 不符合：未使用make显式初始化
var mp map[string]*student 
var ch chan int
```

## :point_right:使用[]byte而不是string存储敏感数据、敏感数据使用完后应立即清0

使用[]byte存储敏感数据，可以降低敏感数据泄漏风险

**正例**

```go
buf := make([]byte,1024)
_,err := f.read(buf)
```

## :point_right:避免使用unsafe包

uintptr是一个可容纳指针的指数，表示对象的地址。

使用时应确保：

- *uintptr 值转换unsafe.Pointer时地址有效*
- *解引用unsafe.Pointer时不存在内存越界访问*

# cgo

## 最小化使用cgo范围

cgo调用开销非常大，需要谨慎使用，如果能用纯go解决的就不要用cgo

## 使用C虚拟包辅助函数C.CString和C.CBytes返回的类型转换参数使用完后需要手动释放

**正例**

```go
// #include "stdio.h"
import "C"
import "unsafe"

func main() {
    cs := C.CString("hello")
    defer C.free(unsafe.Pointer(cs)) //需要手动释放
    C.puts(sc)
}
```

## 避免将Go内存指针直接传入C函数使用

CGO时go语音和c语音实现互通。在需要将go的字符串传入C语音时，先通过C.CString将go语音的字符串对应的数据复制到C语音的内存空间上。确保安全的使用，但会导致效率下降

**正例**

```go
package main
/*
#include "stdio.h"
#include "stdlib.h"

void printString(const char* s){
	printf("%s",s)
}
*/
import "C"
import "unsafe"

func main() {
    cs := C.CString("hello")
    defer C.free(unsafe.Pointer(cs)) //需要手动释放
    C.printString(sc)
}
```

## 避免使用C代码接管系统的信号

应尽量避免C代码接管系统的信号。signal应该go的信号处理来接管

## 避免直接将C代码嵌入在Go文件中

直接经C代码嵌套在go中，可读性差。应该将C代码打包程静态库/动态库的方式调用。

## 避免在CGO中使用C代码调用Go函数

防止下面情形

- 被C函数调用的go函数中再调用C函数
- 被Go函数调用的C函数中再调用Go函数

# 系统接口

## :point_right:命令执行检查

- 使用`exec.Command`、`exec.CommandContext`、`syscall.StartProcess`、`os.StartProcess`等函数时，第一个参数（path）直接取外部输入值时，应使用白名单限定可执行的命令范围，不允许传入`bash`、`cmd`、`sh`等命令；
- 使用`exec.Command`、`exec.CommandContext`等函数时，通过`bash`、`cmd`、`sh`等创建shell，-c后的参数（arg）拼接外部输入，应过滤\n  $  &  ;  |  '  "  ( )  `等潜在恶意字符；

**反例**

```go
func foo() {
	userInputedVal := "&& echo 'hello'" // 假设外部传入该变量值
	cmdName := "ping " + userInputedVal

	// 未判断外部输入是否存在命令注入字符，结合sh可造成命令注入
	cmd := exec.Command("sh", "-c", cmdName)
	output, _ := cmd.CombinedOutput()
	fmt.Println(string(output))

	cmdName := "ls"
	// 未判断外部输入是否是预期命令
	cmd := exec.Command(cmdName)
	output, _ := cmd.CombinedOutput()
	fmt.Println(string(output))
}


```

**正例**

```go
func checkIllegal(cmdName string) bool {
	if strings.Contains(cmdName, "&") || strings.Contains(cmdName, "|") || strings.Contains(cmdName, ";") ||
		strings.Contains(cmdName, "$") || strings.Contains(cmdName, "'") || strings.Contains(cmdName, "`") ||
		strings.Contains(cmdName, "(") || strings.Contains(cmdName, ")") || strings.Contains(cmdName, "\"") {
		return true
	}
	return false
}

func main() {
	userInputedVal := "&& echo 'hello'"
	cmdName := "ping " + userInputedVal

	if checkIllegal(cmdName) { // 检查传给sh的命令是否有特殊字符
		return // 存在特殊字符直接return
	}

	cmd := exec.Command("sh", "-c", cmdName)
	output, _ := cmd.CombinedOutput()
	fmt.Println(string(output))
}
```



# Web跨域

## :point_right:跨域资源共享CORS限制请求来源

CORS请求保护不当可导致敏感信息泄漏，因此应当严格设置Access-Control-Allow-Origin使用同源策略进行保护。

**正例**

```go
c := cors.New(cors.Options{
	AllowedOrigins:   []string{"http://qq.com", "https://qq.com"},
	AllowCredentials: true,
	Debug:            false,
})

// 引入中间件
handler = c.Handler(handler)
```

# 响应输出

## :point_right:设置正确的HTTP响应包类型

响应头Content-Type与实际响应内容，应保持一致。如：API响应数据类型是json，则响应头使用`application/json`；若为xml，则设置为`text/xml`。

## 添加安全响应头

- 所有接口、页面，添加响应头 `X-Content-Type-Options: nosniff`。
- 所有接口、页面，添加响应头`X-Frame-Options `。按需合理设置其允许范围，包括：`DENY`、`SAMEORIGIN`、`ALLOW-FROM origin`。用法参考：[MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/X-Frame-Options)

## 外部输入拼接到HTTP响应头中需进行过滤

- 应尽量避免外部可控参数拼接到HTTP响应头中，如业务需要则需要过滤掉`\r`、`\n`等换行符，或者拒绝携带换行符号的外部输入。

## 外部输入拼接到response页面前进行编码处理

- 直出html页面或使用模板生成html页面的，推荐使用`text/template`自动编码，或者使用`html.EscapeString`或`text/template`对`<, >, &, ',"`等字符进行编码。

```go
import (
	"html/template"
)

func outtemplate(w http.ResponseWriter, r *http.Request) {
	param1 := r.URL.Query().Get("param1")
	tmpl := template.New("hello")
	tmpl, _ = tmpl.Parse(`{{define "T"}}{{.}}{{end}}`)
	tmpl.ExecuteTemplate(w, "T", param1)
}
```

# 会话管理

## :point_right:安全维护session信息

- 用户登录时应重新生成session，退出登录后应清理session。

```go
import (
	"github.com/gorilla/handlers"
	"github.com/gorilla/mux"
	"net/http"
)

// 创建cookie
func setToken(res http.ResponseWriter, req *http.Request) {
	expireToken := time.Now().Add(time.Minute * 30).Unix()
	expireCookie := time.Now().Add(time.Minute * 30)

	//...

	cookie := http.Cookie{
		Name:     "Auth",
		Value:    signedToken,
		Expires:  expireCookie, // 过期失效
		HttpOnly: true,
		Path:     "/",
		Domain:   "127.0.0.1",
		Secure:   true,
	}

	http.SetCookie(res, &cookie)
	http.Redirect(res, req, "/profile", 307)
}

// 删除cookie
func logout(res http.ResponseWriter, req *http.Request) {
	deleteCookie := http.Cookie{
		Name:    "Auth",
		Value:   "none",
		Expires: time.Now(),
	}
	http.SetCookie(res, &deleteCookie)
	return
}
```

## CSRF防护

- 涉及系统敏感操作或可读取敏感信息的接口应校验`Referer`或添加`csrf_token`。

```go
// good
import (
	"github.com/gorilla/csrf"
	"github.com/gorilla/mux"
	"net/http"
)

func main() {
	r := mux.NewRouter()
	r.HandleFunc("/signup", ShowSignupForm)
	r.HandleFunc("/signup/post", SubmitSignupForm)
	// 使用csrf_token验证
	http.ListenAndServe(":8000",
		csrf.Protect([]byte("32-byte-long-auth-key"))(r))
}
```

# 访问控制

## 默认鉴权

- 除非资源完全可对外开放，否则系统默认进行身份认证，使用白名单的方式放开不需要认证的接口或页面。

- 根据资源的机密程度和用户角色，以最小权限原则，设置不同级别的权限，如完全公开、登录可读、登录可写、特定用户可读、特定用户可写等

- 涉及用户自身相关的数据的读写必须验证登录态用户身份及其权限，避免越权操作

  ```sql
  -- 伪代码
  select id from table where id=:id and userid=session.userid
  ```

- 没有独立账号体系的外网服务使用`QQ`或`微信`登录，内网服务使用`统一登录服务`登录，其他使用账号密码登录的服务需要增加验证码等二次验证

# 其他

## :point_right:禁止使用math/rand包提供的函数产生安全用途的伪随机数

math/rand 包提供生成的伪随机数，不能保证其产生的随机数序列质量。

crypto/rand （推荐使用）包中提供了密码安全学伪随机数生成器。提供reader变量，在Unix中reader变量读取/dev/urandom生成随机数，在linux系统中使用getrandom生成随机数，在windows系统中使用CryptGenRandom API生成随机数

安全用于包括但不限于以下几种

- 会话标识的SessionID
- 挑战算法中的伪随机数生成
- 验证码中的随机数生成
- 用于密码算法用途（例如：生成IV，salt值、秘钥等）的随机数生成

## 禁止代码中包含公网地址

对产品发布的软件（包含软件包、补丁包）中的公网地址（包含公网ip、公网url地址、域名、邮箱地址）要求如下

- 禁止包含用户界面不可见、或者产品资料为公开的公网地址
- 已公开的公网地址禁止写在代码和脚本中，可存在在配置文件或者数据库中

> *import 包中的url 是例外*

## 删除无效或者永不执行的代码

注重DT覆盖率，无效或者永不执行的代码通常是编程错误的结果，应该主动识别整理和消除。

> 公共库中未使用的导出函数和变量不违反该准则

## :point_right:不用代码直接删除掉，不要注释掉

被注释掉的代码，无法被正常维护。当企图恢复这段代码时，极有可能被引入被忽略的缺陷

正确的做法是，不需要的代码直接删除掉。若需要时可以通过版本管理工具恢复

> *cgo中的注释是例外*

## 避免调用os.Exit()和runtime.Goexit()退出函数

程序应该优雅的退出，具备有序的退出机制，并在真正退出前做好善后的工作。

除main函数外，禁止在任何地方调用os.Exit()函数，调用os.Exit()会立即终止，终止前无法执行相应的defer，可能导致某些资源难以有效的清理。

正确的做法是通过错误值传递到上一级调用者。结束前确保资源有序的释放，禁止下级程序直接结束进程。





# 参考资料

Effective Go

GO code review Comment

Uber go style Guide

Gitlab Go guide

go proverbs

[腾讯安全编码规范](https://github.com/Tencent/secguide)

