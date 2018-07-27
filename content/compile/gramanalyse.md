---
title: "语法分析"
layout: page
collection: "[阅读纲]"
date: 2018-06-28 20:00:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---
[TOC]

###语法分析FAQ
- 语法分析器用什么工具描述
    `2型文法上下文无关文法`
- 语法分析的类型
    `[1]. 自上而下的分析`
    `[2]. 自下而上的分析`
- 自下而上的分析的常见方法
    `[1]. 算符优先分析文法 适合表达式`
    `[2]. 忘了இдஇ`
- 自上而下的分析的常见方法
    `[1]. 递归下降分析`
    `[2]. 预测分析程序`

---
### 递归下降分析
In a word,通过最左推导进行 <b style="color:red"> 归约</b>
出现两个问题, <b style="color:red"> 过度回溯</b>和 <b style="color:red"> 左递归问题</b>,出现左递归本来不可怕,但是递归下降算法本身的操作方式导致了其可怕,因为左递归匹配时会出现死循环...,不久就会把栈塞满,回溯问题本身是不太影响的,但是递归下降分析操作的数据结构是语法 <b style="color:red">树 </b>,大量的回溯费时费力...

<b style="color:red">LL(1)分析法 </b>的解决方案如下所示:
解决左递归问题:变成右递归文法,比如　<b style="color:green">    $({  P \to P\alpha|\beta   })$ </b> 这个语义就是生成以 $({  \beta  })$开头,0到多个 $({  \alpha  })$ 终止的字符串,改成右递归为 <b style="color:green">  $({ P \to \beta P^＇})$  $({P＇ \to \alpha P^＇|\varepsilon   })$</b>
但是没有那么简单,左递归存在直接左递归和间接左递归,直接左递归就如　<b style="color:green">    $({  P \to P\alpha|\beta   })$ </b> 一样,但是间接左递归存在 <b style="color:red">环</b>,而且这种环在大量推导式的情况下不容易肉眼观察出,即使能够肉眼观察出,交给计算机也看不出,所以使用一种方法,这种方法不管存不存在左递归环,都会将非终结字符连接的链化成直链

[![Pi2u40.md.png](https://s1.ax1x.com/2018/06/28/Pi2u40.md.png)](https://imgchr.com/i/Pi2u40)