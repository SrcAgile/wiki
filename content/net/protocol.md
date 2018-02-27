---
title: 协议探索
layout: page
collection: "[net纲]"
date: 2017-02-03 16:10:09
---

**文档状态：**<a style="color:red;background-color:gray">系统又崩溃了，攒一波.</a>
---

[TOC]

###工具
> 工欲善其事必先利其器
####wireshark
`$ sudo apt-get install wireshark`
但是这样虽然安装上了，但是进程运行过程中需要root的权限，所以需要配置一下，鉴于以前经验，即使以超级用户运行又会出现，以本地用户安装的过程中产生的一些配置信息无法正常加载，作为强迫症，需要解决这个问题，值得一提的是，在linux内核2.x之后才有好的方案，即`能力`，摆脱用户要么有root权限，要么有普通权限的二态性，并且在wireshark加载中会出现使用`/usr/bin/dumpcap`，所以主要是配置这个。
脚本如下
```bash
groupadd -g wireshark
usermod -a -G wireshark <用户名>
chgrp wireshark /usr/bin/dumpcap
chmod 4750 /usr/bin/dumpcap       //chmod 750 /usr/bin/dumpcap也可以，多加了setuid
setcap cap_net_raw,cap_net_admin=eip /usr/bin/dumpcap  //
if setcap不能用{
    bash -c "sudo apt-get install libcap2-bin"
}
```