---
title: "指令[8086]"
layout:  page
collection: "[语法目]"
date: 2017-09-22 19:00:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑....</a>

---
- **今日音乐**
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=286999&auto=0&height=66"></iframe>

---
> ...

---

<b style="color:red"></b>


[TOC]

#8086
---
###前提
---
> 先介绍一下常使用的非标准化的符号

我们应该会常见(ax),((ax))等等
仔细说一下
- 0层括号 取地址
- 1层括号 取数值
- 2曾括号 做指针

---
###分类
---
1. 数据传送指令
2. 算术运算指令
3. 逻辑运算和移位指令
4. 串操作指令
5. 控制转移指令
6. 处理器控制指令

> 粗略划分的话,分为三大类指令,数据传输,操作控制,数据运算

---
###数据传输(送)类
---
####分类
1. 通用数据传送指令
    `[1]. MOV[一般传送]`
    `[2]. PUSH/POP[堆栈操作]`
    `[3]. XCHG(Exchange)[交换]`
    `[4]. XLAT(Translate)[查表转换]`
2. 输入输出指令
    `[1]. IN`
    `[2]. OUT`
3. 目标地址传送指令
    `[1]. LEA(Load Effective Address)`
    `[2]. LDS(Load pointer using DS)`
    `[3]. LES(Load pointer using ES)`
4. 标志传送指令
    `[1]. LAHF(Load AH from Flags)`
    `[2]. SAHF(Store AH into Flags)`
    `[3]. PUSHF(Push Flags onto stack)`
    `[4]. POPF(Pop Flags off stack)`

---
###MOV
---
- 格式
    `mov dest, src; (src)->(dest)`
- 特点
    `[1]. 即可按字节(byte)传送,亦可按字[word]传送`
    `[2]. 可使用七大寻址方式`
    `[3]. 可以实现以下的传送`
    ` |____寄存器<-->(寄存器|存储器)`
    ` |____立即数 -->(寄存器|存储器)`
    ` |____段寄存器<-->(寄存器|存储器)`

- 注意
    `[1]. 不可以修改CS的值，即CS不可以作为[目的操作数],[原因]->CS存储的是当前所在段的段地址，一旦修改会造成麻烦.[这样做合法,但容易导致错误,因此不能这么做.]`
    `[2]. 传送立即数时，一定要注意和目的操作数的数据类型进行匹配。尤其是传送到存储器的时候`
    `[3]. 不可以在段寄存器间或内存单元间传送数据`
    `[4]. 不可以修改或读取IP和标志位寄存器的数值，因此有专门的标志位传送指令`
    `[5]. 不可以把立即数直接传送到段寄存器中[拒绝用户自己划分段]`

- EXTENSION
    `MOVSX & MOVZX[80386CPU]`

---
###PUSH/POP
---

- 格式
    `POP  DEST`
    `PUSH SRC`
- 特点
    `[1]. 操作的数据类型长度为16位[8086]`
    `[2]. push cs合法, pop cs不合法`
    `[3]. 栈底在高地址`
    `[4]. push压数据由高地址向低地址存放`
---


[至于先sp-=2还是先存先pop都是为了方便存储与扔出]

PUSH执行分两步
1. SP = SP - 2，SS:SP指向当前栈顶前面的单元，以当前栈顶前面的单元为新的栈顶
2. 将ax中的内容送到 SS:SP 指向的内存单元处，SS:SP 此时指向栈顶


---

