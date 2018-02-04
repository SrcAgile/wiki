---
title: "linux WIFI指南"
layout: page
collection: "[net纲]"
date: 2015-01-01 01:00:00
---

**文档状态：**<a style="color:red;background-color:gray">新年快乐....</a>
---

[TOC]

###更新日志
- 2016.10.11 添加简便方式开启wifi热点

###网站链接
- [Ubuntu开启wifi热点](https://www.cnblogs.com/king-ding/archive/2016/10/09/ubuntuWIFI.html)
- [无线信号解析](http://blog.csdn.net/lbyyy/article/details/51392629)
- [关于wifi工作模式](https://www.bbsmax.com/A/x9J2O9ZZ56/)

###方式
1. 点击右上角的网络，然后选择下拉菜单中的edit connections
2. 点击增加，连接类型选接Wi-Fi，再点新建
3. WIFI的设置
    `第一个连接名称是指保存在本地的文件名称，SSID是别的电脑搜索到的你wifi的名称,模式设置成hotspot`
4. 设置Wi-Fi安全性wpa&wpa2(person)
5. 此时生成wifi配置文件在`/etc/NetworkManager/system-connections/`下
6. 查看一下配置文件
```c
  [wifi]
  mac-address-blacklist= #设置物理地址黑名单
  mac-address-randomization=1
  mode=ap     #一定设置为ap
  seen-bssids=
  ssid=your ssid
```

###词汇
- ap
    `此处的ap是wirelessAccessPoint，无线网络接入点，类似有线网络中的集线器hub`
