# Overview

关键字 range 帮助我们快速遍历 字符串， 数组、切片、哈希表以及 Channel 等集合类型。

# 经典三段式

```Go
for init; cond; end{
    Body
}

// 一段
for cond {
}

// 永久循环
for {
}
```

- 初始化循环的 init；
- 循环的继续条件 cond；
- 循环体结束时执行的 end；
- 循环体 Body：

# range

range 右边表达式可以是以下这些数据类型：

- 数组
- 指向数组的指针
- 切片
- 字符串
- map
- 可以接收传输的 channel, 比如：`chan int` or `chan<- int`

```Go
for idx := range slice{}
for idx, v := range slice{}
for range slice{}
```

> 如果v 是结构体，func，或其他需要推断的引用类型，有v的请求性能会慢

## 数组和切片

```Go
// array 
var arr := [...]int{1,2,3,4,5,6,7,8}
for idx,v := range arr {
}

// slice 
var s = make([]int,8)
for idx,v := range s{
}
```

编译器源码

```Go
// The loop we generate:
//   for_temp := range
//   len_temp := len(for_temp)
//   for index_temp = 0; index_temp < len_temp; index_temp++ {
//           value_temp = for_temp[index_temp]
//           index = index_temp
//           value = value_temp
//           original body
//   }
```

- range遍历的数组和切边的副本，不影响遍历次数
- 如果数组 非指针 遍历 如果取v，遍历过程中改变遍历的值，数组底层不共享，遍历过程中取得原来的值。
- slice底层数据共享，改变值取得是新值。

## map

```Go
m := make(map[string]string)

for key,v := range m{
}
```

编译器源码

```Go
// The loop we generate:
//   var hiter map_iteration_struct
//   for mapiterinit(type, range, &hiter); hiter.key != nil; mapiternext(&hiter) {
//           index_temp = *hiter.key
//           value_temp = *hiter.val
//           index = index_temp
//           value = value_temp
//           original body
//   }
```

map底层是hash表，删除更新key影响map的遍历

## channel

必须是可以接收传输的chan 

```Go
ch := make(chan struct{})

for range ch{
    //read
}
```

编译器源码

```Go
// The loop we generate:
//   for {
//           index_temp, ok_temp = <-range
//           if !ok_temp {
//                   break
//           }
//           index = index_temp
//           original body
//   }
```

channel遍历是依次从channel中读取数据,读取前是不知道里面有多少个元素的。如果channel中没有元素，则会阻塞等待，如果channel已被关闭，则会解除阻塞并退出循环。

注：

上述注释中index_temp实际上描述是有误的，应该为value_temp，因为index对于channel是没有意义的。

使用for-range遍历channel时只能获取一个返回值。

## string

```Go
var s = "stonebird"
for idx ,r := range s {
    // rune
}

// byte
for i := 0; i<8 ; i++ {
    b := s[i]
}
```

# range 性能

range 在迭代过程中返回的是迭代值的拷贝，如果每次迭代的元素的内存占用很低，那么 for 和 range 的性能几乎是一样，例如 []int。

但是如果迭代的元素内存占用较高，例如一个包含很多属性的 struct 结构体，那么 for 的性能将显著地高于 range

如果使用 range，建议只迭代下标，通过下标访问迭代值，这种使用方式和 for 就没有区别了。