<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [知识点](#%E7%9F%A5%E8%AF%86%E7%82%B9)
  - [信号量](#%E4%BF%A1%E5%8F%B7%E9%87%8F)
- [RWMutex数据结构](#rwmutex%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
- [读锁](#%E8%AF%BB%E9%94%81)
  - [RLock](#rlock)
  - [RUnlock](#runlock)
- [写锁](#%E5%86%99%E9%94%81)
  - [Lock](#lock)
  - [Unlock](#unlock)
- [问题要论](#%E9%97%AE%E9%A2%98%E8%A6%81%E8%AE%BA)
  - [写操作是如何阻止写操作的](#%E5%86%99%E6%93%8D%E4%BD%9C%E6%98%AF%E5%A6%82%E4%BD%95%E9%98%BB%E6%AD%A2%E5%86%99%E6%93%8D%E4%BD%9C%E7%9A%84)
  - [写操作是如何阻止读操作的](#%E5%86%99%E6%93%8D%E4%BD%9C%E6%98%AF%E5%A6%82%E4%BD%95%E9%98%BB%E6%AD%A2%E8%AF%BB%E6%93%8D%E4%BD%9C%E7%9A%84)
  - [读操作是如何阻止写操作的](#%E8%AF%BB%E6%93%8D%E4%BD%9C%E6%98%AF%E5%A6%82%E4%BD%95%E9%98%BB%E6%AD%A2%E5%86%99%E6%93%8D%E4%BD%9C%E7%9A%84)
  - [为什么写锁定不会被饿死](#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%86%99%E9%94%81%E5%AE%9A%E4%B8%8D%E4%BC%9A%E8%A2%AB%E9%A5%BF%E6%AD%BB)
  - [两个读锁之间穿插了一个写锁](#%E4%B8%A4%E4%B8%AA%E8%AF%BB%E9%94%81%E4%B9%8B%E9%97%B4%E7%A9%BF%E6%8F%92%E4%BA%86%E4%B8%80%E4%B8%AA%E5%86%99%E9%94%81)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

`RWMutex` 建立在`Mutex` 之上 具有**写操作优先**的特点，写操作发生时，仅允许正在执行的读操作执行，后续的读操作都会被阻塞。

`RWMutex`常用于大量并发读，少量并发写的场景；比如微服务配置更新、交易路由缓存等场景。相对于`Mutex`互斥锁，`RWMutex`读写锁具有更好的读性能。

实现读写锁需要解决如下几个问题：

1. 写锁需要阻塞写锁：一个协程拥有写锁时，其他协程写锁定需要阻塞
2. 写锁需要阻塞读锁：一个协程拥有写锁时，其他协程读锁定需要阻塞
3. 读锁需要阻塞写锁：一个协程拥有读锁时，其他协程写锁定需要阻塞
4. 读锁不能阻塞读锁：一个协程拥有读锁时，其他协程也可以拥有读锁

> 它不限制资源的并发读，但是读写、写写操作无法并行执行。第一次使用后不得复制RWMutex。

# 知识点

## 信号量

信号量其本质是一个整数，在解决多种同步问题时很有用。并关联两个操作：

- 申请acquire（也称为 wait、decrement 或 P 操作）
  - acquire操作将信号量减 1，如果结果值为负则线程阻塞，且直到其他线程进行了信号量累加为正数才能恢复。如结果为正数，线程则继续执行。

- 释放release（也称 signal、increment 或 V 操作）
  - release操作将信号量加 1，如存在被阻塞的线程，此时他们中的一个线程将解除阻塞。

# RWMutex数据结构

```go
type RWMutex struct {
	w           Mutex  // 用于控制多个写锁，获得写锁首先要获取该锁，如果有一个写锁在进行，那么再到来的写锁将会阻塞于此
	writerSem   uint32 // 写阻塞等待的信号量，最后一个读者释放锁时会释放信号量
	readerSem   uint32 // 读阻塞的协程等待的信号量，持有写锁的协程释放锁后会释放信号量
	readerCount int32  // 记录读者个数
	readerWait  int32  // 记录写阻塞时读者个数
}

const rwmutexMaxReaders = 1 << 30 //支持最多2^30个读
```

# 读锁

## RLock

```go
const rwmutexMaxReaders = 1 << 30

func (rw *RWMutex) RLock() {
    // 竞争检测代码，不看
	if race.Enabled {
		_ = rw.w.state
		race.Disable()
	}
    // 写者在写的时候会将readerCount减去rwmutexMaxReaders，
    // 表示如果readerCount小于0说明还有写者在写
    // 如果(readerCount+1) < 0 表示至少有一个写者在写，新来的读者阻塞
	if atomic.AddInt32(&rw.readerCount, 1) < 0 {
		// 原语：将这个读者放到读锁的等待队列的尾部
		runtime_SemacquireMutex(&rw.readerSem, false, 0)
	}
	if race.Enabled {
		race.Enable()
		race.Acquire(unsafe.Pointer(&rw.readerSem))
	}
}
```

- 加读锁很简单，就是将readerCount数量+1
- 当readerCount + 1之后的值 < 0的时候，如果结果小于0说明有写锁存在，需要阻塞读锁；
- 通过`runtime_SemacquireMutex`将当前的读锁加入到阻塞队列的尾部。

## RUnlock

```go
// 减少读操作计数，即readerCount--
// 唤醒等待写操作的协程（如果有的话）
func (rw *RWMutex) RUnlock() {
	// 是否开启检测race
	if race.Enabled {
		_ = rw.w.state
		race.ReleaseMerge(unsafe.Pointer(&rw.writerSem))
		race.Disable()
	}
	// 首先通过atomic的原子性使readerCount-1
	// 1.若readerCount大于0, 证明当前还有读锁, 直接结束本次操作
	// 2.若readerCount小于0, 证明已经没有读锁, 但是还有因为读锁被阻塞的写锁存在
	if r := atomic.AddInt32(&rw.readerCount, -1); r < 0 {
		// 尝试唤醒被阻塞的写锁
		rw.rUnlockSlow(r)
	}
	// 是否开启检测race
	if race.Enabled {
		race.Enable()
	}
}

func (rw *RWMutex) rUnlockSlow(r int32) {
	// 判断RUnlock被过多使用了
	if r+1 == 0 || r+1 == -rwmutexMaxReaders {
		race.Enable()
		throw("sync: RUnlock of unlocked RWMutex")
	}
	// readerWait--操作，如果readerWait--操作之后的值为0，说明，写锁之前，已经没有读锁了
	// 通过writerSem信号量，唤醒队列中第一个阻塞的写锁
	if atomic.AddInt32(&rw.readerWait, -1) == 0 {
		// 唤醒一个写锁
		runtime_Semrelease(&rw.writerSem, false, 1)
	}
}
```

- 首先还是操作`readerCount`；

  - 如果操作之后的值大于0，说明还有读锁存在，直接结束本次操作；

  - 如果操作之后值小于0，说明还有写锁存在，尝试在最后一个读锁完成的时候去唤醒写锁；

- `readerWait--`操作，如果`readerWait--`操作之后的值为0，说明，写锁之前，已经没有读锁了；

- 通过信号量唤醒队列中第一个被阻塞的写锁。

# 写锁

## Lock

```go
// 获取互斥锁
// 阻塞等待所有读操作结束（如果有的话）
func (rw *RWMutex) Lock() {
	if race.Enabled {
		_ = rw.w.state
		race.Disable()
	}
	// 获取互斥锁
	rw.w.Lock()

	// 原子的修改readerCount的值，直接将readerCount减去rwmutexMaxReaders
	// 说明，有写锁进来了，这在上面的读锁中也有体现
	r := atomic.AddInt32(&rw.readerCount, -rwmutexMaxReaders) + rwmutexMaxReaders
	// 当r不为0说明，当前写锁之前有读锁的存在
	// 修改下readerWait，也就是当前写锁需要等待的读锁的个数 
	if r != 0 && atomic.AddInt32(&rw.readerWait, r) != 0 {
		// 阻塞当前写锁
		runtime_SemacquireMutex(&rw.writerSem, false, 0)
	}
	if race.Enabled {
		race.Enable()
		race.Acquire(unsafe.Pointer(&rw.readerSem))
		race.Acquire(unsafe.Pointer(&rw.writerSem))
	}
}
```

- 修改下`readerCount`的数量，直接减去`rwmutexMaxReaders`；
- 判断当前写锁之前，是否有读锁的存在；
  - 写操作要等待读操作结束后才可以获得锁，写操作等待期间可能还有新的读操作持续到来，如果写操作等待所有读操作结束，很可能被饿死。然而，通过`RWMutex.readerWait`可完美解决这个问题。
  - 写操作到来时，会把`RWMutex.readerCount`值拷贝到`RWMutex.readerWait`中，用于标记排在写操作前面的读者个数。
  - 前面的读操作结束后，除了会递减`RWMutex.readerCount`，还会递减`RWMutex.readerWait`值，当`RWMutex.readerWait`值变为0时唤醒写操作。
  - 写操作之后产生的读操作就会加入到`readerCount`，阻塞直到写锁释放。
- 如果有读锁，阻塞当前写锁；

## Unlock

```go
// 如果写锁未锁定，解锁将会触发panic

//一个锁定的互斥锁与一个特定的goroutine没有关联。
// 它允许一个goroutine锁定一个写锁然后
// 安排另一个goroutine解锁它。
func (rw *RWMutex) Unlock() {
	if race.Enabled {
		_ = rw.w.state
		race.Release(unsafe.Pointer(&rw.readerSem))
		race.Disable()
	}

	// 增加readerCount, 若超过读锁的最大限制, 触发panic
	// 和写锁定的-rwmutexMaxReaders，向对应
	r := atomic.AddInt32(&rw.readerCount, rwmutexMaxReaders)
	if r >= rwmutexMaxReaders {
		race.Enable()
		throw("sync: Unlock of unlocked RWMutex")
	}
	// 如果r>0，说明当前写锁后面，有阻塞的读锁
	// 然后，通过信号量一一释放阻塞的读锁
	for i := 0; i < int(r); i++ {
		runtime_Semrelease(&rw.readerSem, false, 0)
	}
	// 释放互斥锁
	rw.w.Unlock()
	if race.Enabled {
		race.Enable()
	}
}
```

- 首先修改`readerCount`的值，加上`rwmutexMaxReaders`，和上文的`-rwmutexMaxReaders`相呼应；
- 然后判断后面是否有读锁被阻塞，如果有一一唤醒。

# 问题要论

## 写操作是如何阻止写操作的

读写锁包含一个互斥锁(Mutex)，写锁定必须要先获取该互斥锁，如果互斥锁已被协程A获取（或者协程A在阻塞等待读结束），意味着协程A获取了互斥锁，那么协程B只能阻塞等待该互斥锁。

所以，写操作依赖互斥锁阻止其他的写操作。

> mutex

## 写操作是如何阻止读操作的

我们知道`RWMutex.readerCount`是个整型值，用于表示读者数量，不考虑写操作的情况下，每次读锁定将该值+1，每次解除读锁定将该值-1，所以readerCount取值为[0, N]，N为读者个数，实际上最大可支持2^30个并发读者。

当写锁定进行时，会先将readerCount减去2^30，从而readerCount变成了负值，此时再有读锁定到来时检测到readerCount为负值，便知道有写操作在进行，只好阻塞等待。而真实的读操作个数并不会丢失，只需要将readerCount加上2^30即可获得。

> 所以，写操作将readerCount变成负值来阻止读操作的。

## 读操作是如何阻止写操作的

写操作到来时，会把`RWMutex.readerCount`值拷贝到`RWMutex.readerWait`中，用于标记排在写操作前面的读者个数。

前面的读操作结束后，除了会递减`RWMutex.readerCount`，还会递减`RWMutex.readerWait`值，当`RWMutex.readerWait`值变为0时唤醒写操作。

## 为什么写锁定不会被饿死

写操作要等待读操作结束后才可以获得锁，写操作等待期间可能还有新的读操作持续到来，如果写操作等待所有读操作结束，很可能被饿死。然而，通过`RWMutex.readerWait`可完美解决这个问题。

写操作到来时，会把`RWMutex.readerCount`值拷贝到`RWMutex.readerWait`中，用于标记排在写操作前面的读者个数。

前面的读操作结束后，除了会递减`RWMutex.readerCount`，还会递减`RWMutex.readerWait`值，当`RWMutex.readerWait`值变为0时唤醒写操作。

## 两个读锁之间穿插了一个写锁

```go
type test struct {
	data map[string]string
	r    sync.RWMutex
}

func (t test) read() {
	t.r.RLock()
	t.r.Lock()
	fmt.Println(t.data)
	t.r.Unlock()
	t.r.RUnlock()
}
```

`deadlock!`

- 读锁是会阻塞写锁的，上面的读锁已经上锁了；
- 后面的写锁来加锁。发现已经有读锁了，然后使用信号量阻塞当前的写锁，等待读锁解锁时被唤醒；
- 然后这个写锁马上解锁，但是当前的写锁，一直在等待被信号量唤醒，读锁的解锁又一直在等待写锁的解锁；
- 然后就死循环，deadlock。