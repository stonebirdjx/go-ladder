<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
- [创建slice](#%E5%88%9B%E5%BB%BAslice)
  - [var创建slice](#var%E5%88%9B%E5%BB%BAslice)
  - [new创建slice](#new%E5%88%9B%E5%BB%BAslice)
  - [字面量创建slice](#%E5%AD%97%E9%9D%A2%E9%87%8F%E5%88%9B%E5%BB%BAslice)
  - [下标[:]创建slice](#%E4%B8%8B%E6%A0%87%E5%88%9B%E5%BB%BAslice)
  - [make创建slice](#make%E5%88%9B%E5%BB%BAslice)
- [元素访问与赋值](#%E5%85%83%E7%B4%A0%E8%AE%BF%E9%97%AE%E4%B8%8E%E8%B5%8B%E5%80%BC)
- [slice扩容](#slice%E6%89%A9%E5%AE%B9)
  - [1.18版本之前](#118%E7%89%88%E6%9C%AC%E4%B9%8B%E5%89%8D)
  - [1.18版本之后](#118%E7%89%88%E6%9C%AC%E4%B9%8B%E5%90%8E)
- [slice 拷贝](#slice-%E6%8B%B7%E8%B4%9D)
- [注意](#%E6%B3%A8%E6%84%8F)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

`slice`又称动态数组，依托数组实现，可以方便的进行扩容、传递等，实际使用中比数组更灵活。

# 数据结构

文件路径：`src/runtime/slice.go`

```go
type slice struct {
    array unsafe.Pointer  // array 就是底层数组的地址
    len   int 						// len 切片的长度
    cap   int							// cap 切片的容量
}
```

# 创建slice

## var创建slice

```go
var s []int

func main() {
    var slice []int
    _ = slice
}
```

获取汇编代码：`go tool compile -S xxx.go`

```go
0x000e 00014 (main.go:6)        MOVQ    $0, "".slice(SP) // 存放sliceHeader的array字段
0x0016 00022 (main.go:6)        XORPS   X0, X0 // X0是一个128位寄存器，将X0寄存器清0
0x0019 00025 (main.go:6)        MOVUPS  X0, "".slice+8(SP) // 将SP + 8开始的16个字节赋值为0，即len = 0, cap = 0
```

底层并没有调用`makeslice`函数，而是简单的使用栈中的三个值,创建`nil slice`推荐这种方法

## new创建slice

```go
func main() {
    slice := *new([]int)
    _ = slice
}
```

## 字面量创建slice

```go
func main() {
    slice := []int{1, 2, 3}
    _ = slice
}
```

## 下标[:]创建slice

-  双下标形式

```Go
0 <= low <= high <= cap(baseContainer)
```

- 三下标形式

```Go
0 <= low <= high <= max <= cap(baseContainer)
```

下标low和high都可以大于len(baseContainer)。但是它们一定不能大于cap(baseContainer)。

```Go
// arr[0:3] or slice[0:3] 
```

## make创建slice

```Go
s := make([]int,8)
```

make声明slice调用底层makeslice函数。

文件路径：`src/runtime/slice.go`

```Go
func makeslice(et *_type, len, cap int) unsafe.Pointer {
        mem, overflow := math.MulUintptr(et.Size_, uintptr(cap))
        if overflow || mem > maxAlloc || len < 0 || len > cap {
                // NOTE: Produce a 'len out of range' error instead of a
                // 'cap out of range' error when someone does make([]T, bignumber).
                // 'cap out of range' is true too, but since the cap is only being
                // supplied implicitly, saying len is clearer.
                // See golang.org/issue/4085.
                mem, overflow := math.MulUintptr(et.Size_, uintptr(len))
                if overflow || mem > maxAlloc || len < 0 {
                        panicmakeslicelen()
                }
                panicmakeslicecap()
        }

        return mallocgc(mem, et, true)
}
```

基本逻辑就是根据容量申请一块内存。

`mallocgc(mem, et, true)` 分配内存

# 元素访问与赋值

与数组一样，先由`Preindex` 寻址，在 `load` 或者`store`

可以获取汇编代码查看指令

# slice扩容

## 1.18版本之前

使用 `append` 关键字向切片中追加元素也是常见的切片操作

```Go
slice = append(slice,elem1,elem2)
slice = append(slice,anotherSlice...)
```

切片是一个动态数组，当容量不足时会自动扩容，自动扩容实际上就是调用`growslice()`函数

文件路径：`src/runtime/slice.go`

```Go
func growslice(et *_type, old slice, cap int) slice {
      newcap := old.cap
      doublecap := newcap + newcap
      if cap > doublecap {
        newcap = cap
      } else {
        if old.len < 1024 {
          newcap = doublecap
        } else {
          for 0 < newcap && newcap < cap {
            newcap += newcap / 4
          }
          if newcap <= 0 {
            newcap = cap
          }
        }
      }
      ...
      return slice{p, newLen, newcap}
}
```

`growslice` 函数返回的是一个新的切片。

**内存对齐的情况下**：

- 如果小于1024，就正常扩容两倍。
- 大于等于1024，就循环扩容1.25倍

**如果内存没对齐**，还需要根据切片中的元素大小对齐内存，当数组中元素所占的字节大小为`1`、`8` 或者`2 `的倍数时，会先去查表。

```Go
package main

import "fmt"

// 单个append
// 1024 * 1.25 = 1280
// 1024 * 1.25 * 4 = 5120, 查表(sizeclass.go) 5376/4 = 1344 (真时容量)
// 1024 * 1.25 * 8 = 10240,查表(sizeclass.go)  10240/8 = 1280

// 批量append的先检查容量 查表
// 1024 * 8 = 8200  查表 9472 / 8= 1184
func main() {
        var int32s []int32
        for i := 0; i < 1025; i++ {
                int32s = append(int32s, int32(i))
                fmt.Println(len(int32s), cap(int32s))
        }

        var arr1 = make([]int64,1024,1280)
        var arr2 []int64
        arr2 = append(arr2,arr1...)
        fmt.Println(len(arr2), cap(arr2)) // 1024 1184（并不是1280）
}
```

## 1.18版本之后

更平滑一些。`growslice`

文件路径：`src/runtime/slice.go`

设定了一个值为256的threshold，以256为临界点；超过256，不再是每次扩容1/4，而是每次增加（旧容量+3*256）/4

```Go
const threshold = 256
if oldCap < threshold {
        newcap = doublecap
} else {
        // Check 0 < newcap to detect overflow
        // and prevent an infinite loop.
        for 0 < newcap && newcap < newLen {
                // Transition from growing 2x for small slices
                // to growing 1.25x for large slices. This formula
                // gives a smooth-ish transition between the two.
                newcap += (newcap + 3*threshold) / 4
        }
        // Set newcap to the requested cap when
        // the newcap calculation overflowed.
        if newcap <= 0 {
                newcap = newLen
        }
}
```

# slice 拷贝

使用`copy(a, b)` 的形式对切片进行拷贝,会调用`slicecopy`

文件路径：`src/runtime/slice.go`

```Go
// slicecopy is used to copy from a string or slice of pointerless elements into a slice.
func slicecopy(toPtr unsafe.Pointer, toLen int, fromPtr unsafe.Pointer, fromLen int, width uintptr) int {
        if fromLen == 0 || toLen == 0 {
                return 0
        }

        n := fromLen
        if toLen < n {
                n = toLen
        }

        if width == 0 {
                return n
        }
        ...
        return n
 }
```

copy 的长度为最短切片长度，不是容量，如dst的长度为0，容量大于src的长度，copy长度依然为0。

# 注意

虽然编译期间可以检查出很多错误，但是在创建切片的过程中如果发生了以下错误会直接触发运行时错误并崩溃：

- 内存空间的大小发生了溢出；
- 申请的内存大于最大可分配的内存；
- 传入的长度小于 0 或者长度大于容量；