# runtime

Runtime 包与 Go 运行时系统交互的操作，例如控制 goroutine 的函数。 它还包括reflect包使用的低级类型信息； 

GOGC 变量设置初始垃圾收集目标百分比。 当新分配的数据与上次收集后剩余的实时数据的比例达到此百分比时，将触发收集。 默认值为 GOGC=100。 设置 GOGC=off 会完全禁用垃圾收集器。 runtime/debug.SetGCPercent 允许在运行时更改此百分比。

GOMEMLIMIT 变量设置运行时的软内存限制。

GODEBUG 变量控制运行时内的调试变量。

## type BlockProfileRecord struct 

```Go
type BlockProfileRecord struct {
        Count  int64
        Cycles int64
        StackRecord
}
```

BlockProfileRecord 描述源自特定调用序列（堆栈跟踪）的阻塞事件。

## type Error interface 

```Go
type Error interface {
        error

        // RuntimeError is a no-op function but
        // serves to distinguish types that are run time
        // errors from ordinary errors: a type is a
        // run time error if it has a RuntimeError method.
        RuntimeError()
}
```

Error 接口标识运行时错误。

## type Frame

```Go
type Frame struct {
        // PC is the program counter for the location in this frame.
        // For a frame that calls another frame, this will be the
        // program counter of a call instruction. Because of inlining,
        // multiple frames may have the same PC value, but different
        // symbolic information.
        PC uintptr

        // Func is the Func value of this call frame. This may be nil
        // for non-Go code or fully inlined functions.
        Func *Func

        // Function is the package path-qualified function name of
        // this call frame. If non-empty, this string uniquely
        // identifies a single function in the program.
        // This may be the empty string if not known.
        // If Func is not nil then Function == Func.Name().
        Function string

        // File and Line are the file name and line number of the
        // location in this frame. For non-leaf frames, this will be
        // the location of a call. These may be the empty string and
        // zero, respectively, if not known.
        File string
        Line int

        // startLine is the line number of the beginning of the function in
        // this frame. Specifically, it is the line number of the func keyword
        // for Go functions. Note that //line directives can change the
        // filename and/or line number arbitrarily within a function, meaning
        // that the Line - startLine offset is not always meaningful.
        //
        // This may be zero if not known.
        startLine int

        // Entry point program counter for the function; may be zero
        // if not known. If Func is not nil then Entry ==
        // Func.Entry().
        Entry uintptr

        // The runtime's internal view of the function. This field
        // is set (funcInfo.valid() returns true) only for Go functions,
        // not for C functions.
        funcInfo funcInfo
}
```

Frame是Frames为每个调用帧返回的信息。

## type Frames

```Go
type Frames struct {
        // callers is a slice of PCs that have not yet been expanded to frames.
        callers []uintptr

        // frames is a slice of Frames that have yet to be returned.
        frames     []Frame
        frameStore [2]Frame
}
```

帧可用于获取调用者返回的 PC 值片段的函数/文件/行信息。

Func:

```Go
func CallersFrames(callers []uintptr) *Frames //CallersFrames 获取 Callers 返回的 PC 值的一部分，并准备返回函数/文件/行信息。 在完成框架之前，请勿更改切片。

func (ci *Frames) Next() (frame Frame, more bool) // Next 返回一个 Frame，表示 PC 值切片中的下一个调用帧。 如果它已经返回所有调用帧，则 Next 返回零帧。
```

Example:

```Go
package main

import (
        "fmt"
        "runtime"
        "strings"
)

func main() {
        c := func() {
                // Ask runtime.Callers for up to 10 PCs, including runtime.Callers itself.
                pc := make([]uintptr, 10)
                n := runtime.Callers(0, pc)
                if n == 0 {
                        // No PCs available. This can happen if the first argument to
                        // runtime.Callers is large.
                        //
                        // Return now to avoid processing the zero Frame that would
                        // otherwise be returned by frames.Next below.
                        return
                }

                pc = pc[:n] // pass only valid pcs to runtime.CallersFrames
                frames := runtime.CallersFrames(pc)

                // Loop to get frames.
                // A fixed number of PCs can expand to an indefinite number of Frames.
                for {
                        frame, more := frames.Next()

                        // Process this frame.
                        //
                        // To keep this example's output stable
                        // even if there are changes in the testing package,
                        // stop unwinding when we leave package runtime.
                        if !strings.Contains(frame.File, "runtime/") {
                                break
                        }
                        fmt.Printf("- more:%v | func=%s file=%s line=%d \n", more, frame.Function, frame.File, frame.Line)

                        // Check whether there are more frames to process after this one.
                        if !more {
                                break
                        }
                }
        }

        b := func() { c() }
        a := func() { b() }

        a()
}
```

## type Func

## type MemProfileRecord

## type MemStats

## type PanicNilError

## type Pinner

## type StackRecord

## type TypeAssertionError

