---
title: "随机算法"
layout:  page
collection: "[CLRS]"
date: 2017-09-24 16:19:32
---

**文档状态：**<a style="color:red;background-color:gray">编辑....</a>

---
- **今日音乐**
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=27845056&auto=0&height=66"></iframe>

---
>

---


[TOC]

##开篇
### 趣味知识

$$ (1+1)^n=C\begin{matrix}
0 & \\\n
\end{matrix}+C\begin{matrix}
1 & \\\n
\end{matrix}+C\begin{matrix}
2 & \\\n
\end{matrix}+\cdots+C\begin{matrix}
n & \\\n
\end{matrix}$$

主要是今天在硬件课程上发现如何获得一个含有n个字符的系统的所有状态的数量,这样说可能不清楚,用OJ的方式吧
```java
INPUT:1,2,3,4
OUTPUT:{1}
       {2}
       {3}
       {4}
       {1,2}
       {1,3}
       {1,4}
       {2,3}
       {2,4}
       {3,4}
       {1,2,3}
       {1,2,4}
       ...
       ...
       ...
       {1,2,3,4}
#注意输出是集合,集合具有无序性
答案就是 2^n-1
当然这只是初中数学知识,让我写下来的原因是二进制的无处不在...
```

###主题

>引理5.4,the flowing code will generate random sequeue
>prove:skip

```java
#pseudcode 5.4
permute-by-sorting(A)
  n = A.length;
  let P[1..n] be a new array;
  for i=1 to n
    P[i]=Random(1,n^2);
  sort A,using P as sort keys;
```

>引理5.5 Randomize-in-place可以产生随机序列
> prove:loop invariant

```Python
#pseudcode 5.5
Randomize-in-place(A)
   n=A.length;
   for i=1 to n
       swap A[i] with A[Random(i,n)]
```
---
###生日悖论
---

```
#混乱池
我个人的处理方式就是提出问题-->解决问题
以前我的抽象能力不够强,总是迷失,比如,一个房间里人数达到多少才会有两个人相同生日,我只会看到每个人具有特定的一天生日的概率为1/n,但是当研究对象上升为整体统计时我就开始出现了混乱,两个人的生日在特定天的概率...所有人的生日在特定天的概率,其实这里出现了递归哦,虽然伪道教追随者的我不敢随意建立模型,但是这里还是使用了树,将人整体看为研究对象,他有m中可能性(假设有m人),将个体人为研究对象他有n中可能性(假设有n天),其实生活中研究的对象的类型大都如此,很多人妄图建立生活与理论的联系苦不堪言,前人已经总结了大量的规律,人与人之间相互独立,每个人生日的取值相互独立
```