<center style="position:relative; ">
![](https://raw.githubusercontent.com/srcmit/source/wiki/photo/push.png)
</center>

---

POP见图中

---

<center style="position:relative; ">
![](https://raw.githubusercontent.com/srcmit/source/wiki/photo/pop.png)
</center>

---



###XCHG(exchange)
---
- 目的
    `值交换`
- 格式
    `XCHG dest,src`
- 要求
    `寄存器<-->(存储器|寄存器)`
- 例子
    `XCHG BL,DL`
    `XCHG AX,SI`
    `XCHG COUNT[DI],AX`


---
###XLAT
---
- 目的
    `指定内存空间取值[查表转换]`
    `数制转换`
    `编码系统转换`
- 格式
    `XLAT SRC_TABLE`
- 效果
    `(AL)<--((BX)+(AL))`
- 例子
    `[Step1]. MOV BX, OFFSET SRC_TABLE`
    `[Step2]. MOV AL,0AH`
    `[Step3]. XLAT SRC_TABLE`
- 附加
    `有的人说,这种效果也可以使用其他指令实现,而且并不比你的这个执行效率差`
    `其实这个指令有一个很重要的前提,那就是*表*,没错,这个命令过于工程性,例如ascii码转换`

---

<center>
<b style='color:green;background-color:black'>
------------------------------------------------类型分界----------------------------------------------
</b>
</center>

---

> 输入输出指令注重的是外设与cpu的数据传送,他们都是累加器专用指令.

---
###IN/OUT
---
- 格式
    `IN ACC,PORT;(ACC)<--(PORT)`
- 作用
    `从io端口输入数据到累加器`
- 限制
    `port 的使用定义域{8位立即数,16位寄存器DX}`
    `ACC8位则读8位,ACC16位则读16位`
- 例子
    `IN AL,DATA;(AL)<--(DATA)//DATA直接寻址,但不需要加'[]'`
    `IN AX,DATA;(AX)<--((DATA)+1:(DATA))`
    `IN AL,BX;  (AL)<--((DX))`
    `IN AX,BX;  (AX)<--((DX)+1:(DX))`


- 说明
    `OUT除了数据传输方向与IN相反,规则不变`


---
###LEA(Load Effective Address)
----
- xu

---
###LDS(Load pointer using DS)
---
- 远地址传送
- 偏移-->指定寄存器
- 段--->DS

---
###LES(Load pointer using ES)
---
- 远地址传送
- 偏移-->指定寄存器
- 段--->ES




---
###LAHF(Load AH from Flags)
---
- Flags(低8位)--->AH

---
###SAHF(Store AH from Flags)
---

- AH---->Flags(低8位)

---
###PUSHF/POPF
---
- <b style='color:red'>数据传送指令里面只有这两个影响标识符寄存器</b>
- Flags 信息入栈[PUSHF]
- Flags 信息出栈[POPF]

```java
;使用
PUSH AX    ;PROTECT AX
PUSH CX    ;PROTECT CX
PUSHF      ;PROTECT Flags
CALL TRANS ;CALL PROCEDURE
POPF       ;RESTORE Flags
POP  CX    ;RESTORE CX
POP  AX    ;RESTORE AX
   .
   .
   .
```


##算数运算类
- 加
- 减
- 乘
- 除
- 转换

[注意]

- 操作记忆
    `操作数的类型[存储器][寄存器][立即数][段寄存器]`
    `操作数的长度[字][字节]`
    `副作用[影响Flags]`
- 段寄存器不能参加加减乘除运算

<b style='color:red'>[特别说明]:</b>算数运算类指令大都影响标识符寄存器,但是不同指令影响不同
- `+`&`-`(SF,ZF,AF,PF,CF,OF)
- `INC`&`DEC`(不影响CF[进位标识])
- `*`(改变CF和OF,但SZAP均不确定)
- `/`(大部分不确定,见详析)

---
###ADD
---
- dest(寄存器或存储器)
- source(立即数和寄存器和存储器)
- 不能同时为存储器
- 有符号数超出界限,置OF=1(溢出)
- 无符号数超出界限,置CF=1(进位)
- 数既是有符号数也是无符号数
- 可以进行字节操作和字操作

---
###ADC
---
- ADD dest, src
    `(dest)<---(dest)+(src)+(cf)`
- 可以进行字节操作和字操作
- 常用于多字节数据加法


---
###INC
---
- 长度[字][字节]
- 类型[寄存器][存储器]
- 副作用
    `S,Z,A,P,O`
    `NO CF !!!`
- 作用
    `修改循环程序中的地址`
- 例子

```java
  INC DL              ;8  BIT & REGISTER
  INC SI              ;16 BIT & REGISTER
  INC BYTE PTR[BX][SI];8  BIT & MEM
  INC WORD PTR[DI]    ;16 BIT & MEM
```

---
###AAA
---
[ASCII Adjust for Addition]

- 这里涉及编码了,数字为了显示方便使用了BCD码所以在使用ADD和ADC时注意单个字节值不要超过9(非压缩BCD)
- 类型
    `隐含操作数:[AL]和[AH]`
```java
    MOV AX, 0007H  ;(AH)<--00,(AL)<--07
    MOV BL, 08H    ;(BL)<--08
    ADD AL, BL     ;(AL)<--0FH
    AAA            ;(AL)<--05H,(AH)<--01H,(CF)=(AF)<--1

    AF 表示出现辅助进位
```

---
###DAA
---
```java
    MOV AX,0007H ;
    MOV BL,08H   ;
    ADD AL,BL    ;
    DAA          ;(AL)=15H,(AH)=00H,(AF)<-1,(CF)=0

    [CAUTION:]
    DAA 没有试图去操作 AH,但是 AAA 试图了!
    两者都比较关注单字节的运算产生的进位
    DAA 对单字节的进位通过 CF 的形式表达出来
    AAA 对单字节的进位通过修改 AH 的形式表达出来

```


---

减法指令:
- SUB[NORAML Subtraction]
- SBB[Subtraction with Borrow]
- DEC[]
- NEG[]
- CMP[]
- AAS[]
- DAS[]

---
###SUB
---
- 类型
    `[目标] 存储器 寄存器`
    `[源]   立即数 存储器 寄存器`
- 长度[字][字节]
- 无符号数不够借位 CF<-1[进位标志位]
- 有符号数得到负值 SF<-1[符号标志位]
- 有符号数结果溢出 OF<-1[溢出标志位]
    `负-负 可能溢出`

```java
由上面可以看出 有符号数运算影响 OF
             无符号数运算影响 CF
但是计算机并不知道是什么数,所以它认为一个数既为有符号数又为无符号数,这并不矛盾!因为值的含义并不重要,我们只是在乎规则!( ⊙ o ⊙ )我是形式主义?
```

---
###SBB
---

> 带借位减法

- 规则
    `SBB DEST,SRC  ;(DEST)<--(DEST)-(SRC)-(CF)`
- 长度 [字][字节]
- 类型 [同SBB]

---
###DEC
---
- RULE类似INC
- 长度类似INC
- 类型类似INC
- 副作用一样 不影响CF的

---
###NEG
---
[Negate]求补指令

- 很简单在补码表现形式上得到其相反数

---
###Problem
---

`暂时未解决`

- 为什么将段寄存器,ip,内部通信寄存器放在BIU
- 为什么内存与内存之间(段寄存器与段寄存器)不能传输数据

---
###FAQ
---
- 在执行指令期间，EU能直接访问存储器吗?为什么?
    `不能，因为在指令执行期间，执行指令所需的操作数由BIU从相应的内存区域或I\O端口中取出，传送给执行单元EU`
- 简述8086的分段机制
    `因为基址寄存器是16位的,所以可以有64k个节,每个节对应一个段基址,且每个段大小为16字节`
