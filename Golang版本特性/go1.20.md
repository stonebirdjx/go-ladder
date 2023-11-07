<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [切片到数组的转换](#%E5%88%87%E7%89%87%E5%88%B0%E6%95%B0%E7%BB%84%E7%9A%84%E8%BD%AC%E6%8D%A2)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

2023 年 2 月 1 日

https://go.dev/blog/go1.20

我们非常高兴推出配置文件引导优化 (PGO) 预览版，它使编译器能够根据运行时配置文件信息执行特定于应用程序和工作负载的优化。

函数 SliceData、String 和 StringData 已添加到包 unsafe 中。 它们完成了独立于实现的切片和字符串操作的函数集。

Go 的类型转换规则已扩展为允许从切片到数组的直接转换。

语言规范现在定义了比较数组元素和结构字段的确切顺序。 这澄清了比较过程中出现恐慌时会发生什么。

覆盖工具现在可以收集整个程序的覆盖率配置文件，而不仅仅是单元测试的覆盖率配置文件。

标准库添加

- 新的 crypto/ecdh 包为 NIST 曲线和 Curve25519 上的椭圆曲线 Diffie-Hellman 密钥交换提供明确支持。
- 新函数errors.Join 返回一个包含错误列表的错误，如果错误类型实现了Unwrap() []error 方法，则可以再次获取该错误列表。
- 新的 http.ResponseController 类型提供对 http.ResponseWriter 接口未处理的扩展每个请求功能的访问。
  httputil.ReverseProxy 转发代理包含一个新的 Rewrite 挂钩函数，取代了之前的 Director 挂钩。
- 新的 context.WithCancelCause 函数提供了一种取消具有给定错误的上下文的方法。 可以通过调用新的 context.Cause 函数来检索该错误。
- 新的 os/exec.Cmd 字段 Cancel 和 WaitDelay 指定当关联的 Context 被取消或其进程退出时 Cmd 的行为。

提高性能

- 编译器和垃圾收集器的改进减少了内存开销，并将整体 CPU 性能提高了 2%。
- 专门针对编译时间的工作使构建改进了高达 10%。 这使构建速度恢复到与 Go 1.17 一致。

#  切片到数组的转换

```go
func slice2arrOK() {
	var sl = []int{1, 2, 3, 4, 5, 6, 7}
	var arr = [7]int(sl) // 切片到数组可以直接转换，不能超过切片的长度
	var parr = (*[7]int)(sl)
	fmt.Println(sl)  // [1 2 3 4 5 6 7]
	fmt.Println(arr) // [1 2 3 4 5 6 7]
	sl[0] = 11
	fmt.Println(arr)  // [1 2 3 4 5 6 7]
	fmt.Println(parr) // &[11 2 3 4 5 6 7]
}

func slice2arrPanic() {
	var sl = []int{1, 2, 3, 4, 5, 6, 7}
	fmt.Println(sl)
	var arr = [8]int(sl) // panic: runtime error: cannot convert slice with length 7 to array or pointer to array with leng  th 8
	fmt.Println(arr)     // &[11 2 3 4 5 6 7]

}

func main() {
	slice2arrOK()
	slice2arrPanic()
}
```

