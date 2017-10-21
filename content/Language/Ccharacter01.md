---
title: "语言[C]特性"
layout: page
collection: "[三代目]"
date: 2017-09-11 23:00:00
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---
>

---
- **今日音乐**
```
暂无评论
```

---
[TOC]

###位域
1. 一个位域必须存储在同一个字节中，不能跨两个字节。如一个字节所剩空间不够存放另一位域时，应从下一单元起存放该位域。也可以有意使某位域从下一单元开始。

```c
  EX:
  struct bs
  {
      unsigned a:4;
      unsigned :0;
      unsigned b:4;
      unsigned c:4;
  }
```
2. 由于位域不允许跨两个字节，因此位域的长度不能大于一个字节的长度，也就是说不能超过8位二进位
3. 位域可以无位域名，这时它只用来作填充或调整位置。无名的位域是不能使用的。例：


```c
  EX:
  struct k
  {
      int a:1;
      int :2;
      int b:3;
      int c:2;
  };
```
