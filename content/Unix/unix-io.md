---
title: "unix io"
layout: page
date: 2018-01-28 12:00:00
collection: "[io]"
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---

[TOC]

###Question
1. if u want to avoid overwriting an existing file,you can use `(O_CREATE|0_EXCL)`
首先应该知道，当文件存在时，使用open("filename",O_CREATE,0666)是会重写一个文件的，所以引入了(O_CREATE|0_EXCL)，但是有的人认为多此一举，可以使用
```c
if(access(fileName,R_OK)==-1)#如果文件不存在
    open(fileName,O_CREATE|0_RDWR,0666);
```
但是在操作系统课程老师讲过，系统调用是具有原子性的，即执行开始到结束不会切换进程，所以
`open("filename",O_CREATE|O_RDWR|O_EXCL,0666)`多了一个好处，当多道环境下只允许一个进程去打开文件，例如socket，此时便用原语实现了。
