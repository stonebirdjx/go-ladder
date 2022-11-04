<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [go设计模式](#go%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F)
- [0、设计模式的七大原则](#0%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E7%9A%84%E4%B8%83%E5%A4%A7%E5%8E%9F%E5%88%99)
  - [0.1、开闭原则](#01%E5%BC%80%E9%97%AD%E5%8E%9F%E5%88%99)
  - [0.2、单一职责原则](#02%E5%8D%95%E4%B8%80%E8%81%8C%E8%B4%A3%E5%8E%9F%E5%88%99)
  - [0.3、里氏代换原则](#03%E9%87%8C%E6%B0%8F%E4%BB%A3%E6%8D%A2%E5%8E%9F%E5%88%99)
  - [0.4、依赖倒转原则](#04%E4%BE%9D%E8%B5%96%E5%80%92%E8%BD%AC%E5%8E%9F%E5%88%99)
  - [0.5、接口隔离原则](#05%E6%8E%A5%E5%8F%A3%E9%9A%94%E7%A6%BB%E5%8E%9F%E5%88%99)
  - [0.6、合成/聚合复用原则](#06%E5%90%88%E6%88%90%E8%81%9A%E5%90%88%E5%A4%8D%E7%94%A8%E5%8E%9F%E5%88%99)
  - [0.7、迪米特法则](#07%E8%BF%AA%E7%B1%B3%E7%89%B9%E6%B3%95%E5%88%99)
- [1、创建型模式](#1%E5%88%9B%E5%BB%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F)
  - [1.1、单例模式（singleton）](#11%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8Fsingleton)
  - [1.2、工厂模式（factory）](#12%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8Ffactory)
    - [1.2.1、简单工程模式（simple factory）](#121%E7%AE%80%E5%8D%95%E5%B7%A5%E7%A8%8B%E6%A8%A1%E5%BC%8Fsimple-factory)
    - [1.2.2、工厂方法模式（method factory）](#122%E5%B7%A5%E5%8E%82%E6%96%B9%E6%B3%95%E6%A8%A1%E5%BC%8Fmethod-factory)
    - [1.2.3、抽象工厂模式（abstract factory）](#123%E6%8A%BD%E8%B1%A1%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8Fabstract-factory)
  - [1.3、创建者模式（builder）](#13%E5%88%9B%E5%BB%BA%E8%80%85%E6%A8%A1%E5%BC%8Fbuilder)
  - [1.4、原型模式（prototype）](#14%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8Fprototype)
- [2、结构型模式](#2%E7%BB%93%E6%9E%84%E5%9E%8B%E6%A8%A1%E5%BC%8F)
  - [2.1、代理模式（Proxy）](#21%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8Fproxy)
  - [2.2、桥接模式（bridge）](#22%E6%A1%A5%E6%8E%A5%E6%A8%A1%E5%BC%8Fbridge)
  - [2.3、装饰器模式（decorator）](#23%E8%A3%85%E9%A5%B0%E5%99%A8%E6%A8%A1%E5%BC%8Fdecorator)
  - [2.4、适配器模式（adapter）](#24%E9%80%82%E9%85%8D%E5%99%A8%E6%A8%A1%E5%BC%8Fadapter)
  - [2.5、外观模式（facade）](#25%E5%A4%96%E8%A7%82%E6%A8%A1%E5%BC%8Ffacade)
  - [2.6、组合模式（composite）](#26%E7%BB%84%E5%90%88%E6%A8%A1%E5%BC%8Fcomposite)
  - [2.7、享元模式（flyweight）](#27%E4%BA%AB%E5%85%83%E6%A8%A1%E5%BC%8Fflyweight)
- [3、行为型模式](#3%E8%A1%8C%E4%B8%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F)
  - [3.1、观察者模式（observer）](#31%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8Fobserver)
  - [3.2、模板模式（template）](#32%E6%A8%A1%E6%9D%BF%E6%A8%A1%E5%BC%8Ftemplate)
  - [3.3、策略模式（strategy）](#33%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8Fstrategy)
  - [3.4、责任链模式（chain）](#34%E8%B4%A3%E4%BB%BB%E9%93%BE%E6%A8%A1%E5%BC%8Fchain)
  - [3.5、状态模式（state）](#35%E7%8A%B6%E6%80%81%E6%A8%A1%E5%BC%8Fstate)
  - [3.6、迭代器模式（iterator）](#36%E8%BF%AD%E4%BB%A3%E5%99%A8%E6%A8%A1%E5%BC%8Fiterator)
  - [3.7、访问者模式（visitor）](#37%E8%AE%BF%E9%97%AE%E8%80%85%E6%A8%A1%E5%BC%8Fvisitor)
  - [3.8、备忘录模式（memento）](#38%E5%A4%87%E5%BF%98%E5%BD%95%E6%A8%A1%E5%BC%8Fmemento)
  - [3.9、命名模式（command）](#39%E5%91%BD%E5%90%8D%E6%A8%A1%E5%BC%8Fcommand)
  - [3.10、解释器模式（interpreter）](#310%E8%A7%A3%E9%87%8A%E5%99%A8%E6%A8%A1%E5%BC%8Finterpreter)
  - [3.11、中介者模式（mediator）](#311%E4%B8%AD%E4%BB%8B%E8%80%85%E6%A8%A1%E5%BC%8Fmediator)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# go设计模式

整理知识，传播智慧

好好学习，天天向上

gopher：石鸟路遇

email：1245863260@qq.com

Simple, Poetic, Pithy

文档为个人工作和收集积累，不涉及商用，仅供学习和参考，如有不正确的地方，请指正。

powerby：https://github.com/senghoo/golang-design-pattern

​					https://lailin.xyz/post/go-design-pattern.html

GitHub源代码：https://github.com/stonebirdjx/go-patterns



# 0、设计模式的七大原则

| 标记 | 设计模式原则名称  | 简单定义                                         |
| :--- | :---------------- | :----------------------------------------------- |
| OCP  | 开闭原则          | 对扩展开放，对修改关闭                           |
| SRP  | 单一职责原则      | 一个类只负责一个功能领域中的相应职责             |
| LSP  | 里氏代换原则      | 所有引用基类的地方必须能透明地使用其子类的对象   |
| DIP  | 依赖倒转原则      | 依赖于抽象，不能依赖于具体实现                   |
| ISP  | 接口隔离原则      | 类之间的依赖关系应该建立在最小的接口上           |
| CARP | 合成/聚合复用原则 | 尽量使用合成/聚合，而不是通过继承达到复用的目的  |
| LOD  | 迪米特法则        | 一个软件实体应当尽可能少的与其他实体发生相互作用 |



## 0.1、开闭原则

开闭原则（Open Closed Principle，OCP）的定义是：一个软件实体如类、模块和函数应该对扩展开放，对修改关闭。模块应尽量在不修改原（是"原"，指原来的代码）代码的情况下进行扩展。

当软件需要变化时，尽量通过扩展软件实体的行为来实现变化，而不是通过修改已有的代码来实现变化。



## 0.2、单一职责原则

单一职责原则（Single Responsibility Principle, SRP）的定义是：指一个类或者模块应该有且只有一个改变的原因。如果一个类承担的职责过多，就等于把这些职责耦合在一起了。



## 0.3、里氏代换原则

里氏代换原则（Liskov Substitution Principle，LSP）的定义是：所有引用基类的地方必须能透明地使用其子类的对象，**也可以简单理解为任何基类可以出现的地方，子类一定可以出现。**



## 0.4、依赖倒转原则

依赖倒转原则（Dependency Inversion Principle，DIP）的定义：**程序要依赖于抽象接口，不要依赖于具体实现**。简单的说就是要求对抽象进行编程，不要对实现进行编程，这样就降低了客户与实现模块间的耦合。

依赖倒转原则要求我们在程序代码中传递参数时或在关联关系中，尽量引用层次高的抽象层类，即使用接口和抽象类进行变量类型声明、参数类型声明、方法返回类型声明，以及数据类型的转换等，而不要用具体类来做这些事情。



## 0.5、接口隔离原则

接口隔离原则（Interface Segregation Principle，ISP）的定义是客户端不应该依赖它不需要的接口，类间的依赖关系应该建立在最小的接口上。简单来说就是建立单一的接口，不要建立臃肿庞大的接口。也就是接口尽量细化，同时接口中的方法尽量少。



## 0.6、合成/聚合复用原则

合成/聚合复用原则（Composite/Aggregate Reuse Principle，CARP）一般也叫合成复用原则(Composite Reuse Principle, CRP)，定义是：**尽量使用合成/聚合，而不是通过继承达到复用的目的**。

合成/聚合复用原则就是在一个新的对象里面使用一些已有的对象，使之成为新对象的一部分；新的对象通过向内部持有的这些对象的委派达到复用已有功能的目的，而不是通过继承来获得已有的功能。

**聚合(Aggregate)的概念**

聚合表示一种弱的"拥有"关系，一般表现为松散的整体和部分的关系，其实，所谓整体和部分也可以是完全不相关的。例如A对象持有B对象，B对象并不是A对象的一部分，也就是B对象的生命周期是B对象自身管理，和A对象不相关。

**合成(Composite)的概念**

合成表示一种强的"拥有"关系，一般表现为严格的整体和部分的关系，部分和整体的生命周期是一样的



## 0.7、迪米特法则

迪米特法则（Law of Demeter，LOD），有时候也叫做最少知识原则（Least Knowledge Principle，LKP），它的定义是：一个软件实体应当尽可能少地与其他实体发生相互作用。每一个软件单位对其他的单位都只有最少的知识，而且局限于那些与本单位密切相关的软件单位。迪米特法则的初衷在于降低类之间的耦合。

- 尽量少发布public的变量和方法，一旦公开的属性和方法越多，修改的时候影响的范围越大。



# 1、创建型模式

## 1.1、单例模式（singleton）

这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。

单例模式采用了 **饿汉式** 和 **懒汉式** 两种实现

饿汉式：可以将问题及早暴露。性能高、实现简单

懒汉式：延迟加载

**使用场景：**

- 1、要求生产唯一序列号。
- 2、WEB 中的计数器，不用每次刷新都在数据库里加一次，用单例先缓存起来。
- 3、创建的一个对象需要消耗的资源过多，比如 I/O 与数据库的连接等。

![singleton](E:\material\md\img\singleton.jpeg)

singleton.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: singleton.go
// @Date: 2022/2/22 22:30
// @Desc: 单例模式
package singleton

import "sync"

type singleton struct{}

var hungerSingleton *singleton

// 饿汉式：可以将问题及早暴露
func init() {
	hungerSingleton = &singleton{}
}

func GetHungerInstance() *singleton {
	return hungerSingleton
}

// 懒汉式：将问题延时暴露
var (
	lazySingleton *singleton
	once          sync.Once
)

func GetLazyInstance() *singleton {
	if lazySingleton == nil {
		once.Do(func() {
			lazySingleton = &singleton{}
		})
	}
	return lazySingleton
}
```

singleton_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: singleton_test.go
// @Date: 2022/2/22 22:39
// @Desc:
package singleton

import (
	"testing"
	"unsafe"
)

func TestGetHungerInstance(t *testing.T) {
	if uintptr(unsafe.Pointer(GetHungerInstance())) != uintptr(unsafe.Pointer(GetHungerInstance())) {
		t.Errorf("hunger instance is not single")
	}
}

func BenchmarkGetHungerInstance(b *testing.B) {
	for i := 0; i < b.N; i++ {
		if uintptr(unsafe.Pointer(GetHungerInstance())) != uintptr(unsafe.Pointer(GetHungerInstance())) {
			b.Errorf("hunger instance is not single")
		}
	}
}

func TestGetLazyInstance(t *testing.T) {
	if uintptr(unsafe.Pointer(GetLazyInstance())) != uintptr(unsafe.Pointer(GetLazyInstance())) {
		t.Errorf("lazy instance is not single")
	}
}

func BenchmarkGetLazyInstance(b *testing.B) {
	for i := 0; i < b.N; i++ {
		if uintptr(unsafe.Pointer(GetLazyInstance())) != uintptr(unsafe.Pointer(GetLazyInstance())) {
			b.Errorf("lazy instance is not single")
		}
	}
}
```

## 1.2、工厂模式（factory）

![factory](E:\material\md\img\factory.jpeg)

### 1.2.1、简单工程模式（simple factory）

在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过**使用一个共同的接口来指向新创建的对象**。

go 语言没有构造函数一说，所以一般会定义NewXXX函数来初始化相关类。 NewXXX 函数返回接口时就是简单工厂模式，也就是说Golang的一般推荐做法就是简单工厂。

在这个simplefactory包中只有API 接口和NewAPI函数为包外可见，封装了实现细节。

**使用场景：** 

1、日志记录器：记录可能记录到本地硬盘、系统事件、远程服务器等，用户可以选择记录日志到什么地方。

2、数据库访问，当用户不知道最后系统采用哪一类数据库，以及数据库可能有变化时。 

3、设计一个连接服务器的框架，需要三个协议，"POP3"、"IMAP"、"HTTP"，可以把这三个作为产品类，共同实现一个接口。

factorysimple.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: factorysimple.go
// @Date: 2022/2/26 10:26
// @Desc: 简单工程模式
package factory

import "fmt"

type selector int

var hi selector = 1
var hello selector = 2

type api interface {
	say(name string) string
}

type hiApi struct{}

func (*hiApi) say(name string) string {
	return fmt.Sprintf("hi,%s", name)
}

type helloApi struct{}

func (*helloApi) say(name string) string {
	return fmt.Sprintf("hello,%s", name)
}

func NewApi(s selector) api {
	switch s {
	case hi:
		return &hiApi{}
	case hello:
		return &helloApi{}
	default:
	}
	return nil
}
```

factorysimple_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: factorysimple_test.go
// @Date: 2022/2/26 10:41
// @Desc: 简单工厂模式测试方法
package factory

import "testing"

// TestNewApiHi 是 hiApi的测试方法
func TestNewApiHi(t *testing.T) {
	resApi := NewApi(hi)
	if resApi == nil {
		t.Error("hi Api is nil")
		return
	}
	if resApi.say("hjx") != "hi,hjx" {
		t.Error("hi Api return err")
	}
}

// TestNewApiHollo 是 helloApi的测试方法
func TestNewApiHollo(t *testing.T) {
	resApi := NewApi(hello)
	if resApi == nil {
		t.Error("hello Api is nil")
		return
	}
	if resApi.say("hjx") != "hello,hjx" {
		t.Error("hello Api return err")
	}
}

// TestNewApiNil 测试nil 值
func TestNewApiNil(t *testing.T) {
	resApi := NewApi(selector(3))
	if resApi != nil {
		t.Error("resApi is not nil")
		return
	}
}
```

### 1.2.2、工厂方法模式（method factory）

工厂方法模式使用子类的方式**延迟生成对象到子类中实现**。

Go中不存在继承 所以使用匿名组合来实现

factorymethod.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: factorymethod.go
// @Date: 2022/2/26 11:02
// @Desc: 工厂方法模式：加工工厂,延伸到子类中去实践
package factory

//Operator 是被封装的实际类接口
type Operator interface {
	SetA(int)
	SetB(int)
	Result() int
}

//OperatorFactory 是工厂接口
type OperatorFactory interface {
	Create() Operator
}

//OperatorBase 是Operator 接口实现的基类，封装公用方法
type OperatorBase struct {
	a, b int
}

//SetA 设置 A
func (o *OperatorBase) SetA(a int) {
	o.a = a
}

//SetB 设置 B
func (o *OperatorBase) SetB(b int) {
	o.b = b
}

//PlusOperatorFactory 是 PlusOperator 的工厂类
type PlusOperatorFactory struct{}

func (PlusOperatorFactory) Create() Operator {
	return &PlusOperator{
		OperatorBase: &OperatorBase{},
	}
}

//PlusOperator Operator 的实际加法实现
type PlusOperator struct {
	*OperatorBase
}

//Result 获取结果
func (o PlusOperator) Result() int {
	return o.a + o.b
}

//MinusOperatorFactory 是 MinusOperator 的工厂类
type MinusOperatorFactory struct{}

func (MinusOperatorFactory) Create() Operator {
	return &MinusOperator{
		OperatorBase: &OperatorBase{},
	}
}

//MinusOperator Operator 的实际减法实现
type MinusOperator struct {
	*OperatorBase
}

//Result 获取结果
func (o MinusOperator) Result() int {
	return o.a - o.b
}
```

### 1.2.3、抽象工厂模式（abstract factory）

**抽象工厂模式用于生成产品族的工厂，所生成的对象是有关联的。**

> *工厂模式的加工厂，加工成工厂模式*

如果抽象工厂退化成生成的对象无关联则成为工厂函数模式。

比如本例子中使用RDB和XML存储订单信息，抽象工厂分别能生成相关的主订单信息和订单详情信息。 如果业务逻辑中需要替换使用的时候只需要改动工厂函数相关的类就能替换使用不同的存储方式了。

factoryabstract.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: factoryabstract.go
// @Date: 2022/2/26 11:24
// @Desc: 抽象工厂模式：工厂模式的加工厂
package factory

import "fmt"

//OrderMainDAO 为订单主记录
type OrderMainDAO interface {
	SaveOrderMain()
}

//OrderDetailDAO 为订单详情纪录
type OrderDetailDAO interface {
	SaveOrderDetail()
}

//DAOFactory DAO 抽象模式工厂接口
type DAOFactory interface {
	CreateOrderMainDAO() OrderMainDAO
	CreateOrderDetailDAO() OrderDetailDAO
}

//RDBMainDAP 为关系型数据库的OrderMainDAO实现
type RDBMainDAO struct{}

//SaveOrderMain ...
func (*RDBMainDAO) SaveOrderMain() {
	fmt.Print("rdb main save\n")
}

//RDBDetailDAO 为关系型数据库的OrderDetailDAO实现
type RDBDetailDAO struct{}

// SaveOrderDetail ...
func (*RDBDetailDAO) SaveOrderDetail() {
	fmt.Print("rdb detail save\n")
}

//RDBDAOFactory 是RDB 抽象工厂实现
type RDBDAOFactory struct{}

func (*RDBDAOFactory) CreateOrderMainDAO() OrderMainDAO {
	return &RDBMainDAO{}
}

func (*RDBDAOFactory) CreateOrderDetailDAO() OrderDetailDAO {
	return &RDBDetailDAO{}
}

//XMLMainDAO XML存储
type XMLMainDAO struct{}

//SaveOrderMain ...
func (*XMLMainDAO) SaveOrderMain() {
	fmt.Print("xml main save\n")
}

//XMLDetailDAO XML存储
type XMLDetailDAO struct{}

// SaveOrderDetail ...
func (*XMLDetailDAO) SaveOrderDetail() {
	fmt.Print("xml detail save")
}

//XMLDAOFactory 是RDB 抽象工厂实现
type XMLDAOFactory struct{}

func (*XMLDAOFactory) CreateOrderMainDAO() OrderMainDAO {
	return &XMLMainDAO{}
}

func (*XMLDAOFactory) CreateOrderDetailDAO() OrderDetailDAO {
	return &XMLDetailDAO{}
}
```

factoryabstract_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: factoryabstract_test.go
// @Date: 2022/2/26 11:26
// @Desc: 抽象工厂模式的测试方法
package factory

func getMainAndDetail(factory DAOFactory) {
	factory.CreateOrderMainDAO().SaveOrderMain()
	factory.CreateOrderDetailDAO().SaveOrderDetail()
}

func ExampleRdbFactory() {
	var factory DAOFactory
	factory = &RDBDAOFactory{}
	getMainAndDetail(factory)
	// Output:
	// rdb main save
	// rdb detail save
}

func ExampleXmlFactory() {
	var factory DAOFactory
	factory = &XMLDAOFactory{}
	getMainAndDetail(factory)
	// Output:
	// xml main save
	// xml detail save
}
```

## 1.3、创建者模式（builder）

建造者模式（Builder Pattern）使用多个简单的对象一步一步构建成一个复杂的对象。

使用 Go 编写建造者模式的代码其实会很长。

**应用实例：** 1、去肯德基，汉堡、可乐、薯条、炸鸡翅等是不变的，而其组合是经常变化的，生成出所谓的"套餐"。

![builder](E:\material\md\img\builder.jpeg)

builder.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: builder.go
// @Date: 2022/2/26 14:07
// @Desc: 创建者模式:使用多个简单的对象一步一步构建成一个复杂的对象
package builder

//Builder 是生成器接口
type Builder interface {
	Part1()
	Part2()
	Part3()
}

type Director struct {
	builder Builder
}

// NewDirector ...
func NewDirector(builder Builder) *Director {
	return &Director{
		builder: builder,
	}
}

//Construct Product
func (d *Director) Construct() {
	d.builder.Part1()
	d.builder.Part2()
	d.builder.Part3()
}

type Builder1 struct {
	result string
}

func (b *Builder1) Part1() {
	b.result += "1"
}

func (b *Builder1) Part2() {
	b.result += "2"
}

func (b *Builder1) Part3() {
	b.result += "3"
}

func (b *Builder1) GetResult() string {
	return b.result
}

type Builder2 struct {
	result int
}

func (b *Builder2) Part1() {
	b.result += 1
}

func (b *Builder2) Part2() {
	b.result += 2
}

func (b *Builder2) Part3() {
	b.result += 3
}

func (b *Builder2) GetResult() int {
	return b.result
}
```

builder_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: builder_test.go
// @Date: 2022/2/26 14:09
// @Desc: 创建者模式测试方法
package builder

import "testing"

func TestBuilder1(t *testing.T) {
	builder := &Builder1{}
	director := NewDirector(builder)
	director.Construct()
	res := builder.GetResult()
	if res != "123" {
		t.Fatalf("Builder1 fail expect 123 acture %s", res)
	}
}

func TestBuilder2(t *testing.T) {
	builder := &Builder2{}
	director := NewDirector(builder)
	director.Construct()
	res := builder.GetResult()
	if res != 6 {
		t.Fatalf("Builder2 fail expect 6 acture %d", res)
	}
}
```

## 1.4、原型模式（prototype）

原型模式（Prototype Pattern）是用于创建重复的对象，同时又能保证性能。

原型模式使对象能复制自身，并且暴露到接口中，使客户端面向接口编程时，不知道接口实际对象的情况下生成新的对象。

原型模式配合原型管理器使用，使得客户端在不知道具体类的情况下，通过接口管理器得到新的实例，并且包含部分预设定配置。

**使用场景：** 1、资源优化场景。 2、类初始化需要消化非常多的资源，这个资源包括数据、硬件资源等。 3、性能和安全要求的场景。 4、通过 new 产生一个对象需要非常繁琐的数据准备或访问权限，则可以使用原型模式。

> *实现克隆操作*

![prototype](E:\material\md\img\prototype.jpeg)

prototype.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: prototype.go
// @Date: 2022/2/26 14:22
// @Desc: 原型模式：原型模式能够负责自身对象
package prototype

// Cloneable 是原型对象实现的接口
type Cloneable interface {
	Clone() Cloneable
}

type PrototypeManager struct {
	prototypes map[string]Cloneable
}

func NewPrototypeManager() *PrototypeManager {
	return &PrototypeManager{
		prototypes: make(map[string]Cloneable),
	}
}

func (p *PrototypeManager) Get(name string) Cloneable {
	return p.prototypes[name].Clone()
}

func (p *PrototypeManager) Set(name string, prototype Cloneable) {
	p.prototypes[name] = prototype
}
```

prototype_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: prototype_test.go
// @Date: 2022/2/26 14:30
// @Desc: 原型模式测试方法
package prototype

import "testing"

var manager *PrototypeManager

type Type1 struct {
	name string
}

func (t *Type1) Clone() Cloneable {
	tc := *t
	return &tc
}

type Type2 struct {
	name string
}

func (t *Type2) Clone() Cloneable {
	tc := *t
	return &tc
}

func TestClone(t *testing.T) {
	t1 := manager.Get("t1")

	t2 := t1.Clone()

	if t1 == t2 {
		t.Fatal("error! get clone not working")
	}
}

func TestCloneFromManager(t *testing.T) {
	c := manager.Get("t1").Clone()

	t1 := c.(*Type1)
	if t1.name != "type1" {
		t.Fatal("error")
	}

}

func init() {
	manager = NewPrototypeManager()

	t1 := &Type1{
		name: "type1",
	}
	manager.Set("t1", t1)
}
```

# 2、结构型模式

## 2.1、代理模式（Proxy）

在代理模式（Proxy Pattern）中，一个类代表另一个类的功能。主要解决在直接访问对象时带来的问题，比如说：要访问的对象在远程的机器上。

代理模式用于延迟处理操作或者在进行实际操作前后进行其它处理。

![proxy](E:\material\md\img\proxy.jpeg)

proxy.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: proxy.go
// @Date: 2022/2/27 9:39
// @Desc: 代理模式：主要解决在直接访问对象时带来的问题
package proxy

type Subject interface {
	Do() string
}

type RealSubject struct{}

func (RealSubject) Do() string {
	return "real"
}

type Proxy struct {
	real RealSubject
}

func (p Proxy) Do() string {
	var res string

	// 在调用真实对象之前的工作，检查缓存，判断权限，实例化真实对象等。。
	res += "pre:"

	// 调用真实对象
	res += p.real.Do()

	// 调用之后的操作，如缓存结果，对结果进行处理等。。
	res += ":after"

	return res
}
```

proxy_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: proxy_test.go
// @Date: 2022/2/27 9:41
// @Desc: 代理模式测试方法
package proxy

import "testing"

func TestProxy(t *testing.T) {
	var sub Subject
	sub = &Proxy{}

	res := sub.Do()

	if res != "pre:real:after" {
		t.Fail()
	}
}
```

## 2.2、桥接模式（bridge）

桥接（Bridge）是用于把抽象化与实现化解耦，使得二者可以独立变化。**主要解决在有多种可能会变化的情况下，用继承会造成类爆炸问题，扩展起来不灵活。**

**使用场景：** 1、如果一个系统需要在构件的抽象化角色和具体化角色之间增加更多的灵活性，避免在两个层次之间建立静态的继承联系，通过桥接模式可以使它们在抽象层建立一个关联关系。 2、对于那些不希望使用继承或因为多层次继承导致系统类的个数急剧增加的系统，桥接模式尤为适用。 3、一个类存在两个独立变化的维度，且这两个维度都需要进行扩展。

桥接模式类似于策略模式，区别在于策略模式封装一系列算法使得算法可以互相替换。

策略模式使抽象部分和实现部分分离，可以独立变化。

![bridge](E:\material\md\img\bridge.jpeg)

bridge.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: bridge.go
// @Date: 2022/2/27 10:02
// @Desc: 桥接模式：是用于把抽象化与实现化解耦。
package bridge

// IMsgSender IMsgSender
type IMsgSender interface {
	Send(msg string) error
}

// EmailMsgSender 发送邮件
// 可能还有 电话、短信等各种实现
type EmailMsgSender struct {
	emails []string
}

// NewEmailMsgSender NewEmailMsgSender
func NewEmailMsgSender(emails []string) *EmailMsgSender {
	return &EmailMsgSender{emails: emails}
}

// Send Send
func (s *EmailMsgSender) Send(msg string) error {
	// 这里去发送消息
	return nil
}

// INotification 通知接口
type INotification interface {
	Notify(msg string) error
}

// ErrorNotification 错误通知
// 后面可能还有 warning 各种级别
type ErrorNotification struct {
	sender IMsgSender
}

// NewErrorNotification NewErrorNotification
func NewErrorNotification(sender IMsgSender) *ErrorNotification {
	return &ErrorNotification{sender: sender}
}

// Notify 发送通知
func (n *ErrorNotification) Notify(msg string) error {
	return n.sender.Send(msg)
}

```

birdge_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: bridge_test.go
// @Date: 2022/2/27 10:05
// @Desc: 桥接模式测试方法
package bridge

import "testing"

func TestErrorNotification_Notify(t *testing.T) {
	sender := NewEmailMsgSender([]string{"test@test.com"})
	n := NewErrorNotification(sender)
	err := n.Notify("test msg")
	if err != nil {
		t.Fatal()
	}
}
```

## 2.3、装饰器模式（decorator）

装饰器模式（Decorator Pattern）允许向一个现有的对象添加新的功能，同时又不改变其结构。

主要解决：一般的，我们为了扩展一个类经常使用继承方式实现，由于继承为类引入静态特征，并且随着扩展功能的增多，子类会很膨胀。

![decorator](E:\material\md\img\decorator.jpeg)

decorator.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: decorator.go
// @Date: 2022/2/27 10:14
// @Desc: 装饰器模式：允许向一个现有的对象添加新的功能，同时又不改变其结构
package decorator

// IDraw IDraw
type IDraw interface {
	Draw() string
}

// Square 正方形
type Square struct{}

// Draw Draw
func (s Square) Draw() string {
	return "this is a square"
}

// ColorSquare 有颜色的正方形
type ColorSquare struct {
	square IDraw
	color  string
}

// NewColorSquare NewColorSquare
func NewColorSquare(square IDraw, color string) ColorSquare {
	return ColorSquare{color: color, square: square}
}

// Draw Draw
func (c ColorSquare) Draw() string {
	return c.square.Draw() + ", color is " + c.color
}
```

decorator_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: decorator_test.go
// @Date: 2022/2/27 10:15
// @Desc: 装饰器模式测试方法
package decorator

import "testing"

func TestColorSquare_Draw(t *testing.T) {
	sq := Square{}
	csq := NewColorSquare(sq, "red")
	got := csq.Draw()
	if got != "this is a square, color is red" {
		t.Fatal()
	}
}
```

## 2.4、适配器模式（adapter）

适配器模式（Adapter Pattern）是作为两个不兼容的接口之间的桥梁。它结合了两个独立接口的功能。

> *适配器模式用于转换一种接口适配另一种接口。*

主要解决：主要解决在软件系统中，常常要将一些"现存的对象"放到新的环境中，而新环境要求的接口是现对象不能满足的。

比如：美国电器 110V，中国 220V，就要有一个适配器将 110V 转化为 220V。

![adapter](E:\material\md\img\adapter.jpeg)

adapter.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: adapter.go
// @Date: 2022/2/27 12:14
// @Desc: 适配器模式：适用于接口转换
package adapter

//Target 是适配的目标接口
type Target interface {
	Request() string
}

//Adaptee 是被适配的目标接口
type Adaptee interface {
	SpecificRequest() string
}

//NewAdaptee 是被适配接口的工厂函数
func NewAdaptee() Adaptee {
	return &adapteeImpl{}
}

//AdapteeImpl 是被适配的目标类
type adapteeImpl struct{}

//SpecificRequest 是目标类的一个方法
func (*adapteeImpl) SpecificRequest() string {
	return "adaptee method"
}

//NewAdapter 是Adapter的工厂函数
func NewAdapter(adaptee Adaptee) Target {
	return &adapter{
		Adaptee: adaptee,
	}
}

//Adapter 是转换Adaptee为Target接口的适配器
type adapter struct {
	Adaptee
}

//Request 实现Target接口
func (a *adapter) Request() string {
	return a.SpecificRequest()
}
```

adapter_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: adapter_test.go
// @Date: 2022/2/27 12:14
// @Desc: 适配器模式测试方法
package adapter

import "testing"

var expect = "adaptee method"

func TestAdapter(t *testing.T) {
	adaptee := NewAdaptee()
	target := NewAdapter(adaptee)
	res := target.Request()
	if res != expect {
		t.Fatalf("expect: %s, actual: %s", expect, res)
	}
}
```

## 2.5、外观模式（facade）

外观模式（Facade Pattern）隐藏系统的复杂性，并向客户端提供了一个客户端可以访问系统的接口。它向现有的系统添加一个接口，来隐藏系统的复杂性。

主要解决：降低访问复杂系统的内部子系统时的复杂度，简化客户端之间的接口。

比如去医院看病，可能要去挂号、门诊、划价、取药，让患者或患者家属觉得很复杂，如果有提供接待人员，只让接待人员来处理，就很方便。

![facade](E:\material\md\img\facade.jpeg)

facade.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: facade.go
// @Date: 2022/2/27 12:27
// @Desc: 外观模式：提供一个服务接口用于隐藏系统的复杂性
package facade

// IUser 用户接口
type IUser interface {
	Login(phone int, code int) (*User, error)
	Register(phone int, code int) (*User, error)
}

// IUserFacade 门面模式
type IUserFacade interface {
	LoginOrRegister(phone int, code int) error
}

// User 用户
type User struct {
	Name string
}

// UserService UserService
type UserService struct {}

// Login 登录
func (u UserService) Login(phone int, code int) (*User, error) {
	// 校验操作 ...
	return &User{Name: "test login"}, nil
}

// Register 注册
func (u UserService) Register(phone int, code int) (*User, error) {
	// 校验操作 ...
	// 创建用户
	return &User{Name: "test register"}, nil
}

// LoginOrRegister 登录或注册
func (u UserService)LoginOrRegister(phone int, code int) (*User, error) {
	user, err := u.Login(phone, code)
	if err != nil {
		return nil, err
	}

	if user != nil {
		return user, nil
	}

	return u.Register(phone, code)
}
```

facade_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: facade_test.go
// @Date: 2022/2/27 12:28
// @Desc: 外观模式测试方法
package facade

import (
	"reflect"
	"testing"
)

func TestUserService_Login(t *testing.T) {
	service := UserService{}
	user, err := service.Login(13001010101, 1234)
	if err != nil {
		t.Fatal()
	}
	if !reflect.DeepEqual(user, &User{Name: "test login"}){
		t.Fatal()
	}

}

func TestUserService_LoginOrRegister(t *testing.T) {
	service := UserService{}
	user, err := service.LoginOrRegister(13001010101, 1234)
	if err != nil {
		t.Fatal()
	}
	if !reflect.DeepEqual(user, &User{Name: "test login"}){
		t.Fatal()
	}
}

func TestUserService_Register(t *testing.T) {
	service := UserService{}
	user, err := service.Register(13001010101, 1234)
	if err != nil {
		t.Fatal()
	}
	if !reflect.DeepEqual(user, &User{Name: "test register"}){
		t.Fatal()
	}
}
```

## 2.6、组合模式（composite）

组合模式（Composite Pattern），又叫部分整体模式，是用于把一组相似的对象当作一个单一的对象。组合模式依据树形结构来组合对象，用来表示部分以及整体层次。

主要解决：它在我们树型结构的问题中，模糊了简单元素和复杂元素的概念，客户程序可以像处理简单元素一样来处理复杂元素，从而使得客户程序与复杂元素的内部结构解耦。

比如算术表达式包括操作数、操作符和另一个操作数，其中，另一个操作数也可以是操作数、操作符和另一个操作数

![composite](E:\material\md\img\composite.jpeg)

composite.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: composite.go
// @Date: 2022/2/27 12:40
// @Desc: 组合模式：它在我们树型结构的问题中，模糊了简单元素和复杂元素的概念，
//		  客户程序可以像处理简单元素一样来处理复杂元素，从而使得客户程序与复杂元素的内部结构解耦。
package composite

// IOrganization 组织接口，都实现统计人数的功能
type IOrganization interface {
	Count() int
}

// Employee 员工
type Employee struct {
	Name string
}

// Count 人数统计
func (Employee) Count() int {
	return 1
}

// Department 部门
type Department struct {
	Name string

	SubOrganizations []IOrganization
}

// Count 人数统计
func (d Department) Count() int {
	c := 0
	for _, org := range d.SubOrganizations {
		c += org.Count()
	}
	return c
}

// AddSub 添加子节点
func (d *Department) AddSub(org IOrganization) {
	d.SubOrganizations = append(d.SubOrganizations, org)
}

// NewOrganization 构建组织架构 demo
func NewOrganization() IOrganization {
	root := &Department{Name: "root"}
	for i := 0; i < 10; i++ {
		root.AddSub(&Employee{})
		root.AddSub(&Department{Name: "sub", SubOrganizations: []IOrganization{&Employee{}}})
	}
	return root
}
```

composite-test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: composite_test.go
// @Date: 2022/2/27 12:40
// @Desc: 组合模式的测试方法
package composite

import "testing"

func TestNewOrganization(t *testing.T) {
	got := NewOrganization().Count()
	if got != 20 {
		t.Fatal()
	}
}
```

## 2.7、享元模式（flyweight）

享元模式（Flyweight Pattern）主要用于减少创建对象的数量，以减少内存占用和提高性能。

> *享元模式尝试重用现有的同类对象，如果未找到匹配的对象，则创建新对象。*

主要解决：在有大量对象时，有可能会造成内存溢出，我们把其中共同的部分抽象出来，如果有相同的业务请求，直接返回在内存中已有的对象，避免重新创建。

比如数据库的数据池。

![flyweight](E:\material\md\img\flyweight.jpeg)

flyweight.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: flyweight.go
// @Date: 2022/2/27 12:58
// @Desc: 享元模式：享元模式尝试重用现有的同类对象，如果未找到匹配的对象，则创建新对象。
package flyweight

import "fmt"

type ImageFlyweightFactory struct {
	maps map[string]*ImageFlyweight
}

var imageFactory *ImageFlyweightFactory

func GetImageFlyweightFactory() *ImageFlyweightFactory {
	if imageFactory == nil {
		imageFactory = &ImageFlyweightFactory{
			maps: make(map[string]*ImageFlyweight),
		}
	}
	return imageFactory
}

func (f *ImageFlyweightFactory) Get(filename string) *ImageFlyweight {
	image := f.maps[filename]
	if image == nil {
		image = NewImageFlyweight(filename)
		f.maps[filename] = image
	}

	return image
}

type ImageFlyweight struct {
	data string
}

func NewImageFlyweight(filename string) *ImageFlyweight {
	// Load image file
	data := fmt.Sprintf("image data %s", filename)
	return &ImageFlyweight{
		data: data,
	}
}

func (i *ImageFlyweight) Data() string {
	return i.data
}

type ImageViewer struct {
	*ImageFlyweight
}

func NewImageViewer(filename string) *ImageViewer {
	image := GetImageFlyweightFactory().Get(filename)
	return &ImageViewer{
		ImageFlyweight: image,
	}
}

func (i *ImageViewer) Display() {
	fmt.Printf("Display: %s\n", i.Data())
}
```

flyweight_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: flyweight_test.go
// @Date: 2022/2/27 12:59
// @Desc: 享元模式的测试方法
package flyweight

import "testing"

func ExampleFlyweight() {
	viewer := NewImageViewer("image1.png")
	viewer.Display()
	// Output:
	// Display: image data image1.png
}

func TestFlyweight(t *testing.T) {
	viewer1 := NewImageViewer("image1.png")
	viewer2 := NewImageViewer("image1.png")

	if viewer1.ImageFlyweight != viewer2.ImageFlyweishght {
		t.Fail()
	}
}
```

# 3、行为型模式

## 3.1、观察者模式（observer）

当对象间存在一对多关系时，则使用观察者模式（Observer Pattern）。

主要解决：一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作。

观察者模式用于触发联动。

一个对象的改变会触发其它观察者的相关动作，而此对象无需关心连动对象的具体实现。

![observer](E:\material\md\img\observer.jpeg)

observer.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: observer.go
// @Date: 2022/2/28 20:35
// @Desc: 观察者模式：一个对象的改变会触发其它观察者的相关动作，而此对象无需关心连动对象的具体实现。
package observer

import "fmt"

type Subject struct {
	observers []Observer
	context   string
}

func NewSubject() *Subject {
	return &Subject{
		observers: make([]Observer, 0),
	}
}

func (s *Subject) Attach(o Observer) {
	s.observers = append(s.observers, o)
}

func (s *Subject) notify() {
	for _, o := range s.observers {
		o.Update(s)
	}
}

func (s *Subject) UpdateContext(context string) {
	s.context = context
	s.notify()
}

type Observer interface {
	Update(*Subject)
}

type Reader struct {
	name string
}

func NewReader(name string) *Reader {
	return &Reader{
		name: name,
	}
}

func (r *Reader) Update(s *Subject) {
	fmt.Printf("%s receive %s\n", r.name, s.context)
}
```

observer_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: observer_test.go
// @Date: 2022/2/28 20:35
// @Desc: 观察者模式测试方法
package observer

func ExampleObserver() {
	subject := NewSubject()
	reader1 := NewReader("reader1")
	reader2 := NewReader("reader2")
	reader3 := NewReader("reader3")
	subject.Attach(reader1)
	subject.Attach(reader2)
	subject.Attach(reader3)

	subject.UpdateContext("observer mode")
	// Output:
	// reader1 receive observer mode
	// reader2 receive observer mode
	// reader3 receive observer mode
}
```

## 3.2、模板模式（template）

在模板模式（Template Pattern）中，一个抽象类公开定义了执行它的方法的方式/模板。它的子类可以按需要重写方法实现，但调用将以抽象类中定义的方式进行。

如何解决：将这些通用算法抽象出来。

![template](E:\material\md\img\template.jpeg)

template.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: template.go
// @Date: 2022/2/28 20:49
// @Desc: 模板模式：将通用方法抽离出来，子类需要重写
package template

import "fmt"

// ISMS ISMS
type ISMS interface {
	send(content string, phone int) error
}

// SMS 短信发送基类
type sms struct {
	ISMS
}

// Valid 校验短信字数
func (s *sms) Valid(content string) error {
	if len(content) > 63 {
		return fmt.Errorf("content is too long")
	}
	return nil
}

// Send 发送短信
func (s *sms) Send(content string, phone int) error {
	if err := s.Valid(content); err != nil {
		return err
	}

	// 调用子类的方法发送短信
	return s.send(content, phone)
}

// TelecomSms 走电信通道
type TelecomSms struct {
	*sms
}

// NewTelecomSms NewTelecomSms
func NewTelecomSms() *TelecomSms {
	tel := &TelecomSms{}
	// 这里有点绕，是因为 go 没有继承，用嵌套结构体的方法进行模拟
	// 这里将子类作为接口嵌入父类，就可以让父类的模板方法 Send 调用到子类的函数
	// 实际使用中，我们并不会这么写，都是采用组合+接口的方式完成类似的功能
	tel.sms = &sms{ISMS: tel}
	return tel
}

func (tel *TelecomSms) send(content string, phone int) error {
	fmt.Println("send by telecom success")
	return nil
}
```

template_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: template_test.go
// @Date: 2022/2/28 20:49
// @Desc: 模板模式测试方法
package template

import "testing"

func TestSend(t *testing.T) {
	tel := NewTelecomSms()
	err := tel.Send("test", 1239999)
	if err != nil {
		t.Fatal()
	}
}
```

## 3.3、策略模式（strategy）

在策略模式（Strategy Pattern）中，一个类的行为或其算法可以在运行时更改。

如果在一个系统里面有许多类，它们之间的区别仅在于它们的行为，那么使用策略模式可以动态地让一个对象在许多行为中选择一种行为。 

![strategy](E:\material\md\img\strategy.jpeg)

strategy.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: strategy.go
// @Date: 2022/2/28 21:00
// @Desc: 策略模式：可以动态地让一个对象在许多行为中选择一种行为
package strategy

import "fmt"

type Payment struct {
	context  *PaymentContext
	strategy PaymentStrategy
}

type PaymentContext struct {
	Name, CardID string
	Money        int
}

func NewPayment(name, cardid string, money int, strategy PaymentStrategy) *Payment {
	return &Payment{
		context: &PaymentContext{
			Name:   name,
			CardID: cardid,
			Money:  money,
		},
		strategy: strategy,
	}
}

func (p *Payment) Pay() {
	p.strategy.Pay(p.context)
}

type PaymentStrategy interface {
	Pay(*PaymentContext)
}

type Cash struct{}

func (*Cash) Pay(ctx *PaymentContext) {
	fmt.Printf("Pay $%d to %s by cash", ctx.Money, ctx.Name)
}

type Bank struct{}

func (*Bank) Pay(ctx *PaymentContext) {
	fmt.Printf("Pay $%d to %s by bank account %s", ctx.Money, ctx.Name, ctx.CardID)

}
```

strategy_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: strategy_test.go
// @Date: 2022/2/28 21:00
// @Desc: 策略模式测试方法
package strategy

func ExamplePayByCash() {
	payment := NewPayment("Ada", "", 123, &Cash{})
	payment.Pay()
	// Output:
	// Pay $123 to Ada by cash
}

func ExamplePayByBank() {
	payment := NewPayment("Bob", "0002", 888, &Bank{})
	payment.Pay()
	// Output:
	// Pay $888 to Bob by bank account 0002
}
```

## 3.4、责任链模式（chain）

责任链模式（Chain of Responsibility Pattern）为请求创建了一个接收者对象的链。这种模式给予请求的类型，对请求的发送者和接收者进行解耦。

主要解决：职责链上的处理者负责处理请求，客户只需要将请求发送到职责链上即可，无须关心请求的处理细节和请求的传递，所以职责链将请求的发送者和请求的处理者解耦了。

![chain](E:\material\md\img\chain.jpeg)

chain.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: chain.go
// @Date: 2022/2/28 21:23
// @Desc: 责任链模式：对请求的发送者和接收者进行解耦
// 🌰 假设我们现在有个校园论坛，由于社区规章制度、广告、法律法规的原因需要对用户的发言进行敏感词过滤
//    如果被判定为敏感词，那么这篇帖子将会被封禁
package chain

// SensitiveWordFilter 敏感词过滤器，判定是否是敏感词
type SensitiveWordFilter interface {
	Filter(content string) bool
}

// SensitiveWordFilterChain 职责链
type SensitiveWordFilterChain struct {
	filters []SensitiveWordFilter
}

// AddFilter 添加一个过滤器
func (c *SensitiveWordFilterChain) AddFilter(filter SensitiveWordFilter) {
	c.filters = append(c.filters, filter)
}

// Filter 执行过滤
func (c *SensitiveWordFilterChain) Filter(content string) bool {
	for _, filter := range c.filters {
		// 如果发现敏感直接返回结果
		if filter.Filter(content) {
			return true
		}
	}
	return false
}

// AdSensitiveWordFilter 广告
type AdSensitiveWordFilter struct{}

// Filter 实现过滤算法
func (f *AdSensitiveWordFilter) Filter(content string) bool {
	// TODO: 实现算法
	return false
}

// PoliticalWordFilter 政治敏感
type PoliticalWordFilter struct{}

// Filter 实现过滤算法
func (f *PoliticalWordFilter) Filter(content string) bool {
	// TODO: 实现算法
	return true
}
```

chain_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: chain_test.go
// @Date: 2022/2/28 21:23
// @Desc: 责任链模式测试方法
package chain

import "testing"

func TestAdSensitiveWordFilter_Filter(t *testing.T) {
	chain := &SensitiveWordFilterChain{}
	chain.AddFilter(&AdSensitiveWordFilter{})
	if chain.Filter("test") {
		t.Fatal()
	}

	chain.AddFilter(&PoliticalWordFilter{})
	if !chain.Filter("test") {
		t.Fatal()
	}
}
```

## 3.5、状态模式（state）

在状态模式（State Pattern）中，类的行为是基于它的状态改变的。在状态模式中，我们创建表示各种状态的对象和一个行为随着状态对象改变而改变的 context 对象。

主要解决：对象的行为依赖于它的状态（属性），并且可以根据它的状态改变而改变它的相关行为。

状态模式用于分离状态和行为。

![state](E:\material\md\img\state.jpeg)

state.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: state.go
// @Date: 2022/2/28 21:40
// @Desc: 状态模式：用于分离状态和行为,类的行为是基于它的状态改变的
// 这是一个工作流的例子，在企业内部或者是学校我们经常会看到很多审批流程
// 假设我们有一个报销的流程: 员工提交报销申请 -> 直属部门领导审批 -> 财务审批 -> 结束
// 在这个审批流中，处在不同的环节就是不同的状态
// 而流程的审批、驳回就是不同的事件
package state

import "fmt"

// Machine 状态机
type Machine struct {
	state IState
}

// SetState 更新状态
func (m *Machine) SetState(state IState) {
	m.state = state
}

// GetStateName 获取当前状态
func (m *Machine) GetStateName() string {
	return m.state.GetName()
}

func (m *Machine) Approval() {
	m.state.Approval(m)
}

func (m *Machine) Reject() {
	m.state.Reject(m)
}

// IState 状态
type IState interface {
	// 审批通过
	Approval(m *Machine)
	// 驳回
	Reject(m *Machine)
	// 获取当前状态名称
	GetName() string
}

// leaderApproveState 直属领导审批
type leaderApproveState struct{}

// Approval 获取状态名字
func (leaderApproveState) Approval(m *Machine) {
	fmt.Println("leader 审批成功")
	m.SetState(GetFinanceApproveState())
}

// GetName 获取状态名字
func (leaderApproveState) GetName() string {
	return "LeaderApproveState"
}

// Reject 获取状态名字
func (leaderApproveState) Reject(m *Machine) {}

func GetLeaderApproveState() IState {
	return &leaderApproveState{}
}

// financeApproveState 财务审批
type financeApproveState struct{}

// Approval 审批通过
func (f financeApproveState) Approval(m *Machine) {
	fmt.Println("财务审批成功")
	fmt.Println("出发打款操作")
}

// 拒绝
func (f financeApproveState) Reject(m *Machine) {
	m.SetState(GetLeaderApproveState())
}

// GetName 获取名字
func (f financeApproveState) GetName() string {
	return "FinanceApproveState"
}

// GetFinanceApproveState GetFinanceApproveState
func GetFinanceApproveState() IState {
	return &financeApproveState{}
}
```

state_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: state_test.go
// @Date: 2022/2/28 21:40
// @Desc: 状态模式测试方法
package state

import "testing"

func TestMachine_GetStateName(t *testing.T) {
	m := &Machine{state: GetLeaderApproveState()}
	if m.GetStateName() != "LeaderApproveState" {
		t.Fatal()
	}

	m.Approval()
	if m.GetStateName() != "FinanceApproveState" {
		t.Fatal()
	}

	m.Reject()
	if m.GetStateName() != "LeaderApproveState" {
		t.Fatal()
	}

	m.Approval()
	if m.GetStateName() != "FinanceApproveState" {
		t.Fatal()
	}
}
```

## 3.6、迭代器模式（iterator）

迭代器模式（Iterator Pattern）用于顺序访问集合对象的元素，不需要知道集合对象的底层表示。

访问一个聚合对象的内容而无须暴露它的内部表示。

![iterator](E:\material\md\img\iterator.jpeg)

iterator.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: iterator.go
// @Date: 2022/3/2 8:25
// @Desc: 迭代器模式：用于遍历集合元素
package iterator

// Iterator 迭代器接口
type Iterator interface {
	HasNext() bool
	Next()
	// 获取当前元素，我们直接返回 interface{}
	CurrentItem() interface{}
}

// ArrayInt 数组
type ArrayInt []int

// Iterator 返回迭代器
func (a ArrayInt) Iterator() Iterator {
	return &ArrayIntIterator{
		arrayInt: a,
		index:    0,
	}
}

// ArrayIntIterator 数组迭代
type ArrayIntIterator struct {
	arrayInt ArrayInt
	index    int
}

// HasNext 是否有下一个
func (iter *ArrayIntIterator) HasNext() bool {
	return iter.index < len(iter.arrayInt)-1
}

// Next 游标加一
func (iter *ArrayIntIterator) Next() {
	iter.index++
}

// CurrentItem 获取当前元素
func (iter *ArrayIntIterator) CurrentItem() interface{} {
	return iter.arrayInt[iter.index]
}
```

iterator_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: iterator_test.go
// @Date: 2022/3/2 8:25
// @Desc: 迭代器模式测试方法
package iterator

import "testing"

func TestArrayInt_Iterator(t *testing.T) {
	data := ArrayInt{1, 3, 5, 7, 8}
	iterator := data.Iterator()
	// i 用于测试
	i := 0
	for iterator.HasNext() {
		if data[i] != iterator.CurrentItem() {
			t.Fatal()
		}
		iterator.Next()
		i++
	}
}
```

## 3.7、访问者模式（visitor）

访问者模式（Visitor Pattern）中，我们使用了一个访问者类，它改变了元素类的执行算法。通过这种方式，元素的执行算法可以随着访问者改变而改变。

主要将数据结构与数据操作分离。

访问者模式可以给一系列对象透明的添加功能，并且把相关代码封装到一个类中

![visitor](E:\material\md\img\visitor.jpeg)

visitor.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: visitor.go
// @Date: 2022/3/2 8:55
// @Desc: 访问者模式：主要将数据结构与数据操作分离。
package visitor

import "fmt"

type Customer interface {
	Accept(Visitor)
}

type Visitor interface {
	Visit(Customer)
}

type EnterpriseCustomer struct {
	name string
}

type CustomerCol struct {
	customers []Customer
}

func (c *CustomerCol) Add(customer Customer) {
	c.customers = append(c.customers, customer)
}

func (c *CustomerCol) Accept(visitor Visitor) {
	for _, customer := range c.customers {
		customer.Accept(visitor)
	}
}

func NewEnterpriseCustomer(name string) *EnterpriseCustomer {
	return &EnterpriseCustomer{
		name: name,
	}
}

func (c *EnterpriseCustomer) Accept(visitor Visitor) {
	visitor.Visit(c)
}

type IndividualCustomer struct {
	name string
}

func NewIndividualCustomer(name string) *IndividualCustomer {
	return &IndividualCustomer{
		name: name,
	}
}

func (c *IndividualCustomer) Accept(visitor Visitor) {
	visitor.Visit(c)
}

type ServiceRequestVisitor struct{}

func (*ServiceRequestVisitor) Visit(customer Customer) {
	switch c := customer.(type) {
	case *EnterpriseCustomer:
		fmt.Printf("serving enterprise customer %s\n", c.name)
	case *IndividualCustomer:
		fmt.Printf("serving individual customer %s\n", c.name)
	}
}

// only for enterprise
type AnalysisVisitor struct{}

func (*AnalysisVisitor) Visit(customer Customer) {
	switch c := customer.(type) {
	case *EnterpriseCustomer:
		fmt.Printf("analysis enterprise customer %s\n", c.name)
	}
}
```

visitor_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: visitor_test.go
// @Date: 2022/3/2 8:55
// @Desc: 访问者模式的测试方法
package visitor

func ExampleRequestVisitor() {
	c := &CustomerCol{}
	c.Add(NewEnterpriseCustomer("A company"))
	c.Add(NewEnterpriseCustomer("B company"))
	c.Add(NewIndividualCustomer("bob"))
	c.Accept(&ServiceRequestVisitor{})
	// Output:
	// serving enterprise customer A company
	// serving enterprise customer B company
	// serving individual customer bob
}

func ExampleAnalysis() {
	c := &CustomerCol{}
	c.Add(NewEnterpriseCustomer("A company"))
	c.Add(NewIndividualCustomer("bob"))
	c.Add(NewEnterpriseCustomer("B company"))
	c.Accept(&AnalysisVisitor{})
	// Output:
	// analysis enterprise customer A company
	// analysis enterprise customer B company
}
```

## 3.8、备忘录模式（memento）

备忘录模式（Memento Pattern）保存一个对象的某个状态，以便在适当的时候恢复对象。

![memento](E:\material\md\img\memento.jpeg)

memento.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: memento.go
// @Date: 2022/3/2 9:45
// @Desc: 备忘录模式：保存一个对象的某个状态，以便在适当的时候恢复对象
package memento

// InputText 用于保存数据
type InputText struct {
	content string
}

// Append 追加数据
func (in *InputText) Append(content string) {
	in.content += content
}

// GetText 获取数据
func (in *InputText) GetText() string {
	return in.content
}

// Snapshot 创建快照
func (in *InputText) Snapshot() *Snapshot {
	return &Snapshot{content: in.content}
}

// Restore 从快照中恢复
func (in *InputText) Restore(s *Snapshot) {
	in.content = s.GetText()
}

// Snapshot 快照，用于存储数据快照
// 对于快照来说，只能不能被外部（不同包）修改，只能获取数据，满足封装的特性
type Snapshot struct {
	content string
}

// GetText GetText
func (s *Snapshot) GetText() string {
	return s.content
}
```

memento_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: memento_test.go
// @Date: 2022/3/2 9:45
// @Desc: 备忘录模式:测试方法
package memento

import "testing"

func TestDemo(t *testing.T) {
	in := &InputText{}
	snapshots := []*Snapshot{}

	tests := []struct {
		input string
		want  string
	}{
		{
			input: ":list",
			want:  "",
		},
		{
			input: "hello",
			want:  "",
		},
		{
			input: ":list",
			want:  "hello",
		},
		{
			input: "world",
			want:  "",
		},
		{
			input: ":list",
			want:  "helloworld",
		},
		{
			input: ":undo",
			want:  "",
		},
		{
			input: ":list",
			want:  "hello",
		},
	}
	for _, tt := range tests {
		t.Run(tt.input, func(t *testing.T) {
			switch tt.input {
			case ":list":
				if tt.want != in.GetText() {
					t.Fatal()
				}
			case ":undo":
				in.Restore(snapshots[len(snapshots)-1])
				snapshots = snapshots[:len(snapshots)-1]
			default:
				snapshots = append(snapshots, in.Snapshot())
				in.Append(tt.input)
			}
		})
	}
}
```

## 3.9、命名模式（command）

命令模式（Command Pattern）是一种数据驱动的设计模式，请求以命令的形式包裹在对象中，并传给调用对象。调用对象寻找可以处理该命令的合适的对象，并把该命令传给相应的对象，该对象执行命令。

命令模式本质是把某个对象的方法调用封装到对象中，方便传递、存储、调用。

![command](E:\material\md\img\command.jpeg)

command.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: command.go
// @Date: 2022/3/2 11:44
// @Desc: 命令模式：把某个对象的方法调用封装到对象中，方便传递、存储、调用
package command

import "fmt"

type Command interface {
	Execute()
}

type StartCommand struct {
	mb *MotherBoard
}

func NewStartCommand(mb *MotherBoard) *StartCommand {
	return &StartCommand{
		mb: mb,
	}
}

func (c *StartCommand) Execute() {
	c.mb.Start()
}

type RebootCommand struct {
	mb *MotherBoard
}

func NewRebootCommand(mb *MotherBoard) *RebootCommand {
	return &RebootCommand{
		mb: mb,
	}
}

func (c *RebootCommand) Execute() {
	c.mb.Reboot()
}

type MotherBoard struct{}

func (*MotherBoard) Start() {
	fmt.Print("system starting\n")
}

func (*MotherBoard) Reboot() {
	fmt.Print("system rebooting\n")
}

type Box struct {
	button1 Command
	button2 Command
}

func NewBox(button1, button2 Command) *Box {
	return &Box{
		button1: button1,
		button2: button2,
	}
}

func (b *Box) PressButton1() {
	b.button1.Execute()
}

func (b *Box) PressButton2() {
	b.button2.Execute()
}
```

command_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: command_test.go
// @Date: 2022/3/2 11:44
// @Desc: 命令模式测试方法
package command

func ExampleCommand() {
	mb := &MotherBoard{}
	startCommand := NewStartCommand(mb)
	rebootCommand := NewRebootCommand(mb)

	box1 := NewBox(startCommand, rebootCommand)
	box1.PressButton1()
	box1.PressButton2()

	box2 := NewBox(rebootCommand, startCommand)
	box2.PressButton1()
	box2.PressButton2()
	// Output:
	// system starting
	// system rebooting
	// system rebooting
	// system starting
}
```

## 3.10、解释器模式（interpreter）

解释器模式（Interpreter Pattern）提供了评估语言的语法或表达式的方式，这种模式实现了一个表达式接口，该接口解释一个特定的上下文。这种模式被用在 SQL 解析、符号处理引擎等。

主要解决：对于一些固定文法构建一个解释句子的解释器。

![interpreter](E:\material\md\img\interpreter.jpeg)

interpreter.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: interpreter.go
// @Date: 2022/3/2 12:24
// @Desc: 解释器模式：对于一些固定文法构建一个解释句子的解释器
// 采用原课程的示例, 并且做了一下简化
// 假设我们现在有一个监控系统
// 现在需要实现一个告警模块，可以根据输入的告警规则来决定是否触发告警
// 告警规则支持 &&、>、< 3种运算符
// 其中 >、< 优先级比  && 更高
package interpreter

import (
	"fmt"
	"regexp"
	"strconv"
	"strings"
)

// AlertRule 告警规则
type AlertRule struct {
	expression IExpression
}

// NewAlertRule NewAlertRule
func NewAlertRule(rule string) (*AlertRule, error) {
	exp, err := NewAndExpression(rule)
	return &AlertRule{expression: exp}, err
}

// Interpret 判断告警是否触发
func (r AlertRule) Interpret(stats map[string]float64) bool {
	return r.expression.Interpret(stats)
}

// IExpression 表达式接口
type IExpression interface {
	Interpret(stats map[string]float64) bool
}

// GreaterExpression >
type GreaterExpression struct {
	key   string
	value float64
}

// Interpret Interpret
func (g GreaterExpression) Interpret(stats map[string]float64) bool {
	v, ok := stats[g.key]
	if !ok {
		return false
	}
	return v > g.value
}

// NewGreaterExpression NewGreaterExpression
func NewGreaterExpression(exp string) (*GreaterExpression, error) {
	data := regexp.MustCompile(`\s+`).Split(strings.TrimSpace(exp), -1)
	if len(data) != 3 || data[1] != ">" {
		return nil, fmt.Errorf("exp is invalid: %s", exp)
	}

	val, err := strconv.ParseFloat(data[2], 10)
	if err != nil {
		return nil, fmt.Errorf("exp is invalid: %s", exp)
	}

	return &GreaterExpression{
		key:   data[0],
		value: val,
	}, nil
}

// LessExpression <
type LessExpression struct {
	key   string
	value float64
}

// Interpret Interpret
func (g LessExpression) Interpret(stats map[string]float64) bool {
	v, ok := stats[g.key]
	if !ok {
		return false
	}
	return v < g.value
}

// NewLessExpression NewLessExpression
func NewLessExpression(exp string) (*LessExpression, error) {
	data := regexp.MustCompile(`\s+`).Split(strings.TrimSpace(exp), -1)
	if len(data) != 3 || data[1] != "<" {
		return nil, fmt.Errorf("exp is invalid: %s", exp)
	}

	val, err := strconv.ParseFloat(data[2], 10)
	if err != nil {
		return nil, fmt.Errorf("exp is invalid: %s", exp)
	}

	return &LessExpression{
		key:   data[0],
		value: val,
	}, nil
}

// AndExpression &&
type AndExpression struct {
	expressions []IExpression
}

// Interpret Interpret
func (e AndExpression) Interpret(stats map[string]float64) bool {
	for _, expression := range e.expressions {
		if !expression.Interpret(stats) {
			return false
		}
	}
	return true
}

// NewAndExpression NewAndExpression
func NewAndExpression(exp string) (*AndExpression, error) {
	exps := strings.Split(exp, "&&")
	expressions := make([]IExpression, len(exps))

	for i, e := range exps {
		var expression IExpression
		var err error

		switch {
		case strings.Contains(e, ">"):
			expression, err = NewGreaterExpression(e)
		case strings.Contains(e, "<"):
			expression, err = NewLessExpression(e)
		default:
			err = fmt.Errorf("exp is invalid: %s", exp)
		}

		if err != nil {
			return nil, err
		}

		expressions[i] = expression
	}

	return &AndExpression{expressions: expressions}, nil
}
```

interpreter_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: interpreter_test.go
// @Date: 2022/3/2 12:24
// @Desc: 解释器模式测试方法
package interpreter

import "testing"

func TestAlertRule_Interpret(t *testing.T) {
	stats := map[string]float64{
		"a": 1,
		"b": 2,
		"c": 3,
	}
	tests := []struct {
		name  string
		stats map[string]float64
		rule  string
		want  bool
	}{
		{
			name:  "case1",
			stats: stats,
			rule:  "a > 1 && b > 10 && c < 5",
			want:  false,
		},
		{
			name:  "case2",
			stats: stats,
			rule:  "a < 2 && b > 10 && c < 5",
			want:  false,
		},
		{
			name:  "case3",
			stats: stats,
			rule:  "a < 5 && b > 1 && c < 10",
			want:  false,
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			r, err := NewAlertRule(tt.rule)
			if err != nil {
				t.Fatal()
			}
			if tt.want != r.Interpret(tt.stats) {
				t.Fatal()
			}
		})
	}
}
```

## 3.11、中介者模式（mediator）

中介者模式（Mediator Pattern）是用来降低多个对象和类之间的通信复杂性。这种模式提供了一个中介类，该类通常处理不同类之间的通信，并支持松耦合，使代码易于维护。

主要解决：对象与对象之间存在大量的关联关系，这样势必会导致系统的结构变得很复杂，同时若一个对象发生改变，我们也需要跟踪与之相关联的对象，同时做出相应的处理。

![mediator](E:\material\md\img\mediator.jpeg)

mediator.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: mediator.go
// @Date: 2022/3/2 12:43
// @Desc: 中介者模式：用来降低多个对象和类之间的通信复杂性
package mediator

import (
	"fmt"
	"reflect"
)

// Input 假设这表示一个输入框
type Input string

// String String
func (i Input) String() string {
	return string(i)
}

// Selection 假设这表示一个选择框
type Selection string

// Selected 当前选中的对象
func (s Selection) Selected() string {
	return string(s)
}

// Button 假设这表示一个按钮
type Button struct {
	onClick func()
}

// SetOnClick 添加点击事件回调
func (b *Button) SetOnClick(f func()) {
	b.onClick = f
}

// IMediator 中介模式接口
type IMediator interface {
	HandleEvent(component interface{})
}

// Dialog 对话框组件
type Dialog struct {
	LoginButton         *Button
	RegButton           *Button
	Selection           *Selection
	UsernameInput       *Input
	PasswordInput       *Input
	RepeatPasswordInput *Input
}

// HandleEvent HandleEvent
func (d *Dialog) HandleEvent(component interface{}) {
	switch {
	case reflect.DeepEqual(component, d.Selection):
		if d.Selection.Selected() == "登录" {
			fmt.Println("select login")
			fmt.Printf("show: %s\n", d.UsernameInput)
			fmt.Printf("show: %s\n", d.PasswordInput)
		} else if d.Selection.Selected() == "注册" {
			fmt.Println("select register")
			fmt.Printf("show: %s\n", d.UsernameInput)
			fmt.Printf("show: %s\n", d.PasswordInput)
			fmt.Printf("show: %s\n", d.RepeatPasswordInput)
		}
		// others, 如果点击了登录按钮，注册按钮
	}
}
```

mediator_test.go

```go
// Copyright (c) 2021 hu. All rights reserved.
// @Author: stonebirdjx
// @Email: 1245863260@qq.com, g1245863260@gmail.com
// @File: mediator_test.go
// @Date: 2022/3/2 12:43
// @Desc: 中介者模式测试方法
package mediator

import "testing"

func TestDemo(t *testing.T) {
	usernameInput := Input("username input")
	passwordInput := Input("password input")
	repeatPwdInput := Input("repeat password input")

	selection := Selection("登录")
	d := &Dialog{
		Selection:           &selection,
		UsernameInput:       &usernameInput,
		PasswordInput:       &passwordInput,
		RepeatPasswordInput: &repeatPwdInput,
	}
	d.HandleEvent(&selection)

	regSelection := Selection("注册")
	d.Selection = &regSelection
	d.HandleEvent(&regSelection)
}
```

