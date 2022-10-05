<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [泛型原理](#%E6%B3%9B%E5%9E%8B%E5%8E%9F%E7%90%86)
  - [:point_right:性能问题：](#point_right%E6%80%A7%E8%83%BD%E9%97%AE%E9%A2%98)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 泛型原理

泛型的实现方式，通常有： 

- 模版（stenciling）：为每次调用生成代码实例，即便类型参数相同。 
- 字典（dictionaries）：单份代码实例，以字典传递类型参数信息。

模版方式性能最佳，但编译时间较长，且生成文件较大。 

字典方式代码最少，但复杂度较高，且性能最差。 

> Go 泛型实现介于两者之间，一种称作 “GCShape stenciling with Dictionaries” 概念。 
>
> 任意指针类型，或具有相同底层类型（underlying type）的类型，属于同一 GCShape 组。

:point_right:每一个唯一的shape会产生一份代码，每份代码携带一个字典，包含了实例化类型的信息（带了shape、dict的标志）。

```go
type Tester interface {
    *A | *B
    test()
}

func test[T Tester](a T) {
	a.test()
}
// -----------------------------
func main() {
    var a A = 1
    var b B = 2
    test(&a)
    test(&b)
}
```

编译后可以使用 `go tool objdump -S -s "main\.main" ./file` 反编译查看汇编情况

```bash
go build -gcflags "-l"
go tool objdump -S -s "main\.main" ./test
TEXT main.main(SB)
func main() {
test(1)
 0x455214 LEAQ main..dict.test[int](SB), AX
 0x45521b MOVL $0x1, BX
 0x455220 CALL main.test[go.shape.int_0](SB)

test(X(2))
 0x455225 LEAQ main..dict.test[main.X.1](SB), AX ; 字典不同。
 0x45522c MOVL $0x2, BX
 0x455231 CALL main.test[go.shape.int_0](SB) ; 函数相同。

test("abc")
 0x455236 LEAQ main..dict.test[string](SB), AX
 0x45523d LEAQ 0xc84c(IP), BX
 0x455244 MOVL $0x3, CX
 0x455249 CALL main.test[go.shape.string_0](SB) ; 新实例。
}
```

> 指针版本存在内存逃逸

## :point_right:性能问题：

1.18 的泛型实现中，大多数只会让代码运行速度变得更慢,可能引发 动态调用、无法内联、内存逃逸 等性能问题

注意使用场景

- 要在数据结构中使用泛型，这也是泛型目前最理想的用例。能实现实现未装箱类型，就能让这些数据结构更易用、运行更快。（以往使用 interface{}实现的泛型）
- **在任何情况下，都不要将接口传递给泛型函数**。如果对性能比较敏感，请保证只在泛型中使用指针、不用接

- 不要重写基于接口的 API 来使用泛型。受制于当前实现，只要继续使用接口，所有使用非空接口的代码都将更简单、并带来更可预测的性能。在方法调用方面，泛型会将指针转化为两次间接接口，再把接口转换成……总之，特别麻烦、也毫无必要。

