---
title: "AT&T汇编速览"
layout: page
collection: "[二代目]"
date: 2017-09-20 21:00:00
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---
> 为了完成操作系统也是蛮拼的

---
- **今日音乐**
```
暂无评论
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=31789216&auto=0&height=66"></iframe>

---
[TOC]

###前言
---
为了开发linux下的os,使用到了汇编语言,虽然对8086的汇编基本了解,但是众所周知这里我如果使用GCC+GAS的话必须了解AT&T汇编语言.

###80386寄存器
---

1. 16bit寄存器实际上是32bit寄存器的低16位
2. 8bit寄存器实际上是16bit寄存器`%ax,%bx,%cx,%dx`的低8位和高8位


|32bit|16bit|8bit|
|:---:|:---:|:--:|
|%eax|%ax|%ah|
|%ebx|%bx|%al|
|%ecx|%cx|%bh|
|%edx|%dx|%bl|
|%edi|%di|%ch|
|%esi|%si|%cl|
|%ebp|%bp|%dh|
|%esp|%sp|%dl|

3. 6个段寄存器
    - %cs(code)
    - %ds(data)
    - %ss(satck)
    - %es
    - %fs
    - %gs
4. 3个控制寄存器
    - %cr0(control)
    - %cr2
    - %cr3

5. 6个debug寄存器
    - %db0
    - %db1
    - %db2
    - %db3
    - %db6
    - %db7

5. 两个测试存储器
    - %tr6
    - %tr7
6. 8个浮点寄存器栈
    - %str(0)
    - %str(1)
    - %str(2)
    - %str(3)
    - %str(4)
    - %str(5)
    - %str(6)
    - %str(7)

###法
---
####句法
1. 左源右目的

####语法
##### 立即数`$num`
##### 寄存器`$regis`
##### symbol constant(符号常量)
```asm
  value: .long 0x12a3f2de
  movl value , %ebx#装0x12a3f2de->ebx
  movl $value, % ebx#装符号value的地址->ebx
```
##### length of Operator
    - b(byte 8-bit)`movb %al, %bl`
    - w(word 16-bits)`movw %ax, %bx`
    - l(long 32-bits) `movl %eax, %ebx`
<b>W</b>:如果没有指定操作数长度的话,编译期按照目标操作数的长度来设置,如果目标操作数无指定长度,编译器报错,例如 `push $4`


##### Sign and Zero Extension(符号扩展和零扩展)

`符号扩展指令和零扩展指令需要指定源操作数长度和目的操作数长度,即使在某些指令中这些操作数是隐含的。`

```
在 AT&T 语法中,符号扩展和零扩展指令的格式为,基本部分"movs"和"movz"(对应
Intel 语法的 movsx 和 movzx),后面跟上源操作数长度和目的操作数长度。 movsbl 意味着
movs (from) byte (to) long; movbw 意味着 movs (from) byte (to) word; movswl
意味着 movs (from)word (to)long。对于 movz 指令也一样。比如指令“movsbl %al,
%edx” 意味着将 al 寄存器的内容进行符号扩展后放置到 edx 寄存器中。
其它的 Intel 格式的符号扩展指令还有:
1: cbw -- sign-extend byte in %al to word in %ax;
2: cwde -- sign-extend word in %ax to long in %eax;
3: cwd -- sign-extend word in %ax to long in %dx:%ax;
4: cdq -- sign-extend dword in %eax to quad in %edx:%eax;
对应的 AT&T 语法的指令为 cbtw,cwtl,cwtd,cltd
```
##### Call and Jump
- 段内call+jump
    `call`
    `ret`
    `jmp`
- 段间call+jump
    `lcall`
    `lret`
    `ljmp`
##### Prefix(<b>操作码</b>前缀)
- . 字符串重复操作指令(rep,repne);
    `用来让字符串操作重复“%ecx” 次`
- . 指定被操作的段(cs,ds,ss,es,fs,gs);
- . 进行总线加锁(lock)
    `多处理器环境`
- .  指定地址和操作的大小(data16,addr16)
    `它们被用来在 32-bit 操作数/地址
代码中指定 16-bit 的操作数/地址`
```
在 AT&T 汇编语法中,操作码前缀通常被单独放在一行,后面不跟任何操作数。例如,
对于重复 scas 指令,其写法为:
repne
scas
```

##### Memory Reference
- Intel
    `section:[base+index*scale+displacement]`
- AT&
    `section:displacement(base,index,scale)`
```c
    EXAMPLE1:-4(%ebp)
    EXPLAIN1:
            base=%ebp,
            displacement=-4,
            section 没有指定,
            因base=%ebp故section=%ss,
            index,scale 没有指定,则 index 为 0

    EXAMPLE2:foo(,%eax,4)
    EXPLAIN2:
            index=%eax,
            scale=4,
            displacement=foo
            其它域没有指定,默认section=%ds

    EXAMPLE3:foo(,1)
    EXPLAIN3:
            这个表达式引用的是指针 foo 指向的地址所存放的值
            这是一种异常语法,但却合法

    EXAMPLE4:%gs:foo
    EXPLAIN4:
            表达式引用的是放置于%gs 段里变量 foo 的值

```

###内联汇编
---
- 暂停
