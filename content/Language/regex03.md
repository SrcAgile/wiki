---
title: "正则[R|P]编程"
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

####9/04更新
---

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


####9/18更新
---
**注意精通regex  p49总结**

```perl
s/regex/replacement/i 忽略大小写的替换
s/regex/replacement/g 所有行都进行替换
m/regex/i或者g m可以不用加, 意会即可

```
任务来了,老板给了一个超级大的文件,是的html文件(然而所有的代码都挤在了一块),他说让我让这个文档层次感显示,小意思,我去做,但是<b>mmp</b>914行,本来我说使用<b>notepad++</b>毕竟能匹配括号,但是这并没有卵用啊,914行,所以说我只能借助正则婊了,但是完全不会用啊摔!那么只能先使用perl把标签分出来,让其单行显示

<b style='color:red'>v0.01</b>

```perl
meta@meta$  perl -p -i -e 's/(<(!| )*([a-z!-])+ *[^><]*>)/\n\1\n/g' /home/meta/Desktop/a.html

meta@meta$  perl -p -i -e 's/(<\/ *[^><]+ *>)/\n\1\n/g' /home/meta/Desktop/a.html

```
真是呵呵,现在的自己好辣鸡,而且还是分两次做完的单行

<b>部分解释</b>
1. 其实在perl中`$1`, `$2`, `$3`, 可以代替 `\1`, `\2`, `\3`作为捕获文本的
2. 我们都知道正则`()` 可以用来捕获文本,但是如果不需要捕获,它偏偏捕获了,这样只会肆意拓展$型变量的长度,所以使用了非捕获型规则,即`(?:...)`, 但凡出现在这个括号里面,都不会被捕获的.

3. 那么我现在有一个<b style='color:red'>疑问</b>还没有<del>思考出来</del>实践验证出来, `(|)` 会 不会被捕获

<b>Perl参数含义</b>

|参数|含义|
|:--:|:--:|
|-p  |对目标文件的每一行进行查找|
|-w|like gdb, 和warning一个意思,打开perl额外警告功能|
|-e|表示整个程序接在命令后面[<b style='color:green'>不明觉li</b>]|
