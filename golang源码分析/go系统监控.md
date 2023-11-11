<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [main](#main)
- [循环监控](#%E5%BE%AA%E7%8E%AF%E7%9B%91%E6%8E%A7)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

守护进程是很有效的设计，它在整个系统的生命周期中都会存在，会随着系统的启动而启动，系统的结束而结束。

Go 语言的系统监控也起到了很重要的作用，它在内部启动了一个不会中止的循环，在循环的内部会轮询网络、抢占长期运行或者处于系统调用的 Goroutine 以及触发垃圾回收，通过这些行为，它能够让系统的运行状态变得更健康。

# main

 当我们通过 `runtime.newproc` 创建好主 Goroutine 后，会将其加入到一个 P 的本地队列中。 随着 `runtime.mstart` 启动调度器，主 Goroutine 便开始得以调度。

```go
// src/runtime/proc.go

// 主 Goroutine
func main() {
	(...)
	// 启动系统后台监控（定期垃圾回收、并发任务调度）
	systemstack(func() {
		newm(sysmon, nil)
	})
	(...)
}
```

# 循环监控

```go
// 系统监控在一个独立的 m 上运行
// 总是在没有 P 的情况下运行，因此不能出现写屏障
//go:nowritebarrierrec
func sysmon() {
  lock(&sched.lock)
	// 不计入死锁的系统 m 的数量
	sched.nmsys++
	// 死锁检查
	checkdead()
	unlock(&sched.lock)
	idle := 0 // 没有 wokeup 的周期数
	delay := uint32(0)
  for {
    if idle == 0 {
			delay = 20
		} else if idle > 50 {
			delay *= 2
		}
		if delay > 10*1000 {
			delay = 10 * 1000
		}
    // 休眠
    usleep(delay)
    
     // 如果在 STW，则暂时休眠
    if debug.schedtrace <= 0 && (sched.gcwaiting != 0 || atomic.Load(&sched.npidle) == uint32(gomaxprocs)) {
    }
    // 需要时触发 libc interceptor
    // 如果超过 10ms 没有 poll，则 poll 一下网络
    // 抢夺在 syscall 中阻塞的 P、运行时间过长的 G
    // 检查是否需要强制触发 GC
  }
}
```



