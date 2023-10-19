# Overview

[regexp](https://pkg.go.dev/regexp) 包实现正则表达式搜索。

接受的正则表达式的语法与 Perl、Python 和其他语言使用的通用语法相同。 更准确地说，它是 RE2 接受的语法，并在 https://golang.org/s/re2syntax 中进行了描述，但` \C` 除外。 

# Notice

使用前最好用compile 预编译一下，以用来提高性能

# :point_right:func Match

```Go
func Match(pattern string, b []byte) (matched bool, err error)
```

Match 报告字节片 b 是否包含正则表达式模式的任何匹配项。 更复杂的查询需要使用 Compile 和完整的 Regexp 接口。

## Example

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

# func MatchReader

```Go
func MatchReader(pattern string, r io.RuneReader) (matched bool, err error)
```

MatchReader 报告 RuneReader 返回的文本是否包含正则表达式模式的任何匹配项。 更复杂的查询需要使用 Compile 和完整的 Regexp 接口。

# :point_right:func MatchString

```Go
func MatchString(pattern string, s string) (matched bool, err error)
```

MatchString 报告字符串 s 是否包含正则表达式模式的任何匹配项。 更复杂的查询需要使用 Compile 和完整的 Regexp 接口。

## Example

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

# :point_right:func QuoteMeta

```Go
func QuoteMeta(s string) string
```

QuoteMeta 返回一个转义参数文本中所有正则表达式元字符的字符串； 返回的字符串是与文字文本匹配的正则表达式。

## Example

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

# type Regexp struct

```go
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

