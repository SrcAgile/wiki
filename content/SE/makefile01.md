---
title: "工程[M]开发"
layout:  page
collection: "[工程目]"
date: 2017-09-11 21:00:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑....</a>

---
- **今日音乐**
NONE

---
>

---
[TOC]

###order
---
编译-->链接-->运行
- 编译
    `生成.o`
    `编译器只检测程序语法和函数、变量是否被声明如果函数未被声明，编译器会给出一个警告，但可以生成Object File。`
- 链接
    `可能因为有太多.o文件难以书写,引入Archive File[.a文件][window 是.lib文件]`
    `链接器会在所有的Object File中找寻函数的实
现，如果找不到，那到就会报链接错误码（Linker Error），在VC下，这种错误一般是： Link2001错误 ，意思说是说，链接器未能找到函数的实现。你需要指定函数的Object File。`

---
###command
- 特殊命令解释
- -C 大写，切换到指定目录再执行 make 过程，makefile 在这个指定目录里面
- -c 小写，表示只编译，不链接

*_*好多东西要写啊,那我把pdf上传吧!
