<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [CPU、内存与寄存器](#cpu%E5%86%85%E5%AD%98%E4%B8%8E%E5%AF%84%E5%AD%98%E5%99%A8)
- [通用寄存器](#%E9%80%9A%E7%94%A8%E5%AF%84%E5%AD%98%E5%99%A8)
- [伪寄存器](#%E4%BC%AA%E5%AF%84%E5%AD%98%E5%99%A8)
- [汇编程序指令](#%E6%B1%87%E7%BC%96%E7%A8%8B%E5%BA%8F%E6%8C%87%E4%BB%A4)
  - [宏](#%E5%AE%8F)
  - [汇编常量](#%E6%B1%87%E7%BC%96%E5%B8%B8%E9%87%8F)
  - [DATA - 初始化变量](#data---%E5%88%9D%E5%A7%8B%E5%8C%96%E5%8F%98%E9%87%8F)
  - [GLOBL - 全局变量](#globl---%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F)
  - [TEXT - 函数](#text---%E5%87%BD%E6%95%B0)
- [常见操作指令=](#%E5%B8%B8%E8%A7%81%E6%93%8D%E4%BD%9C%E6%8C%87%E4%BB%A4)
  - [MOVQ - 搬运长度](#movq---%E6%90%AC%E8%BF%90%E9%95%BF%E5%BA%A6)
  - [LEAQ - 将有效地址加载到指定的地址寄存器中](#leaq---%E5%B0%86%E6%9C%89%E6%95%88%E5%9C%B0%E5%9D%80%E5%8A%A0%E8%BD%BD%E5%88%B0%E6%8C%87%E5%AE%9A%E7%9A%84%E5%9C%B0%E5%9D%80%E5%AF%84%E5%AD%98%E5%99%A8%E4%B8%AD)
  - [ADDQ - 减法](#addq---%E5%87%8F%E6%B3%95)
  - [SUBQ - 减法](#subq---%E5%87%8F%E6%B3%95)
  - [IMULQ - 无符号乘法](#imulq---%E6%97%A0%E7%AC%A6%E5%8F%B7%E4%B9%98%E6%B3%95)
  - [IDIVQ - 无符号除法](#idivq---%E6%97%A0%E7%AC%A6%E5%8F%B7%E9%99%A4%E6%B3%95)
  - [CMP - 比较](#cmp---%E6%AF%94%E8%BE%83)
  - [AND，OR，XOR - 位运算](#andorxor---%E4%BD%8D%E8%BF%90%E7%AE%97)
  - [JMP - 无条件跳转](#jmp---%E6%97%A0%E6%9D%A1%E4%BB%B6%E8%B7%B3%E8%BD%AC)
  - [JZ，JLS - 有条件跳转](#jzjls---%E6%9C%89%E6%9D%A1%E4%BB%B6%E8%B7%B3%E8%BD%AC)
  - [CALL - 调用函数](#call---%E8%B0%83%E7%94%A8%E5%87%BD%E6%95%B0)
  - [RET - 返回](#ret---%E8%BF%94%E5%9B%9E)
  - [leave - 恢复调用者者栈指针](#leave---%E6%81%A2%E5%A4%8D%E8%B0%83%E7%94%A8%E8%80%85%E8%80%85%E6%A0%88%E6%8C%87%E9%92%88)
- [寻址模式](#%E5%AF%BB%E5%9D%80%E6%A8%A1%E5%BC%8F)
- [标志位](#%E6%A0%87%E5%BF%97%E4%BD%8D)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

Go 语言使用的 Plan 9 汇编。

以输出`hello，world`为例，Plan 9 汇编代码的整体逻辑同样是分了 Data 段、Text 段，同样是用 mov 等指令给寄存器赋值。

```
#include "textflag.h"

DATA  msg<>+0x00(SB)/8, $"Hello, W"
DATA  msg<>+0x08(SB)/8, $"orld!\n"
GLOBL msg<>(SB),NOPTR,$16

TEXT ·PrintMe(SB), NOSPLIT, $0
	MOVL 	$(0x2000000+4), AX 	 // write 系统调用数字编号 4
	MOVQ 	$1, DI 			      // 第 1 个参数 fd
	LEAQ 	msg<>(SB), SI 		// 第 2 个参数 buffer 指针地址
	MOVL 	$16, DX 		        // 第 3 个参数 count
	SYSCALL
	RET
```

go获取汇编代码

```bash
go tool compile -S main.go
# or
go build -gcflags -S main.go
```

# CPU、内存与寄存器

汇编主要是跟 CPU 和内存打交道，CPU 本身只负责运算，不负责存储，数据存储一般都是放在内存中。

我们知道 CPU 的运算速度远高于内存的读写速度，为了 CPU 不被内存读写拖后腿，CPU 内部引入**一级缓存、二级缓存和寄存器**的概念，这些资源都非常宝贵，**寄存器可以认为是在 CPU 内可以存储非常少量数据的超高速的存储单元**。因为寄存器个数有限且非常重要，每个寄存器都有自己的名字，最常用的有以下。

```
%EAX %EBX %ECX %EDX %EDI %ESI %EBP %ESP
```

# 通用寄存器

plan 9 汇编的通用寄存器包括以下寄存器。

```
AX BX CX DX DI SI BP SP R8 R9 R10 R11 R12 R13 R14 PC
```

plan9 中使用寄存器**不需要带 r 或 e 的前缀**，例如 rax，只要写 AX 就可以了。

```
eax->AX
ebx->BX
ecx->CX
```

plan9 汇编语言提供了如下映射，在汇编语言中直接引用就可使用物理寄存器了。

| amd64 | rax  | rbx  | rcx  | rdx  | rdi  | rsi  | rbp  | rsp  | r8   | r9   | r10  | r11  | r12  | r13  | r14  | rip  |
| ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Plan9 | AX   | BX   | CX   | DX   | DI   | SI   | BP   | SP   | R8   | R9   | R10  | R11  | R12  | R13  | R14  | PC   |

# 伪寄存器

伪寄存器并不是真正的寄存器，而是虚拟寄存器（由工具链维护），例如帧指针。

Go 汇编引入了四个伪寄存器，这四个伪寄存器非常重要：

- FP(Frame pointer)-帧指针：用来访问函数的参数
- PC(Program counter)-程序计数器：用于分支和跳转
- SB(Static base pointer)-静态基础指针：一般用于声明函数或者全局变量
- SP(Stack pointer)-栈指针：指向当前栈帧的局部变量的开始位置，一般用来引用函数的局部变量

所有用户定义的符号都作为偏移量写入伪寄存器 FP 和 SB。

汇编代码中需要表示用户定义的符号(变量)时，可以通过 SP 与偏移还有变量名的组合，比如`x-8(SP)` ，因为 SP 指向的是栈顶，所以偏移值都是负的，`x`则表示变量名

# 汇编程序指令

全局数据符号由一系列以 DATA 指令起始和一个 GLOBL 指令定义。

## 宏

在汇编文件中可以定义、引用宏。通过`#define get_tls(r)  MOVQ TLS, r`类似语句来定义一个宏，语法结构与C语言类似；通过`#include "textflag.h"`类似语句来引用一个外部宏定义文件。

```
type vdsoVersionKey struct {
	version string
	verHash uint32
}

#define vdsoVersionKey__size 24
#define vdsoVersionKey_version 0
#define vdsoVersionKey_verHash 16

MOVQ vdsoVersionKey_version(DX) AX
MOVQ (vdsoVersionKey_version+vdsoVersionKey_verHash)(DX) AX
```

## 汇编常量

```
$1           # 十进制
$0xf4f8fcff  # 十六进制
$1.5         # 浮点数
$'a'         # 字符
$"abcd"      # 字符串

$2+2      # 常量表达式
$3&1<<2   # == $4
$(3&1)<<2 # == $4
```

## DATA - 初始化变量

DATA 命令用于初始化变量，格式`DATA symbol+offset(SB)/width, value` ,在给定的 offset 和 width 处初始化该符号的内存为 value。 DATA 必须使用增加的偏移量来写入给定符号的指令。

```
DATA  msg<>+0x00(SB)/8, $"Hello, W"
DATA  msg<>+0x08(SB)/8, $"orld!\n"
```

## GLOBL - 全局变量

GLOBL 指令声明符号是全局的。参数是可选标志，并且数据的大小被声明为全局， **除非 DATA 指令已初始化，否则初始值将全为零**。该 GLOBL 指令必须遵循任何相应的 DATA 指令。

```
GLOBL msg<>(SB),NOPTR,$16
```

注意到 msg 后面有一个`<>`，这表示这个全局变量只在当前文件中可以被访问

```
GLOBL divtab<>(SB), RODATA, $64
```

这条给变量`divtab<>`加了一个 flag `RODATA` ，表示里边存的是只读变量，最后的`$64`表示的是这个变量占用了 64 字节的空间。

## TEXT - 函数

TEXT用于函数定义，表示是汇编中的 .text 分段

```
TEXT pkgname·funname(SB),flag,$framesize-argsize
```

`pkgname` :可以省略，最好省略。不然修改包名还要级联修改；

`funname`: 声明的函数名

`flag`: 标志位，如 `NOSPLIT`，我们知道 Go Runtime 会追踪每个 stack 的使用情况，然后动态自增。而`NOSPLIT `标志位禁止检查，节省开销，但是写程序的人要保证这个函数是安全的。

- NOPROF = 1 （对于TEXT项目。）不要分析标记的功能。此标志已弃用。
- DUPOK = 2 在单个二进制文件中具有此符号的多个实例是合法的。链接器将选择要使用的重复项之一。
- NOSPLIT = 4 （对于TEXT项。）请勿插入序言以检查是否必须拆分堆栈。例程的框架及其所调用的任何内容都必须适合堆栈段顶部的备用空间。用于保护例程，例如堆栈拆分代码本身。
- RODATA = 8 （对于DATA和GLOBL项目。）将此数据放在只读部分中。
- NOPTR = 16 （对于DATA和GLOBL项目。）此数据不包含任何指针，因此不需要由垃圾收集器进行扫描。 包裹= 32 （对于TEXT项目。）这是包装函数，不应视为禁用恢复。
- NEEDCTXT = 64 （对于TEXT项目。）此函数是一个闭包，因此它使用其传入的上下文寄存器。

`framesize`: 函数栈帧大小 = 局部变量 + 调用其它函数参数空间的总大小

`argsize`: 一些参考资料说这里是 **参数+返回值**大小

```
TEXT runtime·profileloop(SB),NOSPLIT,$8  # $8 指运行时占8字节空间(栈帧大小)
	MOVQ	$runtime·profileloop1(SB), CX
	MOVQ	CX, 0(SP)
	CALL	runtime·externalthreadhandler(SB)
	RET
```

上边整段汇编代码称为一个`TEXT block` ，`runtime.profileloop(SB)`后边有一个`NOSPLIT` flag，紧随其后的`$8`表示`frame size`

> 通常 `frame size` 的构成都是形如`$24-8` (中间的`-`只起到分隔的作用)，表示的是这个`TEXT block` 运行的时候需要占用 24 字节空间，参数和返回值要额外占用 8 字节空间（这 8 字节占用的是调用方栈帧里的空间）

# 常见操作指令=

指令有几大类，**一类是用于数据移动的**，**一类是用于跳转的**（无条件跳转，有条件跳转）。**一类是用于逻辑运算和算术运算**。

指令后缀Q说明是64位上的汇编指令

[指令查询](https://github.com/golang/arch/blob/master/x86/x86.csv)

## MOVQ - 搬运长度

`MOV` 指令：其后缀表示搬运长度, `$NUM` 表示具体的数字，如下面例子

```
MOVB $1, DI      	// 1 byte,  DI=1
MOVW $0x10, BX   	// 2 bytes, BX=10
MOVD $1, DX      	// 4 bytes, DX=1
MOVQ $-10, AX     // 8 bytes, AX=-10
```

MOV 指令有有好几种后缀 MOVB MOVW MOVL MOVQ 分别对应的是 1 字节 、2 字节 、4 字节、8 字节

## LEAQ - 将有效地址加载到指定的地址寄存器中

```
// ret+24(FP) 这代表了第三个函数参数，是个地址
LEAQ	ret+24(FP), AX	// 把 ret+24(FP) 地址移到 AX 寄存器中
```

## ADDQ - 减法

`ADD` 加法计算指令

```
ADDQ  AX, BX   // BX += AX
# 栈空间: 高地址向低地址
ADDQ $0x18, SP // 对 SP 做加法，清除函数栈帧
```

## SUBQ - 减法

`SUB`减法计算指令

```
SUBQ  AX, BX   # BX -= AX
# 栈空间: 高地址向低地址
SUBQ $0x18, SP # 对 SP 做减法，为函数分配函数栈帧
```

## IMULQ - 无符号乘法

```
IMULQ AX, BX   # BX *= AX
```

## IDIVQ - 无符号除法

## CMP - 比较

## AND，OR，XOR - 位运算

## JMP - 无条件跳转

`JMP` 指令**无条件**跳转到目标地址，该地址用代码标号来标识，并被汇编器转换为偏移量

```
JMP addr   // 跳转到地址，地址可为代码中的地址，不过实际上手写不会出现这种东西
JMP label  // 跳转到标签，可以跳转到同一函数内的标签位置
JMP 2(PC)  // 以当前指令为基础，向前/后跳转 x 行
JMP -2(PC) // 同上
```

## JZ，JLS - 有条件跳转

```
# 有条件跳转
JZ target // 如果 zero flag 被 set 过，则跳转
JLS num		// 如果上一行的比较结果，左边小于右边则执行跳到 num 地址处
```

## CALL - 调用函数

```
CALL runtime.printnl(SB)
```

## RET - 返回

return

## leave - 恢复调用者者栈指针

# 寻址模式

汇编语言的一个很重要的概念就是它的寻址模式，Plan 9 汇编也不例外，它支持如下寻址模式：

```
R0              数据寄存器
A0              地址寄存器
F0              浮点寄存器
CAAR, CACR, 等  特殊名字
$con            常量
$fcon           浮点数常量
name+o(SB)      外部符号
name<>+o(SB)    局部符号
name+o(SP)      自动符号
name+o(FP)      实际参数
$name+o(SB)     外部地址
$name<>+o(SB)   局部地址
(A0)+           间接后增量
-(A0)           间接前增量
o(A0)
o()(R0.s)

symbol+offset(SP) 引用函数的局部变量，offset 的合法取值是 [-framesize, 0)
    局部变量都是 8 字节，那么第一个局部变量就可以用 localvar0-8(SP) 来表示

如果是 symbol+offset(SP) 形式，则表示伪寄存器 SP
如果是 offset(SP) 则表示硬件寄存器 SP
```

# 标志位

| 助记符 | 名字     | 用途                                                  |
| ------ | -------- | ----------------------------------------------------- |
| OF     | 溢出     | 0为无溢出 1为溢出                                     |
| CF     | 进位     | 0为最高位无进位或错位 1为有                           |
| PF     | 奇偶     | 0表示数据最低8位中1的个数为奇数，1则表示1的个数为偶数 |
| AF     | 辅助进位 |                                                       |
| ZF     | 零       | 0表示结果不为0 1表示结果为0                           |
| SF     | 符号     | 0表示最高位为0 1表示最高位为1                         |

