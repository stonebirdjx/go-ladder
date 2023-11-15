# [unsafe](https://pkg.go.dev/unsafe)

unsafe 包包含绕过 Go 程序类型安全的操作。导入不安全的包可能是不可移植的，并且不受 Go 1 兼容性指南的保护。

:point_right:大致步骤：

1. 普通 pointer 转unsafe.Pointer
2. unsafe.Pointer 转 uintptr （如需）
3. uintptr 做运算操作（如需）
4. uintptr 转 unsafe.Pointer（如需）
5. unsafe.Pointer 转 普通 pointer

## type ArbitraryType int

## type IntegerType int

## :point_right:type Pointer *ArbitraryType

```Go
func Float64bits(f float64) uint64 {
        return *(*uint64)(unsafe.Pointer(&f))
}

p = unsafe.Pointer(uintptr(p) + offset)

// equivalent to f := unsafe.Pointer(&s.f)
f := unsafe.Pointer(uintptr(unsafe.Pointer(&s)) + unsafe.Offsetof(s.f))
// equivalent to e := unsafe.Pointer(&x[i])
e := unsafe.Pointer(uintptr(unsafe.Pointer(&x[0])) + i*unsafe.Sizeof(x[0]))

// INVALID: end points outside allocated space.
var s thing
end = unsafe.Pointer(uintptr(unsafe.Pointer(&s)) + unsafe.Sizeof(s))
// INVALID: end points outside allocated space.
b := make([]byte, n)
end = unsafe.Pointer(uintptr(unsafe.Pointer(&b[0])) + uintptr(n))

// INVALID: uintptr cannot be stored in variable
// before conversion back to Pointer.
u := uintptr(p)
p = unsafe.Pointer(u + offset)
```

- A pointer value of any type can be converted to a Pointer.
- A Pointer can be converted to a pointer value of any type.
- A uintptr can be converted to a Pointer.
- A Pointer can be converted to a uintptr.

Example：

```Go
package main

import (
        "fmt"
        "unsafe"
)

type Person struct {
        name   string
        age    int
        salary float64
}

func main() {
        p := Person{
                name:   "Alice",
                age:    30,
                salary: 5000.0,
        }

        // 将 Person 类型的指针转换为 unsafe.Pointer 类型
        ptr := unsafe.Pointer(&p)

        // 访问未导出的 name 字段
        namePtr := (*string)(unsafe.Pointer(uintptr(ptr) + unsafe.Offsetof(p.name)))
        name := *namePtr

        // 访问未导出的 age 字段
        agePtr := (*int)(unsafe.Pointer(uintptr(ptr) + unsafe.Offsetof(p.age)))
        age := *agePtr

        // 访问未导出的 salary 字段
        salaryPtr := (*float64)(unsafe.Pointer(uintptr(ptr) + unsafe.Offsetof(p.salary)))
        salary := *salaryPtr

        fmt.Println("Name:", name)
        fmt.Println("Age:", age)
        fmt.Println("Salary:", salary)
}
```

## :point_right:func Alignof

```Go
func Alignof(x ArbitraryType) uintptr
```

- Alignof 接受任何类型的表达式 x 并返回假设变量 v 所需的对齐方式，就好像 v 是通过 var v = x 声明的一样。 它是使 v 的地址始终为零 mod m 的最大值 m。 它与reflect.TypeOf(x).Align()返回的值相同。

Example:

```Go
package main

import (
        "fmt"
        "unsafe"
)

type Person struct {
        name   string
        age    int
        salary float64
}

func main() {
        p := Person{
                name:   "Alice",
                age:    30,
                salary: 5000.0,
        }

        fmt.Println(unsafe.Alignof(p))
}
```

## :point_right:func Offsetof

```Go
func Offsetof(x ArbitraryType) uintptr
```

Offsetof 返回 x 表示的字段在结构体中的偏移量，该字段必须采用 structValue.field 形式。 换句话说，它返回结构体开头和字段开头之间的字节数。

Example:

```Go
agePtr := (*int)(unsafe.Pointer(uintptr(ptr) + unsafe.Offsetof(p.age)))
```

## :point_right:func Sizeof

```Go
func Sizeof(x ArbitraryType) uintptr
```

Sizeof 接受任何类型的表达式 x 并返回假设变量 v 的大小（以字节为单位），就好像 v 是通过 var v = x 声明的一样。 该大小不包括 x 可能引用的任何内存。

Example:

```Go
 fmt.Println(unsafe.Sizeof(Args{}))
```

## func String

```Go
func String(ptr *byte, len IntegerType) string
```

- String 返回一个字符串值，其底层字节从 ptr 开始，长度为 len。
- len 参数必须是整数类型或无类型常量。 常量 len 参数必须是非负的并且可以用 int 类型的值表示； 如果它是无类型常量，则其类型为 int。 在运行时，如果 len 为负，或者 ptr 为 nil 并且 len 不为零，则会发生运行时恐慌。
- 由于 Go 字符串是不可变的，因此传递给 String 的字节之后不得修改。

Example:

```Go
b := byte(97)
fmt.Println(unsafe.String(&b, 1))
// a
```

## func StringData

```Go
func StringData(str string) *byte
```

StringData 返回指向 str 底层字节的指针。 对于空字符串，返回值未指定，并且可能为 nil。

由于 Go 字符串是不可变的，因此 StringData 返回的字节不得修改。

Example:

```Go
package main

import (
        "fmt"
        "unsafe"
)

func main() {
        s := "Hello, world!"
        bs := unsafe.StringData(s)

        us := unsafe.String(bs, 5)
        fmt.Println(us)
}
// Hello
```

