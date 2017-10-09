---
title: "Syscof/1"
layout:  page
collection: "[L-File]"
date: 2017-09-08 21:00:00
---

> 致力学习到策略,而非限制

---

[TOC]
##linux配置
> 配置是具有范围的,时效的.

###环境变量
---
####分类
- 生命周期
    `永久的,修改配置文件生效`
    `暂时的,使用export在shell生效`
- 作用域
    `系统环境变量,对所有用户生效`
    `用户环境变量,对指定用户生效`
    `当前shell`

####配置
1. 作用域:所有用户+生命周期:永久的
```bash
$ vim /etc/profile
$ export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
$ source /etc/profile

```
2. 作用域:当前用户+生命周期:永久的
```bash
$ vim ~/.bash.profile
$ export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
$ source ~/.bash.profile

```
3. 作用域:当前shell+生命周期:临时的

```bash
$ export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib
```
####查看
```bash
$ echo $PATH #注意大小写

```
####常见
- 查看所有shell变量
    `$ export`
- 查看所有环境变量
    `$ env`
- PATH 指定命令的搜索路径
    `$ export PATH=$PATH:/NEW/PATH`

####使用语言
**C语言**
- getenv()返回一个环境变量
- setenv()设置一个环境变量
- unsetenv()清除一个环境变量
