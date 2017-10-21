---
title: "序列"
layout: page
collection: "[Python]"
date: 2015-10-07 20:20:37
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---
> ...

---
- **今日音乐**
```
暂无评论
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=386830&auto=0&height=66"></iframe>

---
[TOC]

##Intro


---
###包含
---
1. 列表
2. 字符串
    `分好几种,因为存在编码问题`
3. 元组
---
###特性
---

1. 有序的
2. 可以通过下标偏移量访问到它的一个或者几个成员

---
###操作
---

####操作符
- 标准类型操作符[core_python4.5]()
- 序列类型操作符

---
###输出
---

- 格式化输出
- 字符串模板

鉴于使用%d%s%u等等标识占位符类型实在是太...,比如又一次我就判断类型出错，不如使用字符串模板,Template也确保了string包的地位.

```python
#from string import Template
>>> from string import Template
>>> s = Template('hello ${name}, i am a ${sex}')
>>> me ='lyc'
>>> mysex='male'
>>> s.substitute(name=me,sex=mysex)
'hello lyc, i am a male'
//caution:
>>> s.safe_substitute(sex=mysex)
'hello ${name}, i am a male'

```

---
###内建函数
---

>关于字符串操作的函数

1. 标准类型
    `[1]. cmp`
2. 序列类型
    `[1]. enumrate`
    `[2]. len`
    `[3]. max/min`
    `[4]. zip`

```python
  #zip use
  >>> a,b='helloa','iamb'
  >>> a
  'helloa'
  >>> b
  'iamb'
  >>> zip(a,b)
  [('h', 'i'), ('e', 'a'), ('l', 'm'), ('l', 'b')]
  >>>
```

3. 字符串函数
- raw_input()/input
- str/unicode
- chr[ASCII 0-255]
    `print (chr(65))-->A`
- unichr[unicode]
    ``
- ord

4. 字符串内建函数
- string.expandtabs(num)
- string.capitalize()
- 对string内容检索类函数
    `string.isdigit()`
    `string.islower()`
    `string.strip`
    `string.leftstrip`
    `etc`

####字符串转义
1. 特殊字符串与控制字符
2. \x,\num

####Unicode字符串

> Python 已经可以应付大部分应用对 Unicode 的存储、访问、操作的需要了
