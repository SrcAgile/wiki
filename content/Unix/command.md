---
title: "命令工具"
layout: page
date: 2017-03-22 11:10:20
collection: "[工具目]"
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---

[TOC]

### Base
- manual
```bash
# 最基本是要会使用manual,分7类
1. 用户在shell环境中可以操作的命令或可执行文件
2. 系统内核可调用的函数与工具等[The system call interface has always been documented in Section 2 of the UNIXProgrammer ’s Manual.]
3. 一些常用的函数（function）与函数库（library），大部分为C的函数库（libc）
4. 设备文件的说明，通常是在/dev下的文件
5. 配置文件或者是某些文件的格式
6. 游戏（games）
7. 惯例与协议等，例如Linux文件系统、网络协议、ASCII code等说明
8. 系统管理员可用的管理命令
9. 跟kernel有关的文件
上面这些内容可以通过输入“man 7 man”命令来获取更详细的说明。

[EXAMPLE]
➜  unix git:(master) ✗ man 1 [Tab]
zsh: do you wish to see all 2333 possibilities (584 lines)? 
➜  unix git:(master) ✗ man 1 ls
➜  unix git:(master) ✗ man 7 ascii

```
###常见问题
- [查看文件被什么进程占用](#lsof)




###罗列
####lsof
<p id="lsof"></p>
```c
查看某个文件被哪些进程在读写

lsof是什么意思

list open files

查看某个文件被哪些进程在读写

lsof 文件名

查看某个进程打开了哪些文件
lsof –c 进程名
lsof –p 进程号

lsof用法小全
lsof abc.txt 显示开启文件abc.txt的进程
lsof -i :22 知道22端口现在运行什么程序
lsof -c nsd 显示nsd进程现在打开的文件
lsof -g gid 显示归属gid的进程情况
lsof +d /usr/local/ 显示目录下被进程开启的文件
lsof +D /usr/local/ 同上，但是会搜索目录下的目录，时间较长
lsof -d 4 显示使用fd为4的进程
lsof -i [i] 用以显示符合条件的进程情况
```

### gsettings
`这个命令是因为每次接触不良的时候,电脑连接的手机都会激发电脑出现弹窗,网上有使用gconf-editor的但是貌似已经不管用了,所以找到了gsettings`
```bash
禁止自动挂载：
$ gsettings set org.gnome.desktop.media-handling automount false
禁止自动挂载并打开
$ gsettings set org.gnome.desktop.media-handling automount-open false
允许自动挂载
$ gsettings set org.gnome.desktop.media-handlingautomount true
允许自动挂载并打开
$ gsettings set org.gnome.desktop.media-handling automount-open true
```
