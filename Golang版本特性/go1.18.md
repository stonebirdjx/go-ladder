<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [泛型](#%E6%B3%9B%E5%9E%8B)
- [模糊测试](#%E6%A8%A1%E7%B3%8A%E6%B5%8B%E8%AF%95)
- [Slice扩容机制修改](#slice%E6%89%A9%E5%AE%B9%E6%9C%BA%E5%88%B6%E4%BF%AE%E6%94%B9)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

2022 年 3 月 15 日

Go 1.18 是一个大型版本，包括新功能、性能改进以及我们对语言有史以来最大的更改。 可以毫不夸张地说，Go 1.18 的部分设计是在十多年前我们首次发布 Go 时开始的。

泛型
在 Go 1.18 中，我们引入了对使用参数化类型的泛型代码的新支持。 支持泛型一直是 Go 最常被要求的功能，我们很自豪能够提供当今大多数用户所需的泛型支持。

模糊测试
在 Go 1.18 中，Go 成为第一个将模糊测试完全集成到其标准工具链中的主要语言。 与泛型一样，模糊测试已经设计了很长时间，我们很高兴通过此版本与 Go 生态系统分享它。

工作空间
Go 模块几乎已被普遍采用，并且 Go 用户在我们的年度调查中报告了非常高的满意度得分。 在我们的 2021 年用户调查中，用户认为模块最常见的挑战是跨多个模块工作。

20% 性能提升

Apple M1、ARM64 和 PowerPC64 用户有福了！ 由于将 Go 1.17 的寄存器 ABI 调用约定扩展到这些架构，Go 1.18 的 CPU 性能提高了高达 20%。

# 泛型

https://github.com/stonebirdjx/go-ladder/blob/master/golang%E6%B3%9B%E5%9E%8B.md

# 模糊测试

https://github.com/stonebirdjx/go-ladder/blob/master/golang%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95.md

# Slice扩容机制修改

```go
// 只关心扩容规则的简化版growslice
func growslice(old, cap int) int {
    newcap := old
    doublecap := newcap + newcap
    if cap > doublecap {
        newcap = cap
    } else {
        const threshold = 256 // 不同点1
        if old < threshold {
            newcap = doublecap
        } else {
            for 0 < newcap && newcap < cap {
                newcap += (newcap + 3*threshold) / 4 // 不同点2
            }
            if newcap <= 0 {
                newcap = cap
            }
        }
    }
    return newcap
}
```

首先是双倍容量扩容的最大阈值**从1024降为了256**，只要超过了256，就开始进行缓慢的增长。其次是增长比例的调整，之前超过了阈值之后，基本为恒定的1.25倍增长，