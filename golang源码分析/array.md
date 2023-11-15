<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [Array结构](#array%E7%BB%93%E6%9E%84)
- [初始化数组](#%E5%88%9D%E5%A7%8B%E5%8C%96%E6%95%B0%E7%BB%84)
- [访问和赋值](#%E8%AE%BF%E9%97%AE%E5%92%8C%E8%B5%8B%E5%80%BC)
- [For range 数组](#for-range-%E6%95%B0%E7%BB%84)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

<span style="color:red">数组是单一内存块，并无附加结构</span>

- 数组的长度必须是**非负整型常量**表达式
- 如果元素类型支持`==` 或者`!=`，那么数组也支持
- 语法糖[...]T, 仅允许第一纬度数组
- `len`和`cap`都能返回一维长度
- 多维数组指针依旧是连续的内存地址

# Array结构

文件定义路径：`src/cmd/compile/internal/types/type.go`

```go
// Array contains Type fields specific to array types.
type Array struct {
	Elem  *Type // element type 元素类型
	Bound int64 // number of elements; <0 if unknown yet  元素数量
}
```

# 初始化数组

Go 语言的数组有两种不同的创建方式，一种是显式的指定数组大小，另一种是使用 `[...]T` 声明数组

```go
arr1 := [3]int{1, 2, 3}
arr2 := [...]int{1, 2, 3}
```

无论哪种初始化都会去调`NewArray`

```go
// NewArray returns a new fixed-length array Type.
func NewArray(elem *Type, bound int64) *Type {
	if bound < 0 {
		base.Fatalf("NewArray: invalid bound %v", bound)
	}
	t := newType(TARRAY)
	t.extra = &Array{Elem: elem, Bound: bound}
	if elem.HasShape() {
		t.SetHasShape(true)
	}
	return t
}
```

# 访问和赋值

获取目标地址，再通过 `PtrIndex` 获取目标元素的地址, 再`Load` 或者`Store`

数组访问越界是非常严重的错误，Go 语言中可以在编译期间的静态类型检查判断数组越界，

1. 访问数组的索引是非整数时，报错 “non-integer array index %v”；
2. 访问数组的索引是负数时，报错 “invalid array index %v (index must be non-negative)"；
3. 访问数组的索引越界时，报错 “invalid array index %v (out of bounds for %d-element array)"；

# For range 数组

```go
l := []int{1,2,3}
for idx,v := range l {
  if idx == O {
    l[2] = 5
  }
  
  if idx == 2 {
    fmt.Println(v) // 3
  }
}
```

- 遍历的数组的副本，取得是拷贝之前的值，底层数据不共享。

