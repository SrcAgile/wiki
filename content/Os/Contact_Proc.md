---
title: "进程通信"
layout:  page
collection: "[系统纲]"
date: 2017-09-06 21:00:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑....</a>

---
- **今日音乐**
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=286999&auto=0&height=66"></iframe>

---
> 个人感觉通信最能体现面向对象特性，每个组件相互独立，相互通信

---



[TOC]

##intro
- 通信方式

---
###通信方式
---
1. 同步
2. 异步

####应用
- 中断:异步
- 异常:同步
- 系统调用: 同步或异步


IA-32 Intel ® Architecture

Software Developer’s Manual Volume 1: Basic Architecture给出的解释:

6.4.INTERRUPTS AND EXCEPTIONS
The processor provides two mechanisms for interrupting program execution: interrupts and exceptions:An interrupt is an asynchronous event that is typically triggered by an I/O device.An exception is a synchronous event that is generated when the processor detects one or more predefined conditions while executing an instruction. The IA-32 architecture specifies three classes of exceptions: faults, traps, and aborts.
The processor responds to interrupts and exceptions in essentially the same way. When an interrupt or exception is signaled, the processor halts execution of the current program or task and switches to a handler procedure that has been written specifically to handle the interrupt or exception condition.
 The processor accesses the handler procedure through an entry in the interrupt descriptor table (IDT). When the handler has completed handling the interrupt or exception,program control is returned to the interrupted program or task.
The operating system, executive, and/or device drivers normally handle interrupts and exceptions independently from application programs or tasks. Application programs can, however,access the interrupt and exception handlers incorporated in an operating system or executive through assembly-language calls. The remainder of this section gives a brief overview of theprocessor’s interrupt and exception handling mechanism. See Chapter 5, Interrupt and Exception Handling in the IA-32 Intel Architecture Software Developer’s Manual, Volume 3, for a detailed description of this mechanism.
The IA-32 Architecture defines 17 predefined interrupts and exceptions and 224 user defined interrupts, which are associated with entries in the IDT. Each interrupt and exception in the IDT is identified with a number, called a vector. Table 6-1 lists the interrupts and exceptions with entries in the IDT and their respective vector numbers. Vectors 0 through 8, 10 through 14, and16 through 19 are the predefined interrupts and exceptions, and vectors 32 through 255 are the user-defined interrupts, called mask-able interrupts.
<b>定义</b>：
中断和异常是强制性的执行流的转移，从当前正在执行的程序或任务转移到一个特殊的例程或任务。当处理器收到中断信号或检测到异常时，便挂起当前正在运行的进程或者任务，而转去执行中断或异常处理例程。中断或异常处理例程执行完毕之后，处理器继续执行被中断的进程或任务。

 <b>分类</b>：
<b>中断<b>：又称为异步中断，是其他硬件依照CPU时钟信号随机产生的。中断又被分为可屏蔽硬件中断和不可屏蔽中断。在微机原理课程中，处理器中有两个引脚NMI和INTR负责接受中断信号，还有高级可编程中断控制器（APIC），如8259A管理中断信号。则可屏蔽硬件中断：任何通过INTR或着局部APIC传递到处理器的中断信号都被称为可屏蔽硬件中断，由IO设备产生的IRQ（Interrupt ReQuest）也是可屏蔽硬件中断。但是通过INTR引脚传递的可屏蔽硬件中断可使用Intel架构定义的中断向量（0-255），而局部的APIC传递的部分只能使用16-255号向量。若中断信号从NMI引脚传递过来，则发生的是一个不可屏蔽中断。

<b>异常</b>：又称为同步中断，是当指令执行时CPU控制单元产生的，之所以称为同步，是因为只有在一条指令终止执行后CPU才会发出中断。在不失进程执行连续性的同时，按引起的异常的指令是否能重新执行,且依据它们被报告的方式，异常分为错误，陷阱，和终止三种情况。

<b>错误</b>：错误是一种通常可以能够被修正的异常，一旦修正，程序能够不失去连续性地接着执行。当报告错误发生时，处理器将机器状态恢复到执行错误之前的状态。错误处理例程的返回地址指向产生错误的指令，而不是错误指令之后的的那条指令。如页错误。

<b>陷阱</b>：当引起陷阱的指令发生时，马上产生该异常。陷阱允许程序不失去连续性的继续执行。陷阱处理例程的返回地址指向引起陷阱的指令的下一条指令（与错误本质上的区别）。如溢出。

<b>终止</b>：它并不总是报告产生异常的指令的确定位置，也不允许引起终止的进程或任务重新执行。如总线错误导致异常终止。

<b>中断向量</b>：
Intel x86共支持256中向量中断，Intel给每种中断源编号，从0-255,并把这个无符号整数叫做一个中断向量。不可屏蔽中断的向量和异常的向量是固定的，而可屏蔽的硬件中断可以通过对中断控制器编程来改变。

Linux中的中断向量：

0-19的中断向量对应于异常和非屏蔽中断。

20-31Intel保留

32-127可屏蔽硬件中断

128用于系统调用的可编程异常

129-238可屏蔽硬件中断

239本地APIC时钟中断

240本地APIC高温中断

241-250由Linux留作将来使用

251-253处理器间中断

254本地APIC错误中断

255本地APIC伪中断(CPU屏蔽某个中断时产生的)

中断描述符表：
中断描述符表（Interrupt Descriptor Table，IDT）是一个系统表，它与每一个中断或异常向量相关，每一个向量在表中有相应的中断或异常处理程序的入口地址。内核在允许中断发生前，必须适当地初始化IDT，用lidt汇编指令初始化idtr。

任务门：当中断信号发生时，必须取代当前进程的那个进程的TSS选择符存放在任务门中。

中断门：包含段选择符和中断或异常处理程序的段内偏移量。当控制权转移到一个适当的段时，处理器清IF标志，从而关闭将来会发生的可屏蔽中断。

陷阱门：与中断门相似，只是控制权传递到一个适当的段时处理器不修改IF标志。

Linux利用中断门处理中断，利用陷阱门处理异常。
