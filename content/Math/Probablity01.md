---
title: "随机变量"
layout: page
collection: "[概率论]"
date: 2016-02-25 11:02:20
---

**文档状态：**<a style="color:red;background-color:gray"><b>待修正....</b></a>

- **今日音乐**

---

[TOC]



###一个问题
---
<center style="position:relative ">
![](https://raw.githubusercontent.com/srcmit/source/wiki/photo/probab.png)
</center>

---
###概念
---

1. 随机事件
> 在概率论中，随机事件（或简称事件）指的是一个被赋予机率的事物集合，也就是样本空间中的一个子集。

2. 事件
<del>哲学范畴不予讨论</del>

3. 随机变量

<b>

给定样本空间 $({\displaystyle (S,\mathbb {F} )} )$ ，如果其上的实值函数 $({\displaystyle X:S\to \mathbb {R} } )$ 是 $({\displaystyle \mathbb {F} } )$ (实值)可测函数，则称 $({\displaystyle X})$ 为（实值）随机变量。初等概率论中通常不涉及到可测性的概念，而直接把任何 $({\displaystyle X:S\to \mathbb {R} } )$ 的函数称为随机变量。
如果 $({\displaystyle X})$ 指定给概率空间 $({\displaystyle S})$ 中每一个事件 $({\displaystyle e})$ 有一个实数 $({\displaystyle X(e)})$ ，同时针对每一个实数 $({\displaystyle r})$ 都有一个事件集合 $({\displaystyle A_{r}} )$ 与其相对应，其中 $( A_{r}= {  e:X(e) ≤ r })$ ，那么 $({\displaystyle X} )$ 被称作随机变量。随机变量一般用大写拉丁字母或小写希腊字母（比如 $({\displaystyle X,Y,Z,\xi ,\eta })$ )来表示，从上面的定义注意到，随机变量实质上是函数，不能把它的定义与变量的定义相混淆，另外概率函数 $({\displaystyle P})$ 并没有在考虑之中。

</b>

---
###关系讨论
---
<div style='font-weight:bold;text-decoration: line-through; color:green' >随机事件的解释很明显，随机事件是可以用样本点来描述的，即事件 $(A)$ = {具有特征的某些点}，此处出现一层抽象，现实世界的通过在时间上的独立实验得到的结果通过降维到一个样本空间中，样本空间中的样本是具有多种属性的，而我们通常需要得到样本的某一属性在统计上的结果，于是又出现一层降维，那就是通过事件中样本的某一属性作为样本的标识符，然后向一维几何空间投影到实数轴，这个投影函数被称作<span style='color:red'> 随机变量</span>，投影到实数轴上的目的很单纯 ，当然是<span style='color:red'> 方便数理运算</span>，有人谈到样本的属性值是不是必须要是数值类的才能投影呢？这个实在是小看人类的抽象能力了，比如 骰子6面6种颜色，假设事件 $(B)$ 为当前投掷的骰子的落地面的颜色，难道这就不能进行一维空间的映射了吗？不！我们可以假设某一种颜色投影为实数域上的数值1，其他颜色为0，此时的我们就很容易计算 $(B)$ 的数学期望了，算法学习中有一种东西叫做<span style='color:red'> 指示器变量</span>，是一个很好的应用！</div>

> **此处应该存在一个很大的灵感,目前没有想出来**

---
> 从排列组合的淤泥中飞升到数值运算中？
