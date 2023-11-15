# [sync/atomic](https://pkg.go.dev/sync/atomic@go1.21.2)

包atomic提供了对于实现同步算法有用的低级原子内存原语。

除了特殊的低级应用程序之外，同步最好使用chan或sync包来完成

swap operation

```
old = *addr
*addr = new
return old
```



compare-and-swap operation

```
if *addr == old {
        *addr = new
        return true
}
return false
```



add operation

```
*addr += delta
return *addr
```



## [type Bool struct](https://github.com/stonebirdjx/go-ladder/blob/master/golang标准库整理.md#type-bool-struct)

```
type Bool struct {
        _ noCopy
        v uint32
}
```



- Bool 是一个原子布尔值。
- 零值是 false。

Func:

```
func (*atomic.Bool).CompareAndSwap(old bool, new bool) (swapped bool)
func (*atomic.Bool).Load() bool
func (*atomic.Bool).Store(val bool)
func (*atomic.Bool).Swap(new bool) (old bool)
```



Example:

```
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



## [type Int32 struct](https://github.com/stonebirdjx/go-ladder/blob/master/golang标准库整理.md#type-int32-struct)

```
type Int32 struct {
        _ noCopy
        v int32
}
```



Func:

```
func (*Int32).Add(delta int32) (new int32)
func (*Int32).CompareAndSwap(old int32, new int32) (swapped bool)
func (*Int32).Load() int32
func (*Int32).Store(val int32)
func (*Int32).Swap(new int32) (old int32)
```



Example: 参见Bool

## [type Int64 struct](https://github.com/stonebirdjx/go-ladder/blob/master/golang标准库整理.md#type-int64-struct)

```
type Int64 struct {
        _ noCopy
        _ align64
        v int64
}
```



Func:

```
func (*Int64).Add(delta int64) (new int64)
func (*Int64).CompareAndSwap(old int64, new int64) (swapped bool)
func (*Int64).Load() int64
func (*Int64).Store(val int64)
func (*Int64).Swap(new int64) (old int64)
```



## [👉type Pointer struct](https://github.com/stonebirdjx/go-ladder/blob/master/golang标准库整理.md#point_righttype-pointer-struct)

```
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

```
func (*Pointer[T]).CompareAndSwap(old *T, new *T) (swapped bool)
func (*Pointer[T]).Load() *T
func (*Pointer[T]).Store(val *T)
func (*Pointer[T]).Swap(new *T) (old *T)
```



Example：

```
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



## [type Uint32 struct](https://github.com/stonebirdjx/go-ladder/blob/master/golang标准库整理.md#type-uint32-struct)

```
// A Uint32 is an atomic uint32. The zero value is zero.
type Uint32 struct {
        _ noCopy
        v uint32
}
```



Func:

```
func (*Uint32).Add(delta uint32) (new uint32)
func (*Uint32).CompareAndSwap(old uint32, new uint32) (swapped bool)
func (*Uint32).Load() uint32
func (*Uint32).Store(val uint32)
func (*Uint32).Swap(new uint32) (old uint32)
```



## [type Uint64 struct](https://github.com/stonebirdjx/go-ladder/blob/master/golang标准库整理.md#type-uint64-struct)

```
type Uint64 struct {
        _ noCopy
        _ align64
        v uint64
}
```



Func:

```
func (*Uint64).Add(delta uint64) (new uint64)
func (*Uint64).CompareAndSwap(old uint64, new uint64) (swapped bool)
func (*Uint64).Load() uint64
func (*Uint64).Store(val uint64)
func (*Uint64).Swap(new uint64) (old uint64)
```



## [👉type Uintptr struct](https://github.com/stonebirdjx/go-ladder/blob/master/golang标准库整理.md#point_righttype-uintptr-struct)

```
type Uintptr struct {
        _ noCopy
        v uintptr
}
```

Func:

```
func (*Uintptr).Add(delta uintptr) (new uintptr)
func (*Uintptr).CompareAndSwap(old uintptr, new uintptr) (swapped bool)
func (*Uintptr).Load() uintptr
func (*Uintptr).Store(val uintptr)
func (*Uintptr).Swap(new uintptr) (old uintptr)
```

## [type Value struct](https://github.com/stonebirdjx/go-ladder/blob/master/golang标准库整理.md#type-value-struct)

```
type Value struct {
        v any
}
```

Func:

```
func (*Value).CompareAndSwap(old any, new any) (swapped bool)
func (*Value).Load() (val any)
func (*Value).Store(val any)
func (*Value).Swap(new any) (old any)
```

## [Funcs](https://github.com/stonebirdjx/go-ladder/blob/master/golang标准库整理.md#funcs)

```
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

