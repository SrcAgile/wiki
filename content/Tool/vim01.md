---
title: "VIM大法"
layout: page
date: 2017-09-20 02:00:00
collection: "[编辑器目]"
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---
> EMACSer们,亮剑吧!

---
- **今日音乐**
```
暂无评论
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=386830&auto=0&height=66"></iframe>

---


<center style="position:relative; ">
![](https://raw.githubusercontent.com/srcmit/source/wiki/photo/vim.gif)
</center>

[TOC]

###Intro
---
宇宙第一不介绍

###问题集
---
###起源
事情是这样的,那是大一的春天,有一天我打开vim进行保存的时候,呵呵.
<b style='color:white; background-color:red'>"m" E212: Can't open file for writing</b>

有<del>毛病</del>错误?爸爸来整你!

1. 开始`:help E212`
2. 得到答案

<p style='background-color:black; color:green'>                                                       <b>*E190* *E212*
  Cannot open "{filename}" for writing
  Can't open file for writing For some reason the file you are writing to cannot be created or overwritten.
The reason could be that you do not have permission to write in the directory
or the file name is not valid.
</b></p>

3. 搜了得出使用 `w !sudo tee %`,从此踏上不归路

####root保存权限问题
- 1
