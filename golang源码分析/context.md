<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
- [emptyCtx](#emptyctx)
- [valueCtx](#valuectx)
  - [WithValue](#withvalue)
- [cancelCtx](#cancelctx)
  - [WithCancel](#withcancel)
- [timerCtx](#timerctx)
  - [WithDeadline](#withdeadline)
  - [WithTimeout](#withtimeout)
- [Context最佳实践](#context%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

标准库中的 Context 是一个接口，其具体实现有很多种,主要用于**超时控制**和**多Goroutine间的数据传递**。

Context 在 Go 1.7 中被添加入标准库，主要用于跨多个 Goroutine 设置截止时间、同步信号、传递上下文请求值等。

由于需要跨多个 Goroutine 传递信号，所以多个 Context 往往需要关联到一起，形成类似一个树的结构。所以在角色上 Context 分为三种:

- root(根) Context
- parent(父) Context
- child(子) Context

# 数据结构

```go
type Context interface {
    Done() <-chan struct{}
    Err() error
    Deadline() (deadline time.Time, ok bool)
    Value(key interface{}) interface{}
}
```

- `Done` 返回一个 `chan`, 表示一个取消信号，当这个通道被关闭时，函数应该立刻结束工作并返回。

- `Err()` 返回一个 `error`, 表示取消上下文的原因

- `Deadline` 会返回上下文取消的时间

- `Value` 用于从上下文中获取 `key` 对应的值

# emptyCtx

emptyCtx 实际上就是个 int，其对 Context 接口的主要实现(Deadline、Done、Err、Value)全部返回了 nil，也就是说其实是一个 “啥也不干” 的 Context；它通常用于创建 root Context，标准库中 context.Background() 和 context.TODO() 返回的就是这个 emptyCtx。

```Go
type emptyCtx int

func (*emptyCtx) Deadline() (deadline time.Time, ok bool) {
        return
}

func (*emptyCtx) Done() <-chan struct{} {
        return nil
}

func (*emptyCtx) Err() error {
        return nil
}

func (*emptyCtx) Value(key interface{}) interface{} {
        return nil
}

func (e *emptyCtx) String() string {
        switch e {
        case background:
                return "context.Background"
        case todo:
                return "context.TODO"
        }
        return "unknown empty Context"
}

var (
        background = new(emptyCtx)
        todo       = new(emptyCtx)
)

func Background() Context {
        return background
}


func TODO() Context {
        return todo
}
```

# valueCtx

valueCtx能够为当前ctx附加一个KV信息。这里非常容易产生误解的点是：valueCtx仅仅能够附加一个KV，而不是一个map键值对。

```Go
type valueCtx struct {
        Context
        key, val any // key val 仅仅是any，并不是键值对
}

// WithValue
func WithValue(parent Context, key, val any) Context {
        if parent == nil {
                panic("cannot create context from nil parent")
        }
        if key == nil {
                panic("nil key")
        }
        if !reflectlite.TypeOf(key).Comparable() {
                panic("key is not comparable")
        }
        return &valueCtx{parent, key, val}
}

func (c *valueCtx) Value(key any) any {
        if c.key == key {
                // 找到这个key则返回对应val
                return c.val
        }
        // 递归寻找父Context是否包含这个key
        return value(c.Context, key)
}
```

## WithValue

```Go
package main

import (
        "context"
        "fmt"
)

func main() {
        type favContextKey string

        f := func(ctx context.Context, k favContextKey) {
                if v := ctx.Value(k); v != nil {
                        fmt.Println("found value:", v)
                        return
                }
                fmt.Println("key not found:", k)
        }

        k := favContextKey("language")
        ctx := context.WithValue(context.Background(), k, "Go")

        f(ctx, k)
        f(ctx, favContextKey("color"))

}
```

# cancelCtx

该方法返回子ctx和一个名为cancel的func，cancel不需要任何参数就能执行，它的作用在于控制该ctx进入Done状态。任何进入Done状态的ctx，ctx.Done()方法将会得到channel通道的消息。

```Go
// A cancelCtx can be canceled. When canceled, it also cancels any children
// that implement canceler.
type cancelCtx struct {
        Context

        mu       sync.Mutex            // protects following fields
        done     atomic.Value          // of chan struct{}, created lazily, closed by first cancel call
        children map[canceler]struct{} // set to nil by the first cancel call
        err      error                 // set to non-nil by the first cancel call
        cause    error                 // set to non-nil by the first cancel call
}

//  cancel 是一个闭包
func WithCancelCause(parent Context) (ctx Context, cancel CancelCauseFunc) {
        c := withCancel(parent)
        return c, func(cause error) { c.cancel(true, Canceled, cause) }
}

func withCancel(parent Context) *cancelCtx {
        if parent == nil {
                panic("cannot create context from nil parent")
        }
        c := &cancelCtx{}
        c.propagateCancel(parent, c)
        return c
}
```

## WithCancel

```Go
package main

import (
        "context"
        "fmt"
)

func main() {
        gen := func(ctx context.Context) <-chan int {
                dst := make(chan int)
                n := 1
                go func() {
                        for {
                                select {
                                case <-ctx.Done():
                                        return // returning not to leak the goroutine
                                case dst <- n:
                                        n++
                                }
                        }
                }()
                return dst
        }

        ctx, cancel := context.WithCancel(context.Background())
        defer cancel() // cancel when we are finished consuming integers

        for n := range gen(ctx) {
                fmt.Println(n)
                if n == 5 {
                        break
                }
        }
}
```

# timerCtx

```Go
type timerCtx struct {
        cancelCtx
        timer *time.Timer // Under cancelCtx.mu.

        deadline time.Time
}
```

context包提供了2个常用方法：

context.WithDeadline

context.WithTimeout

## WithDeadline

```Go
func WithDeadline(parent Context, d time.Time) (Context, CancelFunc) {
        if parent == nil {
                panic("cannot create context from nil parent")
        }
        if cur, ok := parent.Deadline(); ok && cur.Before(d) {
                // The current deadline is already sooner than the new one.
                return WithCancel(parent)
        }
        c := &timerCtx{
                cancelCtx: newCancelCtx(parent),
                deadline:  d,
        }
        propagateCancel(parent, c)
        dur := time.Until(d)
        if dur <= 0 {
                c.cancel(true, DeadlineExceeded) // deadline has already passed
                return c, func() { c.cancel(false, Canceled) }
        }
        c.mu.Lock()
        defer c.mu.Unlock()
        if c.err == nil {
                c.timer = time.AfterFunc(dur, func() { // 主要多出time.AfterFunc相关逻辑
                        c.cancel(true, DeadlineExceeded)
                })
        }
        return c, func() { c.cancel(true, Canceled) }
}
d := time.Now().Add(shortDuration)
ctx, cancel := context.WithDeadline(context.Background(), d)

// Even though ctx will be expired, it is good practice to call its
// cancellation function in any case. Failure to do so may keep the
// context and its parent alive longer than necessary.
defer cancel()

select {
case <-neverReady:
        fmt.Println("ready")
case <-ctx.Done():
        fmt.Println(ctx.Err())
}
```

## WithTimeout

context.WithTimeout是直接复用了context.WithDeadline实现

```Go
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc) {
  return WithDeadline(parent, time.Now().Add(timeout)) // 直接调用WithDeadline，以time.Now().Add(timeout)计算出Deadline
}
// Pass a context with a timeout to tell a blocking function that it
// should abandon its work after the timeout elapses.
ctx, cancel := context.WithTimeout(context.Background(), shortDuration)
defer cancel()

select {
case <-neverReady:
        fmt.Println("ready")
case <-ctx.Done():
        fmt.Println(ctx.Err()) // prints "context deadline exceeded"
}
```

# Context最佳实践

- 不传递nil，不确定使用什么类型的情况下，传递context.TODO()。
- 使用函数参数传递而不是结构体，占函数第一个参数位置，并命名为ctx。
- 只在Context中附加与请求有关的元数据，不依赖于Context来传递可选参数。
- Context可以传递到不同的Goroutine，并发安全。

> Parent nil 会pannic.