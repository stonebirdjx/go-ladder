<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [tag 规则](#tag-%E8%A7%84%E5%88%99)
- [tag是Struct的一部分](#tag%E6%98%AFstruct%E7%9A%84%E4%B8%80%E9%83%A8%E5%88%86)
- [tag常见用法](#tag%E5%B8%B8%E8%A7%81%E7%94%A8%E6%B3%95)
- [方法](#%E6%96%B9%E6%B3%95)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

Go的struct声明允许字段附带Tag来对字段做一些标记。

# tag 规则

Tag本身是一个字符串，但字符串中却是：以空格分隔的 key:value 对。

- key: 必须是非空字符串，字符串不能包含控制字符、空格、引号、冒号。
- value: 以双引号标记的字符串

> 注意：冒号前后不能有空格

如下代码所示，如此写没有实际意义，仅用于说明Tag规则

```Go
type Server struct {
    ServerName string `key1: "value1" key11:"value11"`
    ServerIP   string `key2: "value2"`
}
```

# tag是Struct的一部分

文件路径：`src/reflect/type.go`

```Go
// A StructField describes a single field in a struct.
type StructField struct {
        // Name is the field name.
        Name string

        // PkgPath is the package path that qualifies a lower case (unexported)
        // field name. It is empty for upper case (exported) field names.
        // See https://golang.org/ref/spec#Uniqueness_of_identifiers
        PkgPath string

        Type      Type      // field type
        Tag       StructTag // field tag string
        Offset    uintptr   // offset within struct, in bytes
        Index     []int     // index sequence for Type.FieldByIndex
        Anonymous bool      // is an embedded field
}
```

# tag常见用法

常见的tag用法，主要是JSON、Bson数据解析、ORM映射、protobuf、http参数解析（header、query）等。

# 方法

```go
type Stonebird struct{}

func (s *Stonebird) GetName() string {
  return "stonebird"
}
```

