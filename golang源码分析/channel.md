<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
- [创建chan](#%E5%88%9B%E5%BB%BAchan)
- [发送数据](#%E5%8F%91%E9%80%81%E6%95%B0%E6%8D%AE)
- [接收数据](#%E6%8E%A5%E6%94%B6%E6%95%B0%E6%8D%AE)
- [关闭channel](#%E5%85%B3%E9%97%ADchannel)
- [终结](#%E7%BB%88%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

CSP 全称是 “Communicating Sequential Processes”

不要通过共享内存来通信，而要通过通信来实现内存共享。

尽量使用 channel；把 goroutine 当作免费的资源，随便用。

# 数据结构

```Go
type hchan struct {
  // 当前队列中总元素个数
  qcount   uint           // total data in the queue
  // 环形队列长度，即缓冲区大小（申明channel时指定的大小）
  dataqsiz uint           // size of the circular queue
  // 环形队列指针
  buf      unsafe.Pointer // points to an array of dataqsiz elements
  // buf中每个元素的大小
  elemsize uint16
  // 当前通道是否处于关闭状态，创建通道时该字段为0，关闭时字段为1
  closed   uint32
  // 元素类型，用于传值过程的赋值
  elemtype *_type // element type
  // 环形缓冲区中已发送位置索引
  sendx    uint   // send index
  // 环形缓冲区中已接收位置索引
  recvx    uint   // receive index
  // 等待读消息的groutine队列
  recvq    waitq  // list of recv waiters
  // 等待写消息的groutine队列
  sendq    waitq  // list of send waiters
  // 互斥锁，为每个读写操作锁定通道（发送和接收必须互斥）
  lock mutex
}

// 等待读写的队列数据结构，保证先进先出
type waitq struct {
  first *sudog
  last  *sudog
}
```

hchan 中的所有属性大致可以分为三类：

- buffer 相关的属性。例如 buf、dataqsiz、qcount 等。 当 channel 的缓冲区大小不为 0 时，buffer 中存放了待接收的数据。使用 ring buffer 实现。
- waitq 相关的属性，可以理解为是一个 FIFO 的标准队列。其中 recvq 中是正在等待接收数据的 goroutine，sendq 中是等待发送数据的 goroutine。waitq 使用双向链表实现。
- 其他属性，例如 lock、elemtype、closed 等。

# 创建chan

```Go
// 无缓冲的chan
ch := make(chan int)

ch := make(chan int，3)
// golang 编译器翻译为 runtime.makechan 函数, 其函数签名如下
func makechan(t *chantype, size int) *hchan
```

# 发送数据

当我们想要向 Channel 发送数据时，就需要使用 `ch <- i` 语句

- 如果当前 Channel 的 recvq 上存在已经被阻塞的 Goroutine，那么会直接将数据发送给当前 Goroutine 并将其设置成下一个运行的 Goroutine；
- 如果 Channel 存在缓冲区并且其中还有空闲的容量，我们会直接将数据存储到缓冲区 sendx 所在的位置上；
- 如果不满足上面的两种情况，会创建一个 runtime.sudog 结构并将其加入 Channel 的 sendq 队列中，当前 Goroutine 也会陷入阻塞等待其他的协程从 Channel 接收数据；

# 接收数据

- 如果 Channel 为空，那么会直接调用 runtime.gopark 挂起当前 Goroutine；
- 如果 Channel 已经关闭并且缓冲区没有任何数据，runtime.chanrecv 会直接返回；
- 如果 Channel 的 sendq 队列中存在挂起的 Goroutine，会将 recvx 索引所在的数据拷贝到接收变量所在的内存空间上并将 sendq 队列中 Goroutine 的数据拷贝到缓冲区；
- 如果 Channel 的缓冲区中包含数据，那么直接读取 recvx 索引对应的数据；
- 在默认情况下会挂起当前的 Goroutine，将 runtime.sudog 结构加入 recvq 队列并陷入休眠等待调度器的唤醒；

# 关闭channel

使用 `close` 关键字关闭通道

```Go
close(ch)

//  runtime.closechan 函数
```

# 终结

 给一个 nil channel 发送数据，造成永远阻塞

 从一个 nil channel 接收数据，造成永远阻塞

 给一个已经关闭的 channel 发送数据，引起 panic

从一个已经关闭的 channel 接收数据，如果缓冲区中为空，则返回一个零值

无缓冲的channel 是同步的, 有缓冲的channel是异步的