<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Go Clean Code](#go-clean-code)
- [有意义的命名](#%E6%9C%89%E6%84%8F%E4%B9%89%E7%9A%84%E5%91%BD%E5%90%8D)
  - [名副其实](#%E5%90%8D%E5%89%AF%E5%85%B6%E5%AE%9E)
  - [避免误导](#%E9%81%BF%E5%85%8D%E8%AF%AF%E5%AF%BC)
  - [做有意义的区分](#%E5%81%9A%E6%9C%89%E6%84%8F%E4%B9%89%E7%9A%84%E5%8C%BA%E5%88%86)
  - [golang命名规范](#golang%E5%91%BD%E5%90%8D%E8%A7%84%E8%8C%83)
- [名句摘抄](#%E5%90%8D%E5%8F%A5%E6%91%98%E6%8A%84)
- [参考](#%E5%8F%82%E8%80%83)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Go Clean Code

golang 代码整洁之道

阅读本书的两个原因：1、你是程序员；2、你想成为更好的程序员。

衡量代码的唯一标准：WTF/min

> 就是每分钟说出 what the fuck的次数，简单来讲就是当某个人在看一份代码时每分钟爆粗口的次数。

# 有意义的命名

万物从命名开始。

my name is stonebird

## 名副其实

起有意义的名字，让人一目了然。

变量、函数、结构体、接口的名称应该回答了大多数用途所在，做什么事、怎么用等信息

**正例**

```go
var newOrder int = 10
var userList []string
```

**反例**

```go
var array []int64
var num int64
```

> 争取做到不用注释就能知道其目的，如果需要注释来补充，那就不算名副其实

## 避免误导

不要用太长或者很偏僻的单词来命名，也不要用拼音代替英文。
更不要用容易混淆的字母（字母+数字）。尤其是`l`和`O`两个字母，和数字1和0太像了。

**正例**

```go
func modifyPassword(oldPassword,newPassword string) error {
    return nil
}

func getAddress() string {
    return ""
}

func dos2unix() error{
    ...
}
```

反例

```go
func modifyPassword(password1,password2 string) error {
    return nil
}

func getDizhi() string {
    return ""
}

func get1Inf0() {
    ...
}
```

> 除业界通用的缩写外，尽量不要使用缩写

## 做有意义的区分

如果缺少明确约定，那么变量 **moneyAmount** 与 **money** 就没区别，**customerInfo** 与 **customer** 没区别，**accountData** 与 **account** 没区别，**theMessage** 也与 **message** 没区别。要区分名称，就要以读者能鉴别不同之处的方式来区分。

**反例**

```go
var accountData []*Account
var account []*Account

func Account(id int) *Account {
    // ...
}

func AccountData(id int) *Account {
    // ...
}
```

**正例**

```go
func getStudentInfoByID(id string) string{
    // ...
}

func getStudentInfoByName(name string) string{
    // ...
}
```

## golang命名规范





# 名句摘抄

- 函数的第一规则是要短小，第二条规则是还要更短小。
- 如果每个例程都让你感到深合己意，那就是整洁代码。 如果代码让编程语言看起来像是专为解决那个问题而存在，就可以称之为漂亮的代码。
- 我们都曾经瞟一眼自己亲手造成的混乱，决定弃之不顾，走向新一天。 我们都曾经看到自己的烂代码居然能运行，然后断言能运行的烂程序总比没有强。 我们都曾经说过有朝一日再回头清理。 当然，那些日子里，我们都没听过Law of LeBlanc：Later equals never 
- 要不断的完善代码，所以代码永存。

# 参考

- 《Clean Code代码整洁之道》
- [golang安全编码规范]()

