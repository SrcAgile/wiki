---
title: "瓶颈性能调优"
layout:  page
collection: "[工程目]"
date: 2017-02-10 09:00:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑....</a>

---
- **今日音乐**
歌曲名:<b style="color:red">生きていたんだよな</b>
由于版权保护无法贴上外链.
---

>工具可以细化探讨问题的粒度。 

---

<b style="color:red"></b>


[TOC]

###起源
- 在软件发布之后，软件管理人员若想良好的对软件进行升级，性能提升需要对软件运行过程中的各个部分进行有选择性的优化,这一过程对开发人员的基础知识要求十分高，涉及的之事广杂，细节繁琐，例如在大量代码的情况下分析`cpu bound`&`io bound`,若是io密集，需要从io子系统的各个方面出发，比如在应用层添加缓冲，尽量减少系统调用消耗的时间，修改pagecache的writeback设置，使用mmap对内核内存进行应用态的映射减少系统调用，或者对块设备的io调度进行优化，例如针对数据库的系统上不能使用常用的cfq而应该切换为deadline，针对固态硬盘使用noop， 还有例如`上下文切换`,`cpu migration`,`page fault`,`cache miss`,`brach miss`等等等等，这篇文章应该需要很长时间。

###背景知识
####关于处理器
- 需要熟悉掌握cache
- 需要知道流水线，超标量体系结构对brach miss的影响，涉及BTB等[指令分支预测](https://software.intel.com/en-us/articles/branch-and-loop-reorganization-to-prevent-mispredicts/)
    `不依赖上下文的指令和pipeline，避免指令出现上下文依赖`
- 了解PMU(performance monitor unit)
    `PMU 允许软件针对某种硬件事件设置 counter，此后处理器便开始统计该事件的发生次数，当发生的次数超过 counter 内设置的值后，便产生中断。比如 cache miss 达到某个值后，PMU 便能产生相应的中断。`

####Tracepoints
> 散落在内核源代码中的一些 hook，一旦使能，它们便可以在特定的代码被运行到时被触发，这一特性可以被各种 trace/debug 工具所使用。Perf 就是该特性的用户之一

比如说你想了解内核内存管理模块的行为，可以里用slab分配器的tracepoint hook，当然，我们可以
将hook来的数据进行分析生成报告便于诊断。所以可以方面了解程序的内部细节。
###工具
###perf

```
  You may need to install the following packages for this specific kernel:
    linux-tools-4.10.0-28-generic
    linux-cloud-tools-4.10.0-28-generic

  You may also want to install one of the following packages to keep up to date:
    linux-tools-generic
    linux-cloud-tools-generic
```
作用:
- 分析硬件事件（instructions retired ，processor clock cycles ）
- 分析软件事件（ Page Fault 和进程切换）
- 分析断点事件
- 函数级别采样
- 做 benchmark 衡量调度器的好坏
运作机制:
- 通过对软硬中断以及hook出现时候的回调产生信息，这三种情况作为三大类采样点，可以根据选用采样点的不同，来了解程序执行的各个部分在此采样点上的分布律(简单的说，采样点相当于概率论中所说的事件，通过选择对特定事件进行监测，从而得知程序的不同部分对事件触发的概率，概率和当然为1)

**常用命令**
1. perf list
    `列出所有的事件[Hardware Event],[Software Event],[Tracepoint event]`
2. perf stat[参考文章](http://www.lenky.info/archives/2012/10/2007)
```c
#hello.c
1 #include <stdio.h>
  2 void loop(){
  3     int count=0;
  4     for(int i=0;i<20;i++)
  5         count++;
  6 }
  7 void func1(){
  8     for(int i=0;i<100;i++)
  9         loop();
 10 }
 11 void func2(){
 12     for(int i=0;i<1000;i++)
 13         loop();
 14 }
 15 int main(){
 16     func1();
 17     func2();
 18     return 0;
 19 }

meta@meta:~/workstation/c/performace$ gcc -g hello.c -o hello
meta@meta:~/workstation/c/performace$ perf stat ./hello 2&> sys.data
 Performance counter stats for './hello 2':
 0.464171      task-clock (msec)         #    0.555 CPUs utilized          
 0             context-switches          #    0.000 K/sec                  
 0             cpu-migrations            #    0.000 K/sec                  
43             page-faults               #    0.093 M/sec                  
750,271        cycles                    #    1.616 GHz                    
555,199        instructions              #    0.74  insn per cycle         
112,590        branches                  #  242.561 M/sec                  
3,717          branch-misses             #    3.30% of all branches        

       0.000836144 seconds time elapsed
/*
第一列是数据，第二列是第一列数据的单位，第三列多为以时间为量度进行统计单位时间发生的频率
几个统计值的含义分别如下（）：
task-clock-msecs：
cpu运算在整个任务执行过程中消耗时间的比例，该值越高，说明花费越多时间在CPU计算上而非其它（比如I/O）；这里第一列数值为0.464171毫秒，而通过最后一行知道程序整个执行时间为0.000836144秒，所以百分比为0.464171 /(0.000836144×1000)=0.555，即第一行倒数第二列数值。
context-switches：记录程序在运行过程中发生的进程切换次数，应该避免频繁的进程切换；这里为0次。
CPU-migrations：  记录程序在运行过程中发生的CPU迁移次数，即被调度器从一个CPU转移到另外一个 CPU上运行；这里为0次。
page-faults：     记录程序在运行过程中发生的缺页中断次数。
cycles：          记录执行程序所发费的处理器周期数。
instructions：    记录执行程序所花费的指令数。
branches：        记录程序在运行过程中的分支指令数。
branch-misses：   记录程序在运行过程中的分支预测失败次数。
cache-references：记录程序在运行过程中的cache命中次数。
cache-misses：    记录程序在运行过程中的cache失效次数。
*/
meta@meta:~/workstation/c/performace$ perf stat -r n(次数) ./hello 2&> sys.data

perf record -e raw_syscalls:sys_enter ls
/*上述命令可以进行平均统计*/

```

3. perf top
```c
用过top的人都熟悉，这个命令可以根据三大类事件发生的频率进行排名你可以使用-e指定事件，至于事件选择哪个，可以通过`perf list`列出

meta@meta:~/workstation/c/performace$ sudo perf top -e cache-misses
PerfTop:88 irqs/sec kernel:64.8% exact:0.0% [3000Hz cache-misses],(all, 4 CPUs)
-------------------------------------------------------------------------------

    18.35%  perf                [.] 0x0000000000099837
     5.28%  libpthread-2.23.so  [.] __pthread_rwlock_rdlock
     4.58%  perf                [.] 0x00000000000a5441
     4.56%  perf                [.] 0x00000000000a5446
     2.55%  perf                [.] 0x00000000000992d3
     2.07%  [kernel]            [k] __bpf_prog_run
     1.67%  [kernel]            [k] clear_page_c_e
     1.49%  chromium-browser    [.] 0x000000000479b2b1
     1.45%  perf                [.] 0x00000000000bef81
     1.42%  [kernel]            [k] update_cfs_rq_load_avg.constprop.90
     1.32%  perf-25620.map      [.] 0x00002bba5db0bf60
     1.30%  libc-2.23.so        [.] __strchr_sse2
     1.16%  perf-7458.map       [.] 0x000016d9891b703f
     1.16%  chromium-browser    [.] 0x0000000002860517
     1.12%  [kernel]            [k] sys_futex
     1.07%  [kernel]            [k] filemap_map_pages
     1.07%  chromium-browser    [.] 0x0000000004957b80
     1.04%  [kernel]            [k] __switch_to

```
4. perf record
```c
/*上述的命令已经很不错了，但是列出的只是针对程序在硬件内核中某些事件执行的频率，针对程序进行分析时，我们要考虑单个函数级别在事件上的频率分布，perf可以进行函数级别查看*/
meta@meta:~/workstation/c/performace$ sudo perf record -e cpu-clock ./hello
meta@meta:~/workstation/c/performace$ sudo perf report
/*出错！can not open tips.txt
检查一下*/
meta@meta:~/workstation/c/performace$ sudo strace -f -e file perf report 2>&1 | grep tips.txt
access("/usr/share/doc/perf-tip/tips.txt", F_OK) = -1 ENOENT (No such file or directory)
access("/build/linux-hwe-vH8Hlo/linux-hwe-4.10.0/debian/build/tools-perarch/tools/perf/Documentation/tips.txt", F_OK) = -1 ENOENT (No such file or directory)
# (Cannot load tips.txt file, please install perf!)

/*bug:没解决这个问题*/

meta@meta:~/workstation/c/performace$ sudo perf record –e cpu-clock –g ./hello
-g选项可以显示调用栈
```
5. perf probe schedule:12 cpu 
```c
meta@meta:~/workstation/c/performace$ perf probe schedule:12 cpu 
meta@meta:~/workstation/c/performace$ perf record -f -e probe:schedule -a sleep 1 
/*上例利用 probe 命令在内核函数 schedule() 的第 12 行处加入了一个动态 probe 点，和 tracepoint 的功能一样，内核一旦运行到该 probe 点时，便会通知 perf。可以理解为动态增加了一个新的 tracepoint。*/
```
6. Perf sched
7. perf bench
8. perf lock
9. perf Kmem
10. perf timechart
11. firegraph
**参照**[perf2](https://www.ibm.com/developerworks/cn/linux/l-cn-perf2/)
**Instance**
1. 针对branch-misses进行优化[为什么有序数组在循环中速度比无序的快](https://stackoverflow.com/questions/11227809/why-is-it-faster-to-process-a-sorted-array-than-an-unsorted-array#11227902)

---

> 这里还是有点乱,不知道下面是否是从IO角度考虑进行优化.

### swap
```bash
# adjust swap
Linux提供了一个/proc/sys/vm/swappiness,只需要简单的echo 0-100 中的一个数值到这个文件中即可。值越大，系统swap到硬盘的越多.
```
> 网上看到一个2.6内核维护者，将它设置成 100。给出的理由：“我的观点是降低kernel swap出内存数据不对。你实际上不想让几百兆的内存在自己的机器内存中呆着，但从未被访问。所以把这些数据请到磁盘上，留出更多的主存给要用的程序”.

### page size
```c
# include <unistd.h>
int getpagesize();//get size of page
```