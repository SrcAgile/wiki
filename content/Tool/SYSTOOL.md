---
title: "linux系统配置"
layout: page
collection: "[系统目]"
date: 2017-08-21 19:00:00
---

[TOC]

###系统色彩
　1. 调节屏幕对比度参数gamma值[default 1.0]
```c　　
    > xgamma -gamma .75
```
2. 调节本本屏幕背光亮度pci
首先进入终端输入lspci命令，列出各种设备的地址：
```bash
00:00.0 host bridge: Intel Corporation Mobile 945GM/PM/GMS, 943/940GML and 945GT Express Memory Controller Hub (rev 03)
00:02.0 VGA compatible controller: Intel Corporation Mobile 945GM/GMS, 943/940GML Express Integrated Graphics Controller (rev 03)
00:02.1 Display controller: Intel Corporation Mobile 945GM/GMS/GME, 943/940GML Express Integrated Graphics Controller (rev 03)
00:1b.0 Audio device: Intel Corporation N10/ICH 7 Family High Definition Audio Controller (rev 02)
00:1c.0 PCI bridge: Intel Corporation N10/ICH 7 Family PCI Express Port 1 (rev 02)
00:1c.1 PCI bridge: Intel Corporation N10/ICH 7 Family PCI Express Port 2 (rev 02)
```
发现00:02.0是VGA设备，于是我们修改它的属性：
```c
sudo setpci -s 00:02.0 F4.B=FF
```c
setpci 是修改设备属性的命令.
-s 表示接下来输入的是设备的地址。
00:02.0 VGA设备地址（<总线>:<接口>.<功能>）。
F4 要修改的属性的地址，这里应该表示“亮度”。
.B 修改的长度（B应该是字节（Byte），还有w（应该是Word，两个字节）、L（应该是Long，4个字节））。
=FF 要修改的值（可以改）
```

###gif
1. tools
    `ffmpeg`
    `imagemagick`
2. use
```c
ffmpeg
-f 处理生成文件类型
-i 处理的源文件
-r fps[抽帧每秒]
-framerate [合成后文件的帧,不影响文件大小]

 ffmpeg -f image2 -framerate 2 -i out-%03d.jpeg x.gif
 ffmpeg  -i 1.mp4 -r 0.3  -f image2 out-%03d.jpeg 
```

###ref
[1]. [setpci](http://man.linuxde.net/setpci)
[2]. [ffmpeg1](https://linux.cn/article-2542-1.html)
[3]. [ffmpeg2](http://blog.csdn.net/happydeer/article/details/45727227)