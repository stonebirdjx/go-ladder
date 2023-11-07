<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
  - [Standard library additions](#standard-library-additions)
- [min、max和clear](#minmax%E5%92%8Cclear)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

2023 年 8 月 8 日

https://go.dev/blog/go1.21

工具改进
我们宣布在 1.20 中预览的配置文件引导优化 (PGO) 功能现已全面推出！ 如果主包目录中存在名为 default.pgo 的文件，go 命令将使用它来启用 PGO 构建。

我们测量了 PGO 对多种 Go 程序的影响，发现性能提高了 2-7%。

语言变化

- 新的内置函数：min、max和clear。
- 对泛型函数的类型推断进行了多项改进。 规范中类型推断的描述已得到扩展和澄清。

## Standard library additions

- New [log/slog](https://go.dev/pkg/log/slog) package for structured logging.
- New [slices](https://go.dev/pkg/slices) package for common operations on slices of any element type. This includes sorting functions that are generally faster and more ergonomic than the [sort](https://go.dev/pkg/sort) package.
- New [maps](https://go.dev/pkg/maps) package for common operations on maps of any key or element type.
- New [cmp](https://go.dev/pkg/cmp) package with new utilities for comparing ordered values.



# min、max和clear

