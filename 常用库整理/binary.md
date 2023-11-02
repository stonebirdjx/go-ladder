<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [大端序和小端序](#%E5%A4%A7%E7%AB%AF%E5%BA%8F%E5%92%8C%E5%B0%8F%E7%AB%AF%E5%BA%8F)
- [func AppendUvarint(buf []byte, x uint64) []byte](#func-appenduvarintbuf-byte-x-uint64-byte)
- [func AppendVarint(buf []byte, x int64) []byte](#func-appendvarintbuf-byte-x-int64-byte)
- [func PutUvarint(buf []byte, x uint64) int](#func-putuvarintbuf-byte-x-uint64-int)
- [func PutVarint(buf []byte, x int64) int](#func-putvarintbuf-byte-x-int64-int)
- [:point_right:func Read(r io.Reader, order ByteOrder, data any) error](#point_rightfunc-readr-ioreader-order-byteorder-data-any-error)
- [func ReadUvarint(r io.ByteReader) (uint64, error)](#func-readuvarintr-iobytereader-uint64-error)
- [func ReadVarint(r io.ByteReader) (int64, error)](#func-readvarintr-iobytereader-int64-error)
- [func Size(v any) int](#func-sizev-any-int)
- [func Uvarint(buf []byte) (uint64, int)](#func-uvarintbuf-byte-uint64-int)
- [func Varint(buf []byte) (int64, int)](#func-varintbuf-byte-int64-int)
- [:point_right:func Write(w io.Writer, order ByteOrder, data any) error](#point_rightfunc-writew-iowriter-order-byteorder-data-any-error)
- [type AppendByteOrder interfacer](#type-appendbyteorder-interfacer)
- [type ByteOrder interface](#type-byteorder-interface)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

包binary实现了 <font color="red"> 数字和字节序列</font> 之间的简单转换以及varints的编码和解码。

<font color="red">通过读取和写入固定大小的值来转换数字</font> 。 固定大小值可以是固定大小算术类型（bool、int8、uint8、int16、float32、complex64...），也可以是仅包含固定大小值的数组或结构。

# 大端序和小端序

字节（byte）是计算机世界的计量单位

![img](../images/big_and_small_ending.png)

大端字节序：高位字节在前，低位字节在后，这是人类读写数值的方法。简单来说，就是按照从低地址到高地址的顺序存放数据的高位字节到低位字节.

小端字节序：低位字节在前，高位字节在后，就是按照从低地址到高地址的顺序存放据的低位字节到高位字节

> 网络字节序：TCP/IP各层协议将字节序定义为 Big Endian，因此TCP/IP协议中使用的字节序是大端序。

> 主机字节序：整数在内存中存储的顺序，现在 Little Endian 比较普遍。（不同的 CPU 有不同的字节序）

```go
package main

import (
        "bytes"
        "encoding/binary"
        "fmt"
        "unsafe"
)

func main() {
        var i uint32 = 280
        i2 := (*[4]byte)(unsafe.Pointer(&i)) // 转换成byte
        // 验证计算机内部使用的是小端序还是大端序
        fmt.Print("内存中地址是：")
        for _, bin := range i2 {
                fmt.Printf("%02X ", bin)
        }
        fmt.Printf("\n")

        b := make([]byte, 4)
        binary.LittleEndian.PutUint32(b, i) // 小端序
        fmt.Printf("LittleEndian(%d) :", i)
        for _, bin := range b {
                fmt.Printf("%02X ", bin)
        }
        fmt.Printf("\n")

        fmt.Printf("BigEndian(%d) :", i) // 大端序
        binary.BigEndian.PutUint32(b, i)
        for _, bin := range b {
                fmt.Printf("%02X ", bin)
        }
        fmt.Printf("\n")

        bytesBuffer := bytes.NewBuffer(b) // byte转换成uint32
        var j uint32
        err := binary.Read(bytesBuffer, binary.BigEndian, &j)
        if err != nil {
                return
        }
        fmt.Println("j = ", j)

}
/*
内存中地址是：18 01 00 00 
LittleEndian(280) :18 01 00 00 
BigEndian(280) :00 00 01 18 
j =  280
*/
```

# func AppendUvarint(buf []byte, x uint64) []byte

AppendUvarint 将 PutUvarint 生成的 x 的 varint 编码形式附加到 buf 并返回扩展缓冲区。

# func AppendVarint(buf []byte, x int64) []byte

AppendVarint 将 PutVarint 生成的 x 的 varint 编码形式附加到 buf 并返回扩展缓冲区。

#  func PutUvarint(buf []byte, x uint64) int

PutUvarint 将 uint64 编码为 buf 并返回写入的字节数。 如果缓冲区太小，PutUvarint 会出现panic。

```Go
package main

import (
        "encoding/binary"
        "fmt"
)

func main() {
        buf := make([]byte, binary.MaxVarintLen64)

        for _, x := range []uint64{1, 2, 127, 128, 255, 256} {
                n := binary.PutUvarint(buf, x)
                fmt.Printf("%x\n", buf[:n])
        }
}
```

# func PutVarint(buf []byte, x int64) int

PutVarint 将 int64 编码到 buf 中并返回写入的字节数。 如果缓冲区太小，PutVarint 将会出现panic。

# :point_right:func Read(r io.Reader, order ByteOrder, data any) error

Read 将结构化二进制数据从 r 读取到 data 中。 数据必须是指向固定大小值或固定大小值切片的指针。 使用指定的字节顺序对从 r 读取的字节进行解码，并将其写入数据的连续字段。 解码布尔值时，零字节被解码为假，任何其他非零字节被解码为真。 读入结构体时，会跳过字段名称为空白 (_) 的字段的字段数据； 即，空白字段名称可用于填充。 读入结构时，必须导出所有非空白字段，否则读取可能会出现恐慌。

仅当未读取任何字节时，错误才为 EOF。 如果读取部分字节但不是全部字节后发生 EOF，则 Read 将返回 ErrUnexpectedEOF。

```Go
package main

import (
        "bytes"
        "encoding/binary"
        "fmt"
)

func main() {
        var pi float64
        b := []byte{0x18, 0x2d, 0x44, 0x54, 0xfb, 0x21, 0x09, 0x40}
        buf := bytes.NewReader(b)
        err := binary.Read(buf, binary.LittleEndian, &pi)
        if err != nil {
                fmt.Println("binary.Read failed:", err)
        }
        fmt.Print(pi)
}
// 3.141592653589793
```

# func ReadUvarint(r io.ByteReader) (uint64, error)

ReadUvarint 从 r 读取编码的无符号整数并将其作为 uint64 返回。 仅当未读取任何字节时，错误才为 EOF。 如果在读取部分而非全部字节后发生 EOF，则 ReadUvarint 返回 io.ErrUnexpectedEOF。

# func ReadVarint(r io.ByteReader) (int64, error)

ReadVarint 从 r 读取编码的有符号整数并将其作为 int64 返回。 仅当未读取任何字节时，错误才为 EOF。 如果在读取部分而非全部字节后发生 EOF，则 ReadVarint 返回 io.ErrUnexpectedEOF。

# func Size(v any) int

Size 返回 Write 将生成多少字节来编码值 v，该值必须是固定大小值或固定大小值的切片，或者指向此类数据的指针。 如果 v 不是其中任何一个，则 Size 返回 -1。

# func Uvarint(buf []byte) (uint64, int)

Uvarint 从 buf 解码 uint64 并返回该值和读取的字节数 (> 0)。

```Go
package main

import (
        "encoding/binary"
        "fmt"
)

func main() {
        inputs := [][]byte{
                {0x01},
                {0x02},
                {0x7f},
                {0x80, 0x01},
                {0xff, 0x01},
                {0x80, 0x02},
        }
        for _, b := range inputs {
                x, n := binary.Uvarint(b)
                if n != len(b) {
                        fmt.Println("Uvarint did not consume all of in")
                }
                fmt.Println(x)
        }
}
/*
1
2
127
128
255
256
*/
```

# func Varint(buf []byte) (int64, int)

Varint 从 buf 解码 int64 并返回该值和读取的字节数 (> 0)。

# :point_right:func Write(w io.Writer, order ByteOrder, data any) error

Write 将数据的二进制表示形式写入 w 中。 数据必须是固定大小的值或固定大小值的切片，或者指向此类数据的指针。 布尔值编码为一个字节：1 表示 true，0 表示 false。 写入 w 的字节使用指定的字节顺序进行编码，并从数据的连续字段中读取。 编写结构时，将为具有空白 (_) 字段名称的字段写入零值。

```Go
package main

import (
        "bytes"
        "encoding/binary"
        "fmt"
        "math"
)

func main() {
        buf := new(bytes.Buffer)
        var pi float64 = math.Pi
        err := binary.Write(buf, binary.LittleEndian, pi)
        if err != nil {
                fmt.Println("binary.Write failed:", err)
        }
        fmt.Printf("% x", buf.Bytes())
}
```

# type AppendByteOrder interfacer

```Go
type AppendByteOrder interface {
        AppendUint16([]byte, uint16) []byte
        AppendUint32([]byte, uint32) []byte
        AppendUint64([]byte, uint64) []byte
        String() string
}
```

AppendByteOrder 指定如何将 16 位、32 位或 64 位无符号整数附加到字节片中。

# type ByteOrder interface

```Go
package main

import (
        "encoding/binary"
        "fmt"
)

func main() {
        b := []byte{0xe8, 0x03, 0xd0, 0x07}
        x1 := binary.LittleEndian.Uint16(b[0:])
        x2 := binary.LittleEndian.Uint16(b[2:])
        fmt.Printf("%#04x %#04x\n", x1, x2)
}
// 0x03e8 0x07d0
```