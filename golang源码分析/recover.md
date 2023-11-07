<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [recover](#recover)
- [使用](#%E4%BD%BF%E7%94%A8)
- [总结](#%E6%80%BB%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

Golang 没有结构化异常，使用 panic 抛出错误，recover 捕获错误。

- panic 只会触发当前goroutine的defer
- recover只能在defer的func中使用才有效

# recover

编译器会将关键字 recover 转换成` runtime.gorecover`：

```go
func gorecover(argp uintptr) interface{} {
	gp := getg()
	p := gp._panic
	if p != nil && !p.recovered && argp == uintptr(p.argp) {
		p.recovered = true
		return p.arg
	}
	return nil
}
```

# 使用

```go
package main

import (
    "fmt"
)

func main() {
  // recover() 无效
  // defer recover() 无效
    defer func() {
        if err := recover(); err != nil {
            fmt.Println(err)
        }
    }()
    panic("oh my god!panic.")
}

```

# 总结

- `panic` 能够改变程序的控制流，调用 `panic` 后会立刻停止执行当前函数的剩余代码，并在当前 `Goroutine` 中递归执行调用方的 `defer`；

  - 立刻停止执行当前函数的剩余代码

  - 当前goroutine中递归执行调用 defer

- `recover` 可以中止 `panic` 造成的程序崩溃。它是一个只能在 `defer` 中发挥作用的函数，在其他作用域中调用不会发挥作用；
  - recover只能与defer结合使用

- 如果 panic 和 recover 发生在同一个协程，那么 recover 是可以捕获的，如果 panic 和 recover 发生在不同的协程，那么 recover 是不可以捕获的。