<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [为什么字符串不允许修改？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%8D%E5%85%81%E8%AE%B8%E4%BF%AE%E6%94%B9)
- [[]byte转换成string一定会拷贝内存吗？](#byte%E8%BD%AC%E6%8D%A2%E6%88%90string%E4%B8%80%E5%AE%9A%E4%BC%9A%E6%8B%B7%E8%B4%9D%E5%86%85%E5%AD%98%E5%90%97)
- [Slice扩容原理](#slice%E6%89%A9%E5%AE%B9%E5%8E%9F%E7%90%86)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 为什么字符串不允许修改？

但Go的实现中，string不包含内存空间，只有一个内存的指针，这样做的好处是string变得非常轻量，可以很方便的进行传递而不用担心内存拷贝。

因为string通常指向字符串字面量，而字符串字面量存储位置是只读段（数据段），而不是堆或栈上，所以才有了string不可修改的约定。

# []byte转换成string一定会拷贝内存吗？

赋值情况下使用unsafe包可以不内存拷贝

```go
func main() {
    a :="aaa"
    ssh := *(*reflect.StringHeader)(unsafe.Pointer(&a))
    b := *(*[]byte)(unsafe.Pointer(&ssh))  
    fmt.Printf("%v",b)
}
```

不赋值（临时使用的情况下）不会内存拷贝

- 字符串拼接，如"<" + "string(b)" + ">"；
- 字符串比较：string(b) == "foo"

因为是临时把byte切片转换成string，也就避免了因byte切片同容改成而导致string引用失败的情况，所以此时可以不必拷贝内存新建一个string。

> string 转 []byte 也是如此

# Slice扩容原理

基础类型会先内存对齐、检查容量，然后查表(sizeclass.go) 。

分单个appned和 整个切片append。

```go
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

> 所以小于1024是双倍扩容，大于1024是1.25倍扩容描述是不准确，只有当内存对齐的情况下才是。
