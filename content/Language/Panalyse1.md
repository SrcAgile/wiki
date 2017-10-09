---
title: "源码[P]分析"
layout: page
collection: "[分析目]"
date: 2017-08-27 21:00:00
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

- **今日音乐**
```
[网评]
谢谢网易云音乐让我知道真相.
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=624751&auto=0&height=66"></iframe>

---
[TOC]
##写在开头
```
1.本系列文章的理解水平建立在编译原理,c语言,python语言的了解基础上
2.如果不知道Scaner,Parser,AST[抽象语法树],compiler，Code Evaluator建议进修
3.
```
##架构图
<center >
<div style="width:644px">
![](https://raw.githubusercontent.com/srcmit/source/wiki/photo/pyarchive)
</div>
</center>

```
1.Scaner--[token]--->Parser--[AST]-->Compiler--[byte code]-->Code Evaluator
2.Parser         use      Object/Type Structure
3.Compiler       use      memory Allocator
4.Code Evaluator modify   current state of python
```
---
##结构剖析
```
1. [./include] C/C++编写python模块的头文件源
2. [./Lib] python自带标准库#速度慢
3. [./Module] 由c编写#速度快
4. [./Parser] Scaner+parser+some tools
5. [./Objects] Python所有内建对象+运行时的对象实现
6. [./python] **运行核心** Compiler+Evaluator
7. [./PcBuild] windows工程文件
```
###编译安装
- windows 没错这就是我格式化windows的原因
- Linux/Unix
    1.`>  ./configure --prefix='PATH'`
    2.`>  make`
    3.`>  make install`
###调用c函数
- [陈皓](https://coolshell.cn/articles/671.html)
- 暂停睡觉
