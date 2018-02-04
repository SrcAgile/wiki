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
    `[函数][1]. enumrate()`
        `for i in enumerate([1,2,3,4])`
        `python3.x好像添加了惰性求值`
    `[函数][2]. len`
    `[函数][3]. max/min`
    `[函数][4]. zip`
    `[函数][5]. sorted()/reserved()`
        `适用于可变对象序列类型`
    `[函数][6]. sum`
        `对int类值有效`
    `[函数][7]. list/tuple`
    `[算符][8]. in/not/in[成员关系]`
    `[算符][9]. 切片操作`
    `[算符][A]. 连接操作符+`
    `[算符][B]. 重复操作符*`

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


---
#unicode[Messy]
---
1. 首先要理解python目前在内存中的操作大都是Unicode类型的,由于没有具体查,所以使用大都词汇
2. 其次着重说一下python中对字符串的处理
    `操作系统编码`
    `输入法`
    `可视化应用程序[terminal]`
    `内存`

3. python的repr()有所改变,自动的对''

4. python 2.x默认以ascii对字符串进行解码
5. python 3.x默认使用utf8对字符串进行解码
6. 注意字符的处理与显示
7. py把硬编码的字符串称为字面上的字符串,这些字符串默认使用ASCII编码,而加上u前缀则表示以unicode编码方式,编码编码!往哪编,在内存中存放的编码,但是此时要注意一下,如果你是通过终端输入,字符流以系统编码为基准流入文件流[终端],然后对于ascii来看虽然你输入了一个中文字符他却认为你输入了三个ascii字符,然后这些东西都送给print让其操作输出流,他只是将数据送到opengl而已,图形渲染接口通过自己定义的编码格式[例如terminal可以通过修改profile/compatibility/encoding]进行渲染,此时看起来就是外部键盘---中断通知-->操作系统[按照自己的编码格式]--放入-->输入缓冲区----fetched---->python[按照ascii编码去存放取来的字符]----加工---->送到图像引擎渲染[按照自己的编码]---->查表显示


###unicode

1. 在py2.x编程的时候对字符串注意加u前缀
2. 为了考虑兼容性不得不使用string模块,但是这并不是在新时代编码使用其提供的函数接口的原因
3. 正则引擎对Unicode支持


###列表类型操作符

- 列表解析
- ...

###注意
- 可以改变对象值的<b>可变对象</b>的方法是没有返回值的
- 但是对与不可变对象就是完全相反了

###列表的特殊性

- 构建数据结构
    `栈`
    `队列`

###浅拷贝与深拷贝
我准备从寄存器,堆栈和堆角度来谈一下关于深浅拷贝的问题
很多人诧异于区别与c语言python的类型自由,关于类型自动转换我目前没有了解太多,所以仅仅谈一谈在自己角度上的抽象性理解

1. 首先说`a=1`,这里面涉及三个"物理"器件,其中a是地址存放在寄存器,他并不是单纯的指向内存中1的地址,他的目标指向堆栈,而在当前的堆栈中存放的并不是数字`1的`编码数据,而是`1`在堆中的地址,也可以说是`1`的引用,以后会上一个图.

2. 我们看一下存放在堆栈中`1`的引用(属于地址),这个存放在堆栈中的引用应该不仅仅是地址而且包括了一些类型信息,这些类型信息决定了你对其引用的内存对象的操作能力,所以对于`1`来说他是只读型数据,一旦你操作改变它的值他就会报错,不信你可以试试.所以对于`b="string"`这个`b`来说,他肯定不能进行pop,append,push等操作.但是`c=[1,2,3,4]`可以啊,因为`c`的类型信息是允许你这样做的.如上所见,我们看到两种东西,一种是指向内存对象的堆栈引用,另一种就是内存中的对象,注意只想对象的引用不一定存放在堆栈中,比如`d=[[1,2,3,4],5,6,7]`,`d[1]`也是一个对象引用,但是他存放在了内存里面.

3. 所以浅拷贝就是单纯的拷贝堆栈中的数据

4. 所以深拷贝就是拷贝所有的引用(地址),无论你是放在了堆栈还是内存中!

5. 说明一下:以上的内存是除堆栈以外的存储器区域.



###注意
- 谈一谈元组的更新
    `tuple=tuple[0],tuple[1],tuple[-1]`
