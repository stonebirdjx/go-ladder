<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [func Register(value any)](#func-registervalue-any)
- [func RegisterName(name string, value any)](#func-registernamename-string-value-any)
- [type CommonType struct](#type-commontype-struct)
- [type Decoder struct](#type-decoder-struct)
  - [func NewDecoder(r io.Reader) *Decoder](#func-newdecoderr-ioreader-decoder)
  - [func (dec *Decoder) Decode(e any) error](#func-dec-decoder-decodee-any-error)
  - [func (dec *Decoder) DecodeValue(v reflect.Value) error](#func-dec-decoder-decodevaluev-reflectvalue-error)
- [type Encoder struct](#type-encoder-struct)
  - [func NewEncoder(w io.Writer) *Encoder](#func-newencoderw-iowriter-encoder)
  - [func (enc *Encoder) Encode(e any) error](#func-enc-encoder-encodee-any-error)
  - [func (enc *Encoder) EncodeValue(value reflect.Value) error](#func-enc-encoder-encodevaluevalue-reflectvalue-error)
- [type GobDecoder interface](#type-gobdecoder-interface)
- [type GobEncoder interface](#type-gobencoder-interface)
- [Examples](#examples)
  - [:point_right:basic](#point_rightbasic)
  - [:point_right: EncodeDecode](#point_right-encodedecode)
  - [:point_right: interface](#point_right-interface)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

gob 包管理 gob 流 - 在编码器（发送器）和解码器（接收器）之间交换的二进制值。<font color="red">一般是数据文件与golang对象之间的转换</font>。

Gob可以将Go语言的数据结构编码为二进制格式，以便在不同程序之间进行数据传输或持久化存储，并且可以将二进制数据解码为原始的Go语言对象。

# func Register(value any)

Register 在其内部类型名称下记录一个类型，由该类型的值标识。 该名称将标识作为接口变量发送或接收的值的具体类型。 仅需要注册将作为接口值的实现进行传输的类型。 预计仅在初始化期间使用，如果类型和名称之间的映射不是双射，则会出现panic。

# func RegisterName(name string, value any)

RegisterName 与 Register 类似，但使用提供的名称而不是类型的默认名称。

# type CommonType struct

```go
type CommonType struct {
	Name string
	Id   typeId
}
```

CommonType 保存所有类型的元素。 它是一个历史工件，保留是为了二进制兼容性，导出只是为了包的类型描述符编码的好处。 它不适合客户直接使用。

#  type Decoder struct

```go
type Decoder struct {
	mutex        sync.Mutex                              // each item must be received atomically
	r            io.Reader                               // source of the data
	buf          decBuffer                               // buffer for more efficient i/o from r
	wireType     map[typeId]*wireType                    // map from remote ID to local description
	decoderCache map[reflect.Type]map[typeId]**decEngine // cache of compiled engines
	ignorerCache map[typeId]**decEngine                  // ditto for ignored objects
	freeList     *decoderState                           // list of free decoderStates; avoids reallocation
	countBuf     []byte                                  // used for decoding integers while parsing messages
	err          error
}
```

解码器管理从连接的远程端读取的类型和数据信息的接收。 多个 goroutine 并发使用是安全的。

解码器仅对解码的输入大小进行基本的健全性检查，并且其限制不可配置。 解码来自不受信任来源的 gob 数据时要小心。

## func NewDecoder(r io.Reader) *Decoder

NewDecoder 返回一个从 io.Reader 读取的新解码器。 如果 r 没有同时实现 io.ByteReader，它将被包装在 bufio.Reader 中。

## func (dec *Decoder) Decode(e any) error

解码从输入流中读取下一个值并将其存储在空接口值表示的数据中。 如果 e 为零，则该值将被丢弃。 否则，e 的底层值必须是指向接收到的下一个数据项的正确类型的指针。 如果输入位于 EOF，则 Decode 返回 io.EOF 并且不修改 e。

## func (dec *Decoder) DecodeValue(v reflect.Value) error

DecodeValue 从输入流中读取下一个值。 如果 v 为零 Reflect.Value (v.Kind() == Invalid)，DecodeValue 会丢弃该值。 否则，它将值存储到 v 中。在这种情况下，v 必须表示指向数据的非零指针或者是可赋值的 Reflect.Value (v.CanSet()) 如果输入位于 EOF，则 DecodeValue 返回 io.EOF 并且 不修改 v。

# type Encoder struct

```go
type Encoder struct {
	mutex      sync.Mutex              // each item must be sent atomically
	w          []io.Writer             // where to send the data
	sent       map[reflect.Type]typeId // which types we've already sent
	countState *encoderState           // stage for writing counts
	freeList   *encoderState           // list of free encoderStates; avoids reallocation
	byteBuf    encBuffer               // buffer for top-level encoderState
	err        error
}
```

编码器管理类型和数据信息到连接另一端的传输。 多个 goroutine 并发使用是安全的。

## func NewEncoder(w io.Writer) *Encoder

NewEncoder 返回一个新的编码器，该编码器将在 io.Writer 上传输。

## func (enc *Encoder) Encode(e any) error

## func (enc *Encoder) EncodeValue(value reflect.Value) error

# type GobDecoder interface

```go
// GobDecoder is the interface describing data that provides its own
// routine for decoding transmitted values sent by a GobEncoder.
type GobDecoder interface {
	// GobDecode overwrites the receiver, which must be a pointer,
	// with the value represented by the byte slice, which was written
	// by GobEncode, usually for the same concrete type.
	GobDecode([]byte) error
}
```

GobDecoder 是描述数据的接口，它提供自己的例程来解码由 GobEncoder 发送的传输值。

# type GobEncoder interface

```go
type GobEncoder interface {
	// GobEncode returns a byte slice representing the encoding of the
	// receiver for transmission to a GobDecoder, usually of the same
	// concrete type.
	GobEncode() ([]byte, error)
}
```

GobEncoder 是描述数据的接口，它提供自己的编码值表示形式，以便传输到 GobDecoder。 实现 GobEncoder 和 GobDecoder 的类型可以完全控制其数据的表示，因此可能包含私有字段、通道和函数等内容，这些内容通常不能在 gob 流中传输。

注意：由于 gob 可以永久存储，因此保证 GobEncoder 使用的编码随着软件的发展而稳定是一个很好的设计。 例如，GobEncode 在编码中包含版本号可能是有意义的。

# Examples 

## :point_right:basic

```go
package main

import (
	"bytes"
	"encoding/gob"
	"fmt"
	"log"
)

type P struct {
	X, Y, Z int
	Name    string
}

type Q struct {
	X, Y *int32
	Name string
}

// This example shows the basic usage of the package: Create an encoder,
// transmit some values, receive them with a decoder.
func main() {
	// Initialize the encoder and decoder. Normally enc and dec would be
	// bound to network connections and the encoder and decoder would
	// run in different processes.
	var network bytes.Buffer        // Stand-in for a network connection
	enc := gob.NewEncoder(&network) // Will write to network.
	dec := gob.NewDecoder(&network) // Will read from network.

	// Encode (send) some values.
	err := enc.Encode(P{3, 4, 5, "Pythagoras"})
	if err != nil {
		log.Fatal("encode error:", err)
	}
	err = enc.Encode(P{1782, 1841, 1922, "Treehouse"})
	if err != nil {
		log.Fatal("encode error:", err)
	}

	// Decode (receive) and print the values.
	var q Q
	err = dec.Decode(&q)
	if err != nil {
		log.Fatal("decode error 1:", err)
	}
	fmt.Printf("%q: {%d, %d}\n", q.Name, *q.X, *q.Y)
	err = dec.Decode(&q)
	if err != nil {
		log.Fatal("decode error 2:", err)
	}
	fmt.Printf("%q: {%d, %d}\n", q.Name, *q.X, *q.Y)
}
// output
"Pythagoras": {3, 4}
"Treehouse": {1782, 1841}
```

## :point_right: EncodeDecode

```go
package main

import (
	"bytes"
	"encoding/gob"
	"fmt"
	"log"
)

// The Vector type has unexported fields, which the package cannot access.
// We therefore write a BinaryMarshal/BinaryUnmarshal method pair to allow us
// to send and receive the type with the gob package. These interfaces are
// defined in the "encoding" package.
// We could equivalently use the locally defined GobEncode/GobDecoder
// interfaces.
type Vector struct {
	x, y, z int
}

func (v Vector) MarshalBinary() ([]byte, error) {
	// A simple encoding: plain text.
	var b bytes.Buffer
	fmt.Fprintln(&b, v.x, v.y, v.z)
	return b.Bytes(), nil
}

// UnmarshalBinary modifies the receiver so it must take a pointer receiver.
func (v *Vector) UnmarshalBinary(data []byte) error {
	// A simple encoding: plain text.
	b := bytes.NewBuffer(data)
	_, err := fmt.Fscanln(b, &v.x, &v.y, &v.z)
	return err
}

// This example transmits a value that implements the custom encoding and decoding methods.
func main() {
	var network bytes.Buffer // Stand-in for the network.

	// Create an encoder and send a value.
	enc := gob.NewEncoder(&network)
	err := enc.Encode(Vector{3, 4, 5})
	if err != nil {
		log.Fatal("encode:", err)
	}

	// Create a decoder and receive a value.
	dec := gob.NewDecoder(&network)
	var v Vector
	err = dec.Decode(&v)
	if err != nil {
		log.Fatal("decode:", err)
	}
	fmt.Println(v)
}

// output
// {3 4 5}
```

## :point_right: interface

```go
package main

import (
	"bytes"
	"encoding/gob"
	"fmt"
	"log"
	"math"
)

type Point struct {
	X, Y int
}

func (p Point) Hypotenuse() float64 {
	return math.Hypot(float64(p.X), float64(p.Y))
}

type Pythagoras interface {
	Hypotenuse() float64
}

// This example shows how to encode an interface value. The key
// distinction from regular types is to register the concrete type that
// implements the interface.
func main() {
	var network bytes.Buffer // Stand-in for the network.

	// We must register the concrete type for the encoder and decoder (which would
	// normally be on a separate machine from the encoder). On each end, this tells the
	// engine which concrete type is being sent that implements the interface.
	gob.Register(Point{})

	// Create an encoder and send some values.
	enc := gob.NewEncoder(&network)
	for i := 1; i <= 3; i++ {
		interfaceEncode(enc, Point{3 * i, 4 * i})
	}

	// Create a decoder and receive some values.
	dec := gob.NewDecoder(&network)
	for i := 1; i <= 3; i++ {
		result := interfaceDecode(dec)
		fmt.Println(result.Hypotenuse())
	}

}

// interfaceEncode encodes the interface value into the encoder.
func interfaceEncode(enc *gob.Encoder, p Pythagoras) {
	// The encode will fail unless the concrete type has been
	// registered. We registered it in the calling function.

	// Pass pointer to interface so Encode sees (and hence sends) a value of
	// interface type. If we passed p directly it would see the concrete type instead.
	// See the blog post, "The Laws of Reflection" for background.
	err := enc.Encode(&p)
	if err != nil {
		log.Fatal("encode:", err)
	}
}

// interfaceDecode decodes the next interface value from the stream and returns it.
func interfaceDecode(dec *gob.Decoder) Pythagoras {
	// The decode will fail unless the concrete type on the wire has been
	// registered. We registered it in the calling function.
	var p Pythagoras
	err := dec.Decode(&p)
	if err != nil {
		log.Fatal("decode:", err)
	}
	return p
}
// output
// 5
// 10
// 15
```

