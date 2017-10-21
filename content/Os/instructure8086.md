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

------------------------------------------------类型分界----------------------------------------------

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
