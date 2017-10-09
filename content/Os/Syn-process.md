---
title: "进程同步与互斥问题"
layout:  page
collection: "[策略目]"
date: 2017-09-23 17:09:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑....</a>

---
- **今日音乐**
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=286999&auto=0&height=66"></iframe>

---
> ...

---


[TOC]

###Intro[起因]
每个策略的引进都是为了解决所面临的一定的问题，同步策略的原因也是为了解决改善利用率，提高系统吞吐量，如同在算法学习中一样，无非是性能与功能的权衡，灵活性与复杂性的抉择。
权衡不久引入了同步机制，
<b style='color:red'>[补]:</b><b>同步本身就是一种通信</b>
<b style='color:red'>[补]:</b><b>因为同步可以实现互斥，所以互斥可以回归到同步中</b>
###四大准则
- 空闲让进
- 忙则等待
- 有限等待
- 让权等待
    `‘权’-->处理机`

**准则就类似于范式**

###同步机制
>宏观上的同步包括互斥和微观上的同步

####硬件同步机制
- 关中断
    `实现互斥`
    `自我认为关中断并不是为计算机提高资源利用准备的，因为关了中断不仅降低了计算机与外界的交互能力而且大大削弱了资源利用率`
- TS[Test-and-Set]

- SWAP

####信号量机制
> 发展:整型信号量->记录型信号量->信号量集
> 应用:单处理机，多处理机，计算机网络

- **整型信号量机制**

```c
  //P操作
  wait(S){
    while(S<=0); /*DO-NO-OP*/  /*注意出现死循环忙等*/
    S--;
  }
  //V操作
  signal(S){
    S++;
  }
```
有人看出来了当进程操作时如果s<=0,在这个时间片他就会一直问S<=0吗？什么都没有做，正所谓占着*不*,当时有一个学弟问我直接把wait改成`if(S>=0) S--;`不就行了嘛？WOC，脑回路清奇，你的意思是，当S<0时，即使我有进程也不做了，看见有人占着就跑路了？不过这种想法还是很好的，至少他是希望出现让权等待，但是不要为了让权等待让它失去了进程同步的意义，如果我们认真想的话，while实际上类似一种运行中的信息阻塞，我们不如引入阻塞，如果发现资源被占直接阻塞进程，当资源释放后，然后唤醒进程，那么问题来了，谁去唤醒，去哪唤醒？是一个很重要的问题，因为一旦P操作使进程A阻塞了，A便不具有消息通知的能力，这时候不要说轮询比较占用资源了，没人通知它，和上面的学弟说的有什么区别，所以我们应该打算，专门添加唤醒操作，在每个进程运行结束后，就去唤醒沉睡的进程们，至于去哪里唤醒这个很简单，申请一块公共空间(<span style='color:green'>喂我叫PCB<del>我怎么绿了</del></span>)存放沉睡的进程们，然后唤醒他们，其实这相当于去通知他们，signal()的由来也可以解释了，因为你不通知，他们都会阻塞到死...由此也可以看出PV为什么必须在一起！<del>[FFF!!!!]</del>

- **记录型信号量机制**

```c
//修正后的信号量机制
typedef struct {
  int value;//资源数量
  Struct PCB * list;//阻塞列表
}
wait(S){
  S->value--;
  if(S->value<=0) block(S->list);
}
signal(S){
  S->value++;
  if(S->value<=0) wakeup(S->list);
}
//改个名字吧，叫做记录型信号量
```
- AND型信号量
- 信号量集



####信号量的应用
- <b style="color:red">暂停中....</b>
- 实现互斥
```c
//先码为敬
semaphore mutex =1;
PA(){
  while (true) {
    wait(mutex);
    /*互斥资源代码*/
    signal(mutex);
  }
}
```
```c
PB(){
  while (true) {
    wait(mutex);
    /*互斥资源代码*/
    signal(mutex);
  }
}

```
- 实现前趋

---
###思索
---
> 这正是我最头痛的事情，在课堂上一直对同步机制建模，也许是大一修的离散数学基础不够好，我依旧没有将其通过通信原理映射到图论中，所以有点passive,但是不能有始无终，我仍然准备建立模型，通信原理抛了～，图论抛了～我只是准备用自己不成熟的思想去非合理描述同步机制在通信机制的解释

####杂.想

<b style='color:red'>[知识点1]:</b>前面提到的信号量本质是一种进程间的<b>异步通信方式</b>，进程不是通过轮询获得资源的，而是通过被通知的方式获得资源.
<b style='color:red'>[知识点2]:</b>通过在通信部分对<b>死锁</b>的原因探索可以得到很有用的信息
<b style='color:red'>[知识点3]:</b>二维的图形加上操作者[我]通过使用笔延伸通信数据流引入三维时间概念足以描述整个系统进程的运行。
<b style='color:red'>[知识点4]:</b>通过分析边界问题很容易发现死锁所在处

先放两张图

<center style="position:relative; ">
![](https://raw.githubusercontent.com/srcmit/source/wiki/photo/real.gif)
</center>

<center style="position:relative; ">
![](https://raw.githubusercontent.com/srcmit/source/wiki/photo/false.gif)
</center>




---
###外延
---
OK
