<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [GDB/LLDB](#gdblldb)
  - [指令](#%E6%8C%87%E4%BB%A4)
- [delve](#delve)
  - [安装](#%E5%AE%89%E8%A3%85)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

目前 Go 语言支持 GDB、LLDB 和 Delve 几种调试器。

- 其中 GDB 是最早支持的调试工具

- LLDB 是 macOS 系统推荐的标准调试工具。但是 GDB 和 LLDB 对 Go 语言的专有特性都缺乏很大支持，
-  Delve 是专门为 Go 语言设计开发的调试工具。

# GDB/LLDB

```bash
# 关闭内联优化，方便调试
go build -gcflags "-N -l" demo.go

gdb demo

# 使用界面
gdb -tui demo
```

## 指令

要熟练使用 GDB ，你得熟悉的掌握它的指令，这里列举一下

- `r`：run，执行程序 
- `n`：next，下一步，不进入函数
- `s`：step，下一步，会进入函数
- `b`：breakponit，设置断点
- `l`：list，查看源码
- `c`：continue，继续执行到下一断点
- `bt`：backtrace，查看当前调用栈
- `p`：print，打印查看变量
- `q`：quit，退出 GDB
- `whatis`：查看对象类型
- `info breakpoints`：查看所有的断点
- `info locals`：查看局部变量
- `info args`：查看函数的参数值及要返回的变量值 
- `info frame`：堆栈帧信息
- `info goroutines`：查看 goroutines 信息。在使用前 ，需要注意先执行 source /usr/local/go/src/runtime/runtime-gdb.py
- `goroutine 1 bt`：查看指定序号的 goroutine 调用堆栈
- 回车：重复执行上一次操作

# delve

## 安装

```go
go install github.com/go-delve/delve/cmd/dlv@latest

// macOS，还需要安装命令行开发工具
xcode-select --install
sudo /usr/sbin/DevToolsSecurity -enable
```

在项目目录中(go.mod文件所在目录中)执行`dlv debug`命令进入调试:

 `help` 命令查看 `dlv` 支持的命令有哪些：

```go
(dlv) help
```

