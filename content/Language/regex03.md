---
title: "regex(三)"
layout: page
collection: "[四代目]"
date: 2017-09-08 21:00:00
---

**文档状态：**<a style="color:red;background-color:gray">思索中....</a>

---
- **今日音乐**
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=286999&auto=0&height=66"></iframe>

---
> 规律之外,无语无言

---

[TOC]

###perl
简单掌握

- 变量的特殊性 `$variable`，面临引号输出也不会解体,除非转义

- 运算符的特殊性
    - =～ `匹配`
    - ==  `相等`
    - =   `赋值`
- 编码运行
```perl
print "welcome to use perl test \n";
print "please input the value:\n";
$well=<STDIN>;
chomp($well);
if($well =~ /^[0-9]+$/ )
{
    print "你是数字\n";
}
else{
    print "不是数字\n";
}
```
```bash
>>> perl -w test
>>> welcome to use perl test
    please input the value:
    101
    你是数字
>>>
```
