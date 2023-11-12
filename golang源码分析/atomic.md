<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [原子操作](#%E5%8E%9F%E5%AD%90%E6%93%8D%E4%BD%9C)
  - [swap operation](#swap-operation)
  - [CompareAndSwap(CAS)](#compareandswapcas)
  - [add operation](#add-operation)
  - [load and store operations](#load-and-store-operations)
- [CAS 与 ABA 问题](#cas-%E4%B8%8E-aba-%E9%97%AE%E9%A2%98)
- [SwapT](#swapt)
- [CompareAndSwapT](#compareandswapt)
- [LoadT](#loadt)
- [StoreT](#storet)
- [atomic.Value](#atomicvalue)
  - [Load](#load)
  - [Store](#store)
- [总结](#%E6%80%BB%E7%BB%93)
  - [汇编](#%E6%B1%87%E7%BC%96)
- [原子操作与互斥锁的区别](#%E5%8E%9F%E5%AD%90%E6%93%8D%E4%BD%9C%E4%B8%8E%E4%BA%92%E6%96%A5%E9%94%81%E7%9A%84%E5%8C%BA%E5%88%AB)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

atomic包提供了用于实现同步机制的底层原子内存原语,但是这个是一个 low level 的库 ，开发过程中还是使用sync来控制。在一些基础库，或者是需要极致性能的地方用上这个还是很爽的，但是使用的过程中一定要小心，不然还是会容易出 bug。

使用这些功能需要非常小心。除了特殊的底层应用程序外，最好使用通道或sync包来进行同步。**通过通信来共享内存；不要通过共享内存来通信**

# 原子操作

原子操作即是进行过程中不能被中断的操作，针对某个值的原子操作在被进行的过程中，CPU绝不会再去进行其他的针对该值的操作。为了实现这样的严谨性，原子操作仅会由一个独立的CPU指令代表和完成。

原子操作是无锁的，常常直接通过CPU指令直接实现。

来atomic操作通过JMP操作跳到`runtime/internal/atomic`目录下面的**汇编实现**。

##  swap operation

类似于

```go
old = *addr
*addr = new
return old
```

## CompareAndSwap(CAS)

```go
if *addr == old {
		*addr = new
		return true
}
return false
```

## add operation

```go
*addr += delta
return *addr
```

## load and store operations

```go
*addr = val
return *addr
```

# CAS 与 ABA 问题

CAS，Compare-and-Swap，一般我们会用它是实现乐观锁，自旋锁，伪代码如下

```scss
while(!swapped) {
    swapped = CAS(V, E, N)
    sleep(1)
}
```

所谓自旋锁，Spin Lock，就是循环等待，直到获取到锁。不过乐观锁其实也会带来如下问题

- 自旋开销大
- ABA 问题

自旋开销大，这个看伪代码就能明白。那么 ABA 问题是什么呢？假设有线程 X，Y 和变量 N

1. 当线程 X 用 CAS 将变量 N 的值从 A 改为 B 时
2. 此时线程 Y 将变量 N 的值改为 A
3. 这时候线程 X 的 CAS 判断变量 N 的值没有发生过变化，不符合预期

这就是 ABA 问题，解决方法是可以增加 version 或 timestamp，每次变量更新时 version++，这也就将 `A->B->A` 变成了 `1A->2B->3A`

# SwapT

```go
// 原子性的将新值保存到*addr并返回旧值。
func SwapInt32(addr *int32, new int32) (old int32)

// 源码：
func Xchg(ptr *uint32, new uint32) uint32 {
    old := *ptr
    *ptr = new
    return old
}
```

# CompareAndSwapT

```go
// 原子性的比较*addr和old，如果相同则将new赋值给*addr并返回真。
func CompareAndSwapInt32(addr *int32, old, new int32) (swapped bool)

// 源码：
func Cas(ptr *uint32, old, new uint32) bool {
    if *ptr == old {
        *ptr = new
        return  true
    }

    return  false
}
```

# LoadT

```go
// 原子性的获取*addr的值
func LoadInt32(addr *int32) (val int32)

// 源码
func Load(ptr *uint32) uint32 {
    return *ptr
}
```

# StoreT

```go
// 原子性的将val的值保存到*addr
func StoreInt32(addr *int32, val int32)

// 源码
func Store(ptr *uint32, val uint32) {
    *ptr = val
}
```

# atomic.Value

```go
// A Value must not be copied after first use.
type Value struct {
	v interface{}
}

```

里面主要是包含了两个方法

- `v.Store(c)` - 写操作，将原始的变量c存放到一个`atomic.Value`类型的v里。
- `c = v.Load()` - 读操作，从线程安全的v中读取上一步存放的内容。

## Load

```go
// ifaceWords is interface{} internal representation.
type ifaceWords struct {
	// 类型
	typ unsafe.Pointer
	// 数据
	data unsafe.Pointer
}

// 如果没Store将返回nil
func (v *Value) Load() (x interface{}) {
	// 获得 interface 结构的指针
	vp := (*ifaceWords)(unsafe.Pointer(v))
	// 获取类型
	typ := LoadPointer(&vp.typ)
	// 判断，第一次写入还没有开始，或者还没完成，返回nil
	if typ == nil || uintptr(typ) == ^uintptr(0) {
		// First store not yet completed.
		return nil
	}
	// 获得存储值的实际数据
	data := LoadPointer(&vp.data)
	// 将复制得到的 typ 和 data 给到 x
	xp := (*ifaceWords)(unsafe.Pointer(&x))
	xp.typ = typ
	xp.data = data
	return
}
```

## Store

```go
// 如果两次Store的类型不同将会panic
// 如果写入nil，也会panic
func (v *Value) Store(x interface{}) {
	// value不能为nil
	if x == nil {
		panic("sync/atomic: store of nil value into Value")
	}
	// Value存储的指针
	vp := (*ifaceWords)(unsafe.Pointer(v))
	// 写入value的目标指针x
	xp := (*ifaceWords)(unsafe.Pointer(&x))
	for {
		typ := LoadPointer(&vp.typ)
		// 第一次Store
		if typ == nil {
			// 禁止抢占当前 Goroutine 来确保存储顺利完成
			runtime_procPin()
			// 如果typ为nil，设置一个标志位，宣告正在有人操作此值
			if !CompareAndSwapPointer(&vp.typ, nil, unsafe.Pointer(^uintptr(0))) {
				// 如果没有成功，取消不可抢占，下次再试
				runtime_procUnpin()
				continue
			}
			// 如果标志位设置成功，说明其他人都不会向 interface{} 中写入数据
			// 这点细品很巧妙，先写数据，在写类型，应该类型设置了不可写入的表示位
			// 写入数据
			StorePointer(&vp.data, xp.data)
			// 写入类型
			StorePointer(&vp.typ, xp.typ)
			// 存储成功，取消不可抢占，，直接返回
			runtime_procUnpin()
			return
		}
		// 已经有值写入了，或者有正在写入的Goroutine

		// 有其他 Goroutine 正在对 v 进行写操作
		if uintptr(typ) == ^uintptr(0) {
			continue
		}

		// 如果本次存入的类型与前次存储的类型不同
		if typ != xp.typ {
			panic("sync/atomic: store of inconsistently typed value into Value")
		}
		// 类型已经写入，直接保存数据
		StorePointer(&vp.data, xp.data)
		return
	}
}
```

# 总结

## 汇编

```go
func CompareAndSwapInt32(addr *int32, old, new int32) (swapped bool)

TEXT ·CompareAndSwapInt32(SB),NOSPLIT,$0
    JMP    runtime∕internal∕atomic·Cas(SB)

TEXT runtime∕internal∕atomic·Cas(SB),NOSPLIT,$0-17
    MOVQ    ptr+0(FP), BX
    MOVL    old+8(FP), AX
    MOVL    new+12(FP), CX
    LOCK
    CMPXCHGL    CX, 0(BX)
    SETEQ    ret+16(FP)
    RET
```

- **Swap** : `XCHGQ`
- **CAS** : `LOCK`+ `CMPXCHGQ`
- **Add** : `LOCK` + `XADDQ`
- **Load** : `MOVQ`
- **Store** : `XCHGQ`
- **Pointer** : 以上指令结合GC 调度

Swap和Store会对共享数据做修改，但是为啥它们没有加LOCK?

- 在实现`Swap`和`Store`方法时，其实不管是否存在`LOCK`前缀，在交换操作期间（`XCHGQ`）将自动实现CPU的锁定协议。

# 原子操作与互斥锁的区别

首先atomic操作的优势是更轻量，比如CAS可以在不形成临界区和创建互斥量的情况下完成并发安全的值替换操作。这可以大大的减少同步对程序性能的损耗。下面是几点区别：

- 互斥锁是一种数据结构，用来让一个线程执行程序的关键部分，完成互斥的多个操作
- 原子操作是无锁的，常常直接通过CPU指令直接实现
- 原子操作中的cas趋于乐观锁，CAS操作并不那么容易成功，需要判断，然后尝试处理
- 可以把互斥锁理解为悲观锁，共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程