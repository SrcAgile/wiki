---
title: "常见[L]问题"
layout:  page
collection: "[问题目]"
date: 2017-10-16 21:00:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑....</a>

---
- **今日音乐**
NONE

---
> 不仅仅是解决问题,而是要能够发现问题,了解问题的本质,这样问题才会变少.

---



[TOC]

###Problem

-  cc&gcc
```python
    linux下makefile中常使用cc,一开始我很奇怪,怎么命令行参数和gcc一样呢?
    那么我来查询一下
    $ which cc
      /usr/bin/cc
    $ ls -al /usr/bin/cc
      /usr/bin/cc -> /etc/alternatives/cc
    $ ls -al /etc/alternatives/cc
      /etc/alternatives/cc -> /usr/bin/gcc
    看起来他们是一个东西,但是为什么要这样弄一个别名呢?当然是为了兼容!unix下用的是cc,但是cc太贵,linux用不起,自创gcc,但是很多linux下的工程编译文件makefile都是写的cc,呵呵~mmp,重命名就可以了~
```
