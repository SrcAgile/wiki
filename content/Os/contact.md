---
title: "异种机通讯"
layout:  page
collection: "[策略目]"
date: 2017-11-13 07:00:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑....</a>

---
> ...

---


[TOC]

##Intro
介绍一下常见的异种机通讯方式,同时简单的描述一下有关加密的知识

###Knowledge
####两种场景
- 公钥加密私钥解密称为<b style='color:red'>解密</b>
- 私钥加密公钥解密称为<b style='color:red'>签名</b>

### SSH
####单向ssh
- 安装ssh-client&ssh-server
- 本地生成公钥私钥
    `0. 保证机器能够互相ping通`
    `1. ssh-keygen -t [rsa|dsa] #个人选用rsa`
    `2. copy  id_dsa.pub 到目标机器中`
    `3. 目标机器执行 cat  id_dsa.pub >> ~/.ssh/authorized_keys`

####双向ssh
- 安装ssh-client&ssh-server
    `0. 保证机器能够互相ping通`
    `1. 两个节点都执行操作ssh-keygen -t [rsa|dsa] #个人选用rsa`
    `2. copy  id_dsa.pub 到对方机器中`
    `3. 对方机器执行 cat  id_dsa.pub >> ~/.ssh/authorized_keys`
    `4. chmod 600 authorized_keys`
    `5. chmod 700 -R .ssh`

###乱入序列化

|序列化组件|数据库组件|说明|
|:-----:|:-----:|:-----:|
|IDL|DDL|用于建表或者模型的语言|
|DL file|DB Schema|表创建文件或模型文件|
|Stub/Skeleton|lib O/R mapping|将class和Table或者数据模型进行映射|
