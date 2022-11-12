<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [为什么字符串不允许修改？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%8D%E5%85%81%E8%AE%B8%E4%BF%AE%E6%94%B9)
- [[]byte转换成string一定会拷贝内存吗？](#byte%E8%BD%AC%E6%8D%A2%E6%88%90string%E4%B8%80%E5%AE%9A%E4%BC%9A%E6%8B%B7%E8%B4%9D%E5%86%85%E5%AD%98%E5%90%97)
- [Slice扩容原理](#slice%E6%89%A9%E5%AE%B9%E5%8E%9F%E7%90%86)
- [slice copy](#slice-copy)
- [为什么重复解锁要panic](#%E4%B8%BA%E4%BB%80%E4%B9%88%E9%87%8D%E5%A4%8D%E8%A7%A3%E9%94%81%E8%A6%81panic)
- [写操作是如何阻止写操作的](#%E5%86%99%E6%93%8D%E4%BD%9C%E6%98%AF%E5%A6%82%E4%BD%95%E9%98%BB%E6%AD%A2%E5%86%99%E6%93%8D%E4%BD%9C%E7%9A%84)
- [写操作是如何阻止读操作的](#%E5%86%99%E6%93%8D%E4%BD%9C%E6%98%AF%E5%A6%82%E4%BD%95%E9%98%BB%E6%AD%A2%E8%AF%BB%E6%93%8D%E4%BD%9C%E7%9A%84)
- [读操作是如何阻止写操作的](#%E8%AF%BB%E6%93%8D%E4%BD%9C%E6%98%AF%E5%A6%82%E4%BD%95%E9%98%BB%E6%AD%A2%E5%86%99%E6%93%8D%E4%BD%9C%E7%9A%84)
- [为什么写锁定不会被饿死](#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%86%99%E9%94%81%E5%AE%9A%E4%B8%8D%E4%BC%9A%E8%A2%AB%E9%A5%BF%E6%AD%BB)
- [函数传递指针真的比传值效率高吗](#%E5%87%BD%E6%95%B0%E4%BC%A0%E9%80%92%E6%8C%87%E9%92%88%E7%9C%9F%E7%9A%84%E6%AF%94%E4%BC%A0%E5%80%BC%E6%95%88%E7%8E%87%E9%AB%98%E5%90%97)

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

# slice copy 

```go
copy(dst []type, src []type)
// 底层是调用 runtime.memmove
```

copy 的长度是最短切片长度（不是容量），比如dst的长度为0，容量大于src长度，也是copy不了的。

# 为什么重复解锁要panic

仔细想想Unlock的逻辑就可以理解，这实际上很难做到。Unlock过程分为将Locked置为0，然后判断Waiter值，如果值>0，则释放信号量。

如果多次Unlock()，那么可能每次都释放一个信号量，这样会唤醒多个协程，多个协程唤醒后会继续在Lock()的逻辑里抢锁，势必会增加Lock()实现的复杂度，也会引起不必要的协程切换。

> 加锁和解锁最好出现在同一个层次的代码块中，比如同一个函数。重复解锁会引起panic，应避免这种操作的可能性。
>
> 加锁后立即使用defer对其解锁，可以有效的避免死锁（但要尽量保持锁的粒度小）。

# 写操作是如何阻止写操作的

读写锁包含一个互斥锁(Mutex)，写锁定必须要先获取该互斥锁，如果互斥锁已被协程A获取（或者协程A在阻塞等待读结束），意味着协程A获取了互斥锁，那么协程B只能阻塞等待该互斥锁。

所以，写操作依赖互斥锁阻止其他的写操作。

> RWMutex 结构体中 写操作 是一个mutex

# 写操作是如何阻止读操作的

这个是读写锁实现中最精华的技巧。

RWMutex.readerCount是个整型值，用于表示读者数量，不考虑写操作的情况下，每次读锁定将该值+1，每次解除读锁定将该值-1，所以readerCount取值为[0, N]，N为读者个数，实际上最大可支持2^30个并发读者。

当写锁定进行时，会先将readerCount减去2^30，从而readerCount变成了负值，此时再有读锁定到来时检测到readerCount为负值，便知道有写操作在进行，只好阻塞等待。

> 所以，写操作将readerCount变成负值来阻止读操作的。

# 读操作是如何阻止写操作的

读锁定会先将RWMutext.readerCount加1，此时写操作到来时发现读者数量不为0，会阻塞等待所有读操作结束。

> 所以，读操作通过readerCount来将来阻止写操作的。

# 为什么写锁定不会被饿死

写操作等待期间可能还有新的读操作持续到来，如果写操作等待所有读操作结束，很可能被饿死。然而，通过RWMutex.readerWait可完美解决这个问题。

写操作到来时，会把RWMutex.readerCount值拷贝到RWMutex.readerWait中，用于标记排在写操作前面的读者个数。

前面的读操作结束后，除了会递减RWMutex.readerCount，还会递减RWMutex.readerWait值，当RWMutex.readerWait值变为0时唤醒写操作。

# 函数传递指针真的比传值效率高吗

传递指针可以减少底层值的拷贝，可以提高效率，但是如果拷贝的数据量小，由于指针传递会产生逃逸，可能会使用堆，也可能会增加GC的负担，所以传递指针不一定是高效的。

> golang都是值传递，引用类型或者指针也只是加了一层引用而已
