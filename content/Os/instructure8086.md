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
