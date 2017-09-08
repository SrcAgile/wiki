---
title: "regex(二)"
layout: page
collection: "[四代目]"
date: 2017-08-31 21:00:00
---

**文档状态：**<a style="color:red;background-color:gray">思索中....</a>

---
- **今日音乐**
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=286999&auto=0&height=66"></iframe>

---
> 规律之外,无语无言

---

[TOC]

###声明
以下实例均出自egrep所属流派(flavor),如果出现复制之后无法匹配,证明你...
###术语
- 正则[Regex]
- 匹配[matching]
- 元字符[metacharacter]
- 流派[flavor]
- 子表达式[subexpression]
- 字符[character]
###常见
---
- 量词
    - ? + *

###解决方案
---

- 重复出现 + *
- 多选择  
    - 单位置 (|)
        - 不确定 .
    - 连续多位置 [...]  [^...] 集合与补集
- 可选择 ？
- 高级手段
    - 反向匹配[看到了动态语言的影子]
    `实现重复单词查询`
    `Ex: \<([A-Za-z]+) +\1\>`

###解析
---
- 使用`()`的原因
    - 限制多选结构
    - 分组
    - 捕获文本
- 转义的三种情况
    - 元字符普通化
    - 普通字符元字符序列化
    - 除此之外被忽略掉

###常用手段
---
- 缩小问题空间
  `color 与colour egrep '(color|colour)'----'colo(u)?r'`

- 正则表达式内部没有忽略大小写,想要忽略大小写从封装他的软件入手
   - egrep -i#忽略大小写


###常见问题
---
- 单词分界
    `smallcat 与cat 但是我仅仅想要匹配单独的cat单词`
    `解决:元字符序列 \<  \>类似单词版本的^$`
    `使用:(\>cat\>) `
    `特性:遇见非英文字符直接认为是词首或者词尾`

- 贪心匹配
- 提取引号内字符串
    `"[^"]*"`
- 美元金额
    `\$[0-9]+(\.[0-9]+)?`
- 匹配HTTP URL
    `\<(H|h)(T|t){2}(P|p)://[^ ]+\>`
- html tag
    `< *([a-z]+) *>[^<>]*<\1>`
- 匹配时间
    `下次有时间补上`

###实践心得
---
- `* + `的对象大都是空格
