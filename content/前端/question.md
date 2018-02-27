---
title: "前端问题"
layout: page
date: 2016-02-18 09:00:00
collection: "[io]"
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---

[TOC]

###messy
AVC（H.264）编码的视频在FireFox和Chrome上都可以播放。而使用MPEG4编码的视频在FireFox不能播放，而在Chrome上则只有声音没有图像

###应用
- 插入视频
```html
<video width="160" height="48" controls>
    <source src="./1.mp4" type="video/mp4">
    您的浏览器不支持video标签。
</video>
#火狐浏览器出错,谷歌正常
```

- 控制chrome\[[help]\](../Tool/conftool.html)
```bash
1. install selenium
2. install chrome driver
```

###js
- 查看UA
    `CTRL+SHIFT+J--->javascript:alert(navigator.userAgent)--->COPY IT`