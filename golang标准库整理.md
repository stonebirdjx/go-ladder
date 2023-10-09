<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [Regexp](#regexp)
  - [:point_right:type Regexp struct](#point_righttype-regexp-struct)
  - [func Match](#func-match)
  - [func MatchReader](#func-matchreader)
  - [func MatchString](#func-matchstring)
  - [func QuoteMeta](#func-quotemeta)
- [regexp/syntax](#regexpsyntax)
- [runtime](#runtime)
  - [type BlockProfileRecord struct](#type-blockprofilerecord-struct)
  - [type Error interface](#type-error-interface)
  - [type Frame](#type-frame)
  - [type Frames](#type-frames)
  - [type Func](#type-func)
  - [type MemProfileRecord](#type-memprofilerecord)
  - [type MemStats](#type-memstats)
  - [type PanicNilError](#type-panicnilerror)
  - [type Pinner](#type-pinner)
  - [type StackRecord](#type-stackrecord)
  - [type TypeAssertionError](#type-typeassertionerror)
- [sync](#sync)
  - [:point_right:type Locker interface](#point_righttype-locker-interface)
  - [:point_right:type Mutex struct](#point_righttype-mutex-struct)
  - [:point_right:type RWMutex struct](#point_righttype-rwmutex-struct)
  - [:point_right:type WaitGroup struct](#point_righttype-waitgroup-struct)
  - [:point_right:type Pool struct](#point_righttype-pool-struct)
  - [:point_right:type Once struct](#point_righttype-once-struct)
  - [:point_right:type Map struct](#point_righttype-map-struct)
  - [:point_right:type Cond struct](#point_righttype-cond-struct)
  - [func OnceFunc](#func-oncefunc)
  - [func OnceValue](#func-oncevalue)
  - [func OnceValues](#func-oncevalues)
- [sync/atomic](#syncatomic)
  - [type Bool struct](#type-bool-struct)
  - [type Int32 struct](#type-int32-struct)
  - [type Int64 struct](#type-int64-struct)
  - [:point_right:type Pointer struct](#point_righttype-pointer-struct)
  - [type Uint32 struct](#type-uint32-struct)
  - [type Uint64 struct](#type-uint64-struct)
  - [:point_right:type Uintptr struct](#point_righttype-uintptr-struct)
  - [type Value struct](#type-value-struct)
  - [Funcs](#funcs)
- [unsafe](#unsafe)
  - [type ArbitraryType int](#type-arbitrarytype-int)
  - [type IntegerType int](#type-integertype-int)
  - [:point_right:type Pointer *ArbitraryType](#point_righttype-pointer-arbitrarytype)
  - [:point_right:func Alignof](#point_rightfunc-alignof)
  - [:point_right:func Offsetof](#point_rightfunc-offsetof)
  - [:point_right:func Sizeof](#point_rightfunc-sizeof)
  - [func String](#func-string)
  - [func StringData](#func-stringdata)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

golang的一些常用标准库的整理，基于`go1.21.2`版本

# Regexp

regexp 包实现正则表达式搜索。它是 RE2 接受的语法，并在 https://golang.org/s/re2syntax 中进行了描述，但 \C 除外。

> 使用前最好用compile 与编译一下提性能

## :point_right:type Regexp struct 

```Go
type Regexp struct {
        expr           string       // as passed to Compile
        prog           *syntax.Prog // compiled program
        onepass        *onePassProg // onepass program or nil
        numSubexp      int
        maxBitStateLen int
        subexpNames    []string
        prefix         string         // required prefix in unanchored matches
        prefixBytes    []byte         // prefix, as a []byte
        prefixRune     rune           // first rune in prefix
        prefixEnd      uint32         // pc for last rune in prefix
        mpool          int            // pool for machines
        matchcap       int            // size of recorded match lengths
        prefixComplete bool           // prefix is the entire regexp
        cond           syntax.EmptyOp // empty-width conditions required at start of match
        minInputLen    int            // minimum length of the input in bytes

        // This field can be modified by the Longest method,
        // but it is otherwise read-only.
        longest bool // whether regexp prefers leftmost-longest match
}
```

Regexp 是编译后的正则表达式的表示。 正则表达式对于多个 goroutine 并发使用是安全的，除了配置方法，例如 Longest。

Func:

```Go
func Compile(expr string) (*Regexp, error) // Compile 解析正则表达式，如果成功，则返回可用于匹配文本的 Regexp 对象。当匹配文本时，正则表达式返回一个在输入中尽可能早开始的匹配（最左边），并在其中选择回溯搜索首先找到的匹配。 这种所谓的最左优先匹配与 Perl、Python 和其他实现使用的语义相同，尽管这个包实现它时没有回溯的代价。
func CompilePOSIX(expr string) (*Regexp, error) // CompilePOSIX 与 Compile 类似，但将正则表达式限制为 POSIX ERE (egrep) 语法，并将匹配语义更改为最左最长。CompilePOSIX 与 Compile 类似，但将正则表达式限制为 POSIX ERE (egrep) 语法，并将匹配语义更改为最左最长。
func MustCompile(str string) *Regexp // MustCompile 与 Compile 类似，但如果无法解析表达式，则会出现恐慌。 它简化了保存已编译正则表达式的全局变量的安全初始化。
func MustCompilePOSIX(str string) *Regexp //MustCompilePOSIX 与 CompilePOSIX 类似，但如果无法解析表达式，则会出现恐慌。 它简化了保存已编译正则表达式的全局变量的安全初始化。
func (*Regexp).Copy() *Regexp //DEPRECATED
func (*Regexp).Expand(dst []byte, template []byte, src []byte, match []int) []byte
func (*Regexp).ExpandString(dst []byte, template string, src string, match []int) []byte
func (*Regexp).Find(b []byte) []byte
func (*Regexp).FindAll(b []byte, n int) [][]byte
func (*Regexp).FindAllIndex(b []byte, n int) [][]int
func (*Regexp).FindAllString(s string, n int) []string
func (*Regexp).FindAllStringIndex(s string, n int) [][]int
func (*Regexp).FindAllStringSubmatch(s string, n int) [][]string
func (*Regexp).FindAllStringSubmatchIndex(s string, n int) [][]int
func (*Regexp).FindAllSubmatch(b []byte, n int) [][][]byte
func (*Regexp).FindAllSubmatchIndex(b []byte, n int) [][]int
func (*Regexp).FindIndex(b []byte) (loc []int)
func (*Regexp).FindReaderIndex(r io.RuneReader) (loc []int)
func (*Regexp).FindReaderSubmatchIndex(r io.RuneReader) []int
func (*Regexp).FindString(s string) string
func (*Regexp).FindStringIndex(s string) (loc []int)
func (*Regexp).FindStringSubmatch(s string) []string
func (*Regexp).FindStringSubmatchIndex(s string) []int
func (*Regexp).FindSubmatch(b []byte) [][]byte
func (*Regexp).FindSubmatchIndex(b []byte) []int
func (*Regexp).LiteralPrefix() (prefix string, complete bool)
func (*Regexp).Longest()
func (*Regexp).MarshalText() ([]byte, error)
func (*Regexp).Match(b []byte) bool
func (*Regexp).MatchReader(r io.RuneReader) bool
func (*Regexp).MatchString(s string) bool
func (*Regexp).NumSubexp() int
func (*Regexp).ReplaceAll(src []byte, repl []byte) []byte
func (*Regexp).ReplaceAllFunc(src []byte, repl func([]byte) []byte) []byte
func (*Regexp).ReplaceAllLiteral(src []byte, repl []byte) []byte
func (*Regexp).ReplaceAllLiteralString(src string, repl string) string
func (*Regexp).ReplaceAllString(src string, repl string) string
func (*Regexp).ReplaceAllStringFunc(src string, repl func(string) string) string
func (*Regexp).Split(s string, n int) []string
func (*Regexp).String() string
func (*Regexp).SubexpIndex(name string) int
func (*Regexp).SubexpNames() []string
func (*Regexp).UnmarshalText(text []byte) error
func (*Regexp).allMatches(s string, b []byte, n int, deliver func([]int))
func (*Regexp).backtrack(ib []byte, is string, pos int, ncap int, dstCap []int) []int
func (*Regexp).doExecute(r io.RuneReader, b []byte, s string, pos int, ncap int, dstCap []int) []int
func (*Regexp).doMatch(r io.RuneReader, b []byte, s string) bool
func (*Regexp).doOnePass(ir io.RuneReader, ib []byte, is string, pos int, ncap int, dstCap []int) []int
func (*Regexp).expand(dst []byte, template string, bsrc []byte, src string, match []int) []byte
func (*Regexp).get() *machine
func (*Regexp).pad(a []int) []int
func (*Regexp).put(m *machine)
func (*Regexp).replaceAll(bsrc []byte, src string, nmatch int, repl func(dst []byte, m []int) []byte) []byte
func (*Regexp).tryBacktrack(b *bitState, i input, pc uint32, pos int) bool
```

## func Match

```Go
func Match(pattern string, b []byte) (matched bool, err error)
```

Match 报告字节片 b 是否包含正则表达式模式的任何匹配项。 更复杂的查询需要使用 Compile 和完整的 Regexp 接口。

Example

```Go
package main

import (
        "fmt"
        "regexp"
)

func main() {
        matched, err := regexp.Match(`foo.*`, []byte(`seafood`))
        fmt.Println(matched, err)
        matched, err = regexp.Match(`bar.*`, []byte(`seafood`))
        fmt.Println(matched, err)
        matched, err = regexp.Match(`a(b`, []byte(`seafood`))
        fmt.Println(matched, err)

}
```

## func MatchReader

```Go
func MatchReader(pattern string, r io.RuneReader) (matched bool, err error)
```

MatchReader 报告 RuneReader 返回的文本是否包含正则表达式模式的任何匹配项。 更复杂的查询需要使用 Compile 和完整的 Regexp 接口。

## func MatchString

```Go
func MatchString(pattern string, s string) (matched bool, err error)
```

MatchString 报告字符串 s 是否包含正则表达式模式的任何匹配项。 更复杂的查询需要使用 Compile 和完整的 Regexp 接口。

```Go
package main

import (
        "fmt"
        "regexp"
)

func main() {
        matched, err := regexp.MatchString(`foo.*`, "seafood")
        fmt.Println(matched, err)
        matched, err = regexp.MatchString(`bar.*`, "seafood")
        fmt.Println(matched, err)
        matched, err = regexp.MatchString(`a(b`, "seafood")
        fmt.Println(matched, err)
}
```

## func QuoteMeta

```Go
func QuoteMeta(s string) string
```

QuoteMeta 返回一个转义参数文本中所有正则表达式元字符的字符串； 返回的字符串是与文字文本匹配的正则表达式。

Example

```Go
package main

import (
        "fmt"
        "regexp"
)

func main() {
        fmt.Println(regexp.QuoteMeta(`Escaping symbols like: .+*?()|[]{}^$`))
}
```

# regexp/syntax

https://pkg.go.dev/regexp/syntax

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

# [sync](https://pkg.go.dev/sync)

> sync包里的东西禁止复制

## :point_right:type Locker interface 

Locker 代表可以锁定和解锁的对象。

```Go
// A Locker represents an object that can be locked and unlocked.
type Locker interface {
        Lock()
        Unlock()
}
```

- 合理使用锁，`Unlock()`一定要在`Lock()` 建议使用`defer`

## :point_right:type Mutex struct 

```Go
type Mutex struct {
        state int32
        sema  uint32
}
```

- Mutex 是互斥锁。
- 互斥体的零值是未锁定的互斥体。
- 第一次使用后不得复制互斥体。

Func:

```Go
func (*Mutex).Lock()
func (*Mutex).TryLock() bool
func (*Mutex).Unlock()
func (*Mutex).lockSlow()
func (*Mutex).unlockSlow(new int32)
```

Example:

```Go
package main

import (
    "fmt"
    "sync"
)

var counter int
var counterMutex sync.Mutex

func increment() {
    counterMutex.Lock()  // atomic.CompareAndSwapInt32
    defer counterMutex.Unlock() // new := atomic.AddInt32(&m.state, -mutexLocked) 设置state值为0
    counter++
}

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 1000; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            increment()
        }()
    }
    wg.Wait()
    fmt.Println("Final counter value:", counter)
}
```

## :point_right:type RWMutex struct

```Go
type RWMutex struct {
        w           Mutex        // held if there are pending writers
        writerSem   uint32       // semaphore for writers to wait for completing readers
        readerSem   uint32       // semaphore for readers to wait for completing writers
        readerCount atomic.Int32 // number of pending readers
        readerWait  atomic.Int32 // number of departing readers
}
```

- RWMutex 是读/写互斥锁。
- RWMutex 可以由任意数量的读取者或单个写入者持有。
- RWMutex 的零值是未锁定的互斥体。
- RWMutex 在首次使用后不得复制。

Func:

```Go
func (*RWMutex).Lock()
func (*RWMutex).RLock()
func (*RWMutex).RLocker() Locker
func (*RWMutex).RUnlock()
func (*RWMutex).TryLock() bool
func (*RWMutex).TryRLock() bool
func (*RWMutex).Unlock()
func (*RWMutex).rUnlockSlow(r int32)
```

Example:

```Go
package main

import (
        "fmt"
        "sync"
        "time"
)

var (
        data  map[string]string
        mutex sync.RWMutex
)

func main() {
        data = make(map[string]string)

        // 启动多个读取协程
        for i := 0; i < 3; i++ {
                go readData(i)
        }

        // 启动一个写入协程
        go writeData()

        // 等待一段时间，让读写操作执行
        time.Sleep(5 * time.Second)
}

func readData(id int) {
        for {
                mutex.RLock() // 获取读锁
                fmt.Printf("协程 %d 读取数据：%v\n", id, data)
                mutex.RUnlock() // 释放读锁

                time.Sleep(1 * time.Second)
        }
}

func writeData() {
        for {
                mutex.Lock()                                  // 获取写锁
                data["key"] = time.Now().Format(time.RFC3339) // 写入数据
                fmt.Println("写入数据：", data)
                mutex.Unlock() // 释放写锁

                time.Sleep(2 * time.Second)
        }
}
```

## :point_right:type WaitGroup struct 

```Go
type WaitGroup struct {
        noCopy noCopy

        state atomic.Uint64 // high 32 bits are counter, low 32 bits are waiter count.
        sema  uint32
}
```

- WaitGroup 等待 goroutine 集合完成。
- 主goroutine调用Add来设置数量
- 要等待的 goroutine。 然后每个 goroutine
- 运行并在完成时调用 Done。 同时，
- Wait 可用于阻塞，直到所有 goroutine 完成。
- 首次使用后不得复制 WaitGroup。

Func:

```Go
func (*WaitGroup).Add(delta int)
func (*WaitGroup).Done()
func (*WaitGroup).Wait()
```

Example：

```Go
package main

import (
        "fmt"
        "sync"
        "time"
)

func main() {
        var wg sync.WaitGroup

        // 启动多个 goroutine
        for i := 0; i < 5; i++ {
                wg.Add(1) // 增加等待计数

                go func(id int) {
                        defer wg.Done() // 减少等待计数

                        fmt.Printf("协程 %d 开始执行\n", id)
                        time.Sleep(2 * time.Second)
                        fmt.Printf("协程 %d 执行完成\n", id)
                }(i)
        }

        wg.Wait() // 等待所有协程完成

        fmt.Println("所有协程执行完成")
}
```

## :point_right:type Pool struct 

```Go
type Pool struct {
        noCopy noCopy

        local     unsafe.Pointer // local fixed-size per-P pool, actual type is [P]poolLocal
        localSize uintptr        // size of the local array

        victim     unsafe.Pointer // local from previous cycle
        victimSize uintptr        // size of victims array

        // New optionally specifies a function to generate
        // a value when Get would otherwise return nil.
        // It may not be changed concurrently with calls to Get.
        New func() any
}
```

- Pool 是一组可以单独保存和使用的临时对象
- 检索到存储在池中的任何项目都可以随时自动删除，无需通知。 如果发生这种情况时 Pool 持有唯一的引用，则项目可能会被释放。
- 多个 Goroutine 同时使用 Pool 是安全的。
- 池的目的是缓存已分配但未使用的项目以供以后重用，减轻垃圾收集器的压力。 也就是说，它可以很容易地构建高效、线程安全的空闲列表。
- Pool 的适当用途是管理一组临时项在并发独立之间静默共享并可能被并发独立重用包的客户端。 池提供了一种分摊分配开销的方法
- 很好使用 Pool 的例子，它维护了一个动态大小的临时输出缓冲区存储。
- 另一方面，作为短期对象的一部分维护的空闲列表是不适合使用 Pool，因为开销不能很好地摊销
-  首次使用后不得复制池。

> sync.Pool 可以用于存储临时对象，以避免频繁地分配和释放内存，从而减少垃圾回收的负担。

Func:

```Go
func (*Pool).Get() any
func (*Pool).Put(x any)
func (*Pool).getSlow(pid int) any
func (*Pool).pin() (*poolLocal, int)
func (*Pool).pinSlow() (*poolLocal, int)
```

Example：

```Go
package main

import (
        "fmt"
        "sync"
)

type Object struct {
        ID int
}

func main() {
        pool := sync.Pool{
                New: func() interface{} {
                        fmt.Println("Creating new object")
                        return &Object{}
                },
        }

        // 从池中获取对象
        obj1 := pool.Get().(*Object)
        obj1.ID = 1
        fmt.Println("Object 1:", obj1)

        // 将对象放回池中
        pool.Put(obj1)

        // 再次从池中获取对象
        obj2 := pool.Get().(*Object)
        fmt.Println("Object 2:", obj2)
}
```

## :point_right:type Once struct 

```Go
type Once struct {
        // done indicates whether the action has been performed.
        // It is first in the struct because it is used in the hot path.
        // The hot path is inlined at every call site.
        // Placing done first allows more compact instructions on some architectures (amd64/386),
        // and fewer instructions (to calculate offset) on other architectures.
        done uint32
        m    Mutex
}
```

- Once 是一个只执行一个操作的对象。
- 首次使用后不得复制 Once。

Func:

```Go
func (*Once).Do(f func())
func (*Once).doSlow(f func())
```

Example:

```Go
package main

import (
        "fmt"
        "sync"
)

func main() {
        var once sync.Once

        for i := 0; i < 3; i++ {
                go func() {
                        once.Do(func() {
                                fmt.Println("只执行一次的操作")
                        })
                        fmt.Println("其他操作")
                }()
        }

        // 等待一段时间，以便观察输出
        fmt.Scanln()
}
```

## :point_right:type Map struct 

```Go
type Map struct {
        mu Mutex

        // read contains the portion of the map's contents that are safe for
        // concurrent access (with or without mu held).
        //
        // The read field itself is always safe to load, but must only be stored with
        // mu held.
        //
        // Entries stored in read may be updated concurrently without mu, but updating
        // a previously-expunged entry requires that the entry be copied to the dirty
        // map and unexpunged with mu held.
        read atomic.Pointer[readOnly]

        // dirty contains the portion of the map's contents that require mu to be
        // held. To ensure that the dirty map can be promoted to the read map quickly,
        // it also includes all of the non-expunged entries in the read map.
        //
        // Expunged entries are not stored in the dirty map. An expunged entry in the
        // clean map must be unexpunged and added to the dirty map before a new value
        // can be stored to it.
        //
        // If the dirty map is nil, the next write to the map will initialize it by
        // making a shallow copy of the clean map, omitting stale entries.
        dirty map[any]*entry

        // misses counts the number of loads since the read map was last updated that
        // needed to lock mu to determine whether the key was present.
        //
        // Once enough misses have occurred to cover the cost of copying the dirty
        // map, the dirty map will be promoted to the read map (in the unamended
        // state) and the next store to the map will make a new dirty copy.
        misses int
}
```

- Map 类似于 Go map[interface{}]interface{} 但可以安全地并发使用由多个 goroutine 执行，无需额外的锁定或协调。加载、存储和删除在摊余常量时间内运行。
- 读取操作：Load, LoadAndDelete, LoadOrStore, Swap, CompareAndSwap, and CompareAndDelete
- 写入操作: Delete, LoadAndDelete, Store, and Swap

Func:

```Go
func (*Map).CompareAndDelete(key any, old any) (deleted bool) //如果 key 的值等于 old，CompareAndDelete 将删除该 key 的条目。 旧值必须是可比较类型。如果映射中没有 key 的当前值，CompareAndDelete 将返回 false（即使旧值是 nil 接口值）。

func (*Map).CompareAndSwap(key any, old any, new any) bool // 如果存储在映射中的值等于旧值，CompareAndSwap 就会交换键的新旧值。 旧值必须是可比较类型。

func (*Map).Delete(key any)
func (*Map).Load(key any) (value any, ok bool)
func (*Map).LoadAndDelete(key any) (value any, loaded bool)
func (*Map).LoadOrStore(key any, value any) (actual any, loaded bool) //LoadOrStore 返回键的现有值（如果存在）。 否则，它存储并返回给定值。

func (*Map).Range(f func(key any, value any) bool)
func (*Map).Store(key any, value any)
func (*Map).Swap(key any, value any) (previous any, loaded bool) // Swap 将值交换为键，并返回之前的值（如果有）。 loaded 报告密钥是否存在。
func (*Map).dirtyLocked()
func (*Map).loadReadOnly() readOnly
func (*Map).missLocked()
```

Example：

```Go
package main

import (
        "fmt"
        "sync"
)

func main() {
        var m sync.Map

        // 存储键值对
        m.Store("key1", "value1")
        m.Store("key2", "value2")

        // 从映射中加载值
        value1, ok1 := m.Load("key1")
        value2, ok2 := m.Load("key2")
        value3, ok3 := m.Load("key3")

        fmt.Println("Value for key1:", value1, ", Exists:", ok1)
        fmt.Println("Value for key2:", value2, ", Exists:", ok2)
        fmt.Println("Value for key3:", value3, ", Exists:", ok3)

        // 删除键值对
        m.Delete("key2")

        // 遍历映射中的所有键值对
        m.Range(func(key, value interface{}) bool {
                fmt.Println("Key:", key, "Value:", value)
                return true
        })
}
```

## :point_right:type Cond struct 

```Go
type Cond struct {
        noCopy noCopy

        // L is held while observing or changing the condition
        L Locker

        notify  notifyList
        checker copyChecker
}
```

- Cond 实现一个条件变量，一个集合点用于等待或宣布发生的 goroutine一个事件。
- 每个Cond都有一个关联的Locker L（通常是*Mutex或*RWMutex），
- 改变条件时必须保存 当调用Wait方法时。
-  首次使用后不得复制 Cond。

> 一般用于微小广播或信号唤醒

Func:

```Go
func (*Cond).Broadcast() //广播唤醒所有在 c 上等待的 goroutine。允许但不要求呼叫者在通话过程中按住 c.L。
func (*Cond).Signal() // 信号会唤醒一个在 c 上等待的 goroutine（如果有的话）。
func (*Cond).Wait() // Wait 以原子方式解锁 c.L 并暂停调用 goroutine 的执行。 稍后恢复执行后，Wait 在返回之前锁定 c.L。
```

Example：

```Go
package main

import (
        "fmt"
        "sync"
        "time"
)

var sharedRsc = false

func main() {
        var wg sync.WaitGroup
        wg.Add(2)
        m := sync.Mutex{}
        c := sync.NewCond(&m)
        go func() {
                defer wg.Done()
                c.L.Lock()
                defer c.L.Unlock()
                for !sharedRsc {
                        fmt.Println("goroutine1 wait")
                        c.Wait()
                }
                fmt.Println("goroutine1", sharedRsc)
        }()

        go func() {
                defer wg.Done()
                c.L.Lock()
                defer c.L.Unlock()
                for !sharedRsc {
                        fmt.Println("goroutine2 wait")
                        c.Wait()
                }
                fmt.Println("goroutine2", sharedRsc)
        }()

        time.Sleep(2 * time.Second)
        c.L.Lock()
        fmt.Println("main goroutine ready")
        sharedRsc = true
        c.Broadcast()
        fmt.Println("main goroutine broadcast")
        c.L.Unlock()
        wg.Wait()
}
```

## func OnceFunc

```Go
func OnceFunc(f func()) func() {}
```

- OnceFunc 返回一个仅调用 f 一次的函数。 返回的函数可以被并发调用。
- 如果 f 发生panic，则返回的函数将在每次调用时以相同的值发生panic。

Example:

```Go
package main

import (
        "fmt"
        "sync"
)

func exec() {
        fmt.Println("haha")
}

func main() {
        f := sync.OnceFunc(exec)
        f()
        f()
}
```

## func OnceValue

```Go
func OnceValue[T any](f func() T) func() T
```

- OnceValue 返回一个仅调用 f 一次的函数，并返回 f 返回的值。 返回的函数可以被并发调用。
- 如果 f 发生恐慌，则返回的函数将在每次调用时以相同的值发生恐慌。

Example:

```Go
package main

import (
        "fmt"
        "sync"
)

func exec() string {
        return "haha"
}

func main() {
        f := sync.OnceValue[string](exec)
        fmt.Println(f())
        fmt.Println(f())
}
// haha
// haha
```

## func OnceValues

```Go
func OnceValues[T1, T2 any](f func() (T1, T2)) func() (T1, T2)
```

- OnceValue 用法一致

# [sync/atomic](https://pkg.go.dev/sync/atomic@go1.21.2)

包atomic提供了对于实现同步算法有用的低级原子内存原语。

除了特殊的低级应用程序之外，同步最好使用chan或sync包来完成

swap operation

```Go
old = *addr
*addr = new
return old
```

compare-and-swap operation

```Go
if *addr == old {
        *addr = new
        return true
}
return false
```

add operation

```Go
*addr += delta
return *addr
```

## type Bool struct 

```Go
type Bool struct {
        _ noCopy
        v uint32
}
```

- Bool 是一个原子布尔值。
- 零值是 false。

Func:

```Go
func (*atomic.Bool).CompareAndSwap(old bool, new bool) (swapped bool)
func (*atomic.Bool).Load() bool
func (*atomic.Bool).Store(val bool)
func (*atomic.Bool).Swap(new bool) (old bool)
```

Example:

```Go
package main

import (
        "fmt"
        "sync/atomic"
)

var (
        a = false
        b = true
)

func main() {
        atoBool := atomic.Bool{}
        atoBool.Store(a)
        s := atoBool.CompareAndSwap(a, b)
        fmt.Println(atoBool.Load())
}
// true
```

## type Int32 struct 

```Go
type Int32 struct {
        _ noCopy
        v int32
}
```

Func:

```Go
func (*Int32).Add(delta int32) (new int32)
func (*Int32).CompareAndSwap(old int32, new int32) (swapped bool)
func (*Int32).Load() int32
func (*Int32).Store(val int32)
func (*Int32).Swap(new int32) (old int32)
```

Example: 参见Bool

## type Int64 struct 

```Go
type Int64 struct {
        _ noCopy
        _ align64
        v int64
}
```

Func:

```Go
func (*Int64).Add(delta int64) (new int64)
func (*Int64).CompareAndSwap(old int64, new int64) (swapped bool)
func (*Int64).Load() int64
func (*Int64).Store(val int64)
func (*Int64).Swap(new int64) (old int64)
```

## :point_right:type Pointer struct 

```Go
type Pointer[T any] struct {
        // Mention *T in a field to disallow conversion between Pointer types.
        // See go.dev/issue/56603 for more details.
        // Use *T, not T, to avoid spurious recursive type definition errors.
        _ [0]*T

        _ noCopy
        v unsafe.Pointer
}
```

Func:

```Go
func (*Pointer[T]).CompareAndSwap(old *T, new *T) (swapped bool)
func (*Pointer[T]).Load() *T
func (*Pointer[T]).Store(val *T)
func (*Pointer[T]).Swap(new *T) (old *T)
```

Example：

```Go
package main

import (
        "fmt"
        "sync/atomic"
)

func main() {
        s := "hello world"
        sn := "hello world sichuan"
        atoPointer := atomic.Pointer[string]{}
        atoPointer.Store(&s)
        atoPointer.Swap(&sn)
        fmt.Println(*atoPointer.Load())
}
// hello world sichuan
```

## type Uint32 struct 

```Go
// A Uint32 is an atomic uint32. The zero value is zero.
type Uint32 struct {
        _ noCopy
        v uint32
}
```

Func:

```Go
func (*Uint32).Add(delta uint32) (new uint32)
func (*Uint32).CompareAndSwap(old uint32, new uint32) (swapped bool)
func (*Uint32).Load() uint32
func (*Uint32).Store(val uint32)
func (*Uint32).Swap(new uint32) (old uint32)
```

## type Uint64 struct 

```Go
type Uint64 struct {
        _ noCopy
        _ align64
        v uint64
}
```

Func:

```Go
func (*Uint64).Add(delta uint64) (new uint64)
func (*Uint64).CompareAndSwap(old uint64, new uint64) (swapped bool)
func (*Uint64).Load() uint64
func (*Uint64).Store(val uint64)
func (*Uint64).Swap(new uint64) (old uint64)
```

## :point_right:type Uintptr struct 

```Go
type Uintptr struct {
        _ noCopy
        v uintptr
}
```

Func:

```Go
func (*Uintptr).Add(delta uintptr) (new uintptr)
func (*Uintptr).CompareAndSwap(old uintptr, new uintptr) (swapped bool)
func (*Uintptr).Load() uintptr
func (*Uintptr).Store(val uintptr)
func (*Uintptr).Swap(new uintptr) (old uintptr)
```

## type Value struct 

```Go
type Value struct {
        v any
}
```

Func:

```Go
func (*Value).CompareAndSwap(old any, new any) (swapped bool)
func (*Value).Load() (val any)
func (*Value).Store(val any)
func (*Value).Swap(new any) (old any)
```

## Funcs

```Go
func SwapInt32(addr *int32, new int32) (old int32)
func SwapInt64(addr *int64, new int64) (old int64)
func SwapUint32(addr *uint32, new uint32) (old uint32)
func SwapUint64(addr *uint64, new uint64) (old uint64)
func SwapUintptr(addr *uintptr, new uintptr) (old uintptr)
func SwapPointer(addr *unsafe.Pointer, new unsafe.Pointer) (old unsafe.Pointer)
func CompareAndSwapInt32(addr *int32, old, new int32) (swapped bool)
func CompareAndSwapInt64(addr *int64, old, new int64) (swapped bool)
func CompareAndSwapUint32(addr *uint32, old, new uint32) (swapped bool)
func CompareAndSwapUint64(addr *uint64, old, new uint64) (swapped bool)
func CompareAndSwapUintptr(addr *uintptr, old, new uintptr) (swapped bool)
func CompareAndSwapPointer(addr *unsafe.Pointer, old, new unsafe.Pointer) (swapped bool)
func AddInt32(addr *int32, delta int32) (new int32)
func AddUint32(addr *uint32, delta uint32) (new uint32)
func AddInt64(addr *int64, delta int64) (new int64)
func AddUint64(addr *uint64, delta uint64) (new uint64)
func AddUintptr(addr *uintptr, delta uintptr) (new uintptr)
func LoadInt32(addr *int32) (val int32)
func LoadInt64(addr *int64) (val int64)
func LoadUint32(addr *uint32) (val uint32)
func LoadUint64(addr *uint64) (val uint64)
func LoadUintptr(addr *uintptr) (val uintptr)
func LoadPointer(addr *unsafe.Pointer) (val unsafe.Pointer)
func StoreInt32(addr *int32, val int32)
func StoreInt64(addr *int64, val int64)
func StoreUint32(addr *uint32, val uint32)
func StoreUint64(addr *uint64, val uint64)
func StoreUintptr(addr *uintptr, val uintptr)
func StorePointer(addr *unsafe.Pointer, val unsafe.Pointer)
```

# [unsafe](https://pkg.go.dev/unsafe)

unsafe 包包含绕过 Go 程序类型安全的操作。导入不安全的包可能是不可移植的，并且不受 Go 1 兼容性指南的保护。

:point_right:大致步骤：

1. 普通 pointer 转unsafe.Pointer
2. unsafe.Pointer 转 uintptr （如需）
3. uintptr 做运算操作（如需）
4. uintptr 转 unsafe.Pointer（如需）
5. unsafe.Pointer 转 普通 pointer

## type ArbitraryType int

## type IntegerType int

## :point_right:type Pointer *ArbitraryType

```Go
func Float64bits(f float64) uint64 {
        return *(*uint64)(unsafe.Pointer(&f))
}

p = unsafe.Pointer(uintptr(p) + offset)

// equivalent to f := unsafe.Pointer(&s.f)
f := unsafe.Pointer(uintptr(unsafe.Pointer(&s)) + unsafe.Offsetof(s.f))
// equivalent to e := unsafe.Pointer(&x[i])
e := unsafe.Pointer(uintptr(unsafe.Pointer(&x[0])) + i*unsafe.Sizeof(x[0]))

// INVALID: end points outside allocated space.
var s thing
end = unsafe.Pointer(uintptr(unsafe.Pointer(&s)) + unsafe.Sizeof(s))
// INVALID: end points outside allocated space.
b := make([]byte, n)
end = unsafe.Pointer(uintptr(unsafe.Pointer(&b[0])) + uintptr(n))

// INVALID: uintptr cannot be stored in variable
// before conversion back to Pointer.
u := uintptr(p)
p = unsafe.Pointer(u + offset)
```

- A pointer value of any type can be converted to a Pointer.
- A Pointer can be converted to a pointer value of any type.
- A uintptr can be converted to a Pointer.
- A Pointer can be converted to a uintptr.

Example：

```Go
package main

import (
        "fmt"
        "unsafe"
)

type Person struct {
        name   string
        age    int
        salary float64
}

func main() {
        p := Person{
                name:   "Alice",
                age:    30,
                salary: 5000.0,
        }

        // 将 Person 类型的指针转换为 unsafe.Pointer 类型
        ptr := unsafe.Pointer(&p)

        // 访问未导出的 name 字段
        namePtr := (*string)(unsafe.Pointer(uintptr(ptr) + unsafe.Offsetof(p.name)))
        name := *namePtr

        // 访问未导出的 age 字段
        agePtr := (*int)(unsafe.Pointer(uintptr(ptr) + unsafe.Offsetof(p.age)))
        age := *agePtr

        // 访问未导出的 salary 字段
        salaryPtr := (*float64)(unsafe.Pointer(uintptr(ptr) + unsafe.Offsetof(p.salary)))
        salary := *salaryPtr

        fmt.Println("Name:", name)
        fmt.Println("Age:", age)
        fmt.Println("Salary:", salary)
}
```

## :point_right:func Alignof

```Go
func Alignof(x ArbitraryType) uintptr
```

- Alignof 接受任何类型的表达式 x 并返回假设变量 v 所需的对齐方式，就好像 v 是通过 var v = x 声明的一样。 它是使 v 的地址始终为零 mod m 的最大值 m。 它与reflect.TypeOf(x).Align()返回的值相同。

Example:

```Go
package main

import (
        "fmt"
        "unsafe"
)

type Person struct {
        name   string
        age    int
        salary float64
}

func main() {
        p := Person{
                name:   "Alice",
                age:    30,
                salary: 5000.0,
        }

        fmt.Println(unsafe.Alignof(p))
}
```

## :point_right:func Offsetof

```Go
func Offsetof(x ArbitraryType) uintptr
```

Offsetof 返回 x 表示的字段在结构体中的偏移量，该字段必须采用 structValue.field 形式。 换句话说，它返回结构体开头和字段开头之间的字节数。

Example:

```Go
agePtr := (*int)(unsafe.Pointer(uintptr(ptr) + unsafe.Offsetof(p.age)))
```

## :point_right:func Sizeof

```Go
func Sizeof(x ArbitraryType) uintptr
```

Sizeof 接受任何类型的表达式 x 并返回假设变量 v 的大小（以字节为单位），就好像 v 是通过 var v = x 声明的一样。 该大小不包括 x 可能引用的任何内存。

Example:

```Go
 fmt.Println(unsafe.Sizeof(Args{}))
```

## func String

```Go
func String(ptr *byte, len IntegerType) string
```

- String 返回一个字符串值，其底层字节从 ptr 开始，长度为 len。
- len 参数必须是整数类型或无类型常量。 常量 len 参数必须是非负的并且可以用 int 类型的值表示； 如果它是无类型常量，则其类型为 int。 在运行时，如果 len 为负，或者 ptr 为 nil 并且 len 不为零，则会发生运行时恐慌。
- 由于 Go 字符串是不可变的，因此传递给 String 的字节之后不得修改。

Example:

```Go
b := byte(97)
fmt.Println(unsafe.String(&b, 1))
// a
```

## func StringData

```Go
func StringData(str string) *byte
```

StringData 返回指向 str 底层字节的指针。 对于空字符串，返回值未指定，并且可能为 nil。

由于 Go 字符串是不可变的，因此 StringData 返回的字节不得修改。

Example:

```Go
package main

import (
        "fmt"
        "unsafe"
)

func main() {
        s := "Hello, world!"
        bs := unsafe.StringData(s)

        us := unsafe.String(bs, 5)
        fmt.Println(us)
}
// Hello
```