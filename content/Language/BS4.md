---
title: "数据采集"
layout: page
collection: "[采集目]"
date: 2017-08-27 23:00:00
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

- **今日音乐**

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=28872586&auto=0&height=66"></iframe>


---
[TOC]

##<b style='color:#580000'>Intro</b>
```
1.[lib]I choose bs4
2.[version]python3.x
```
##<b style='color:#580000'>Object[BeautifulSoup]</b>
###<b style='color:#700000'>构造参数</b>
####<b style='color:#A00000'>Encode</b>
`输入编码其实可以自动检测,但是不一定准哦,输出指定UTF8`
- from_encoding[以文档指定的编码解析]`E:from_encoding="iso-8859-8"`
- exclude_encodings[排除编码]`E: exclude_encodings=["ISO-8859-7"]`
####<b style='color:#A00000'>Parser</b>
- xml
- lxml[推荐]
- html5lib
- html.parser[内置]
- <a>差异比较</a>
####<b style='color:#A00000'>parse_only</b>
- SoupStrainer对象
    - SoupStrainer("TAG_NAME")
    - SoupStrainer(id='##')[选择器]
    - SoupStrainer(method引用)
###<b style='color:#700000'>Attribute</b>
- soup.tag-------------[<a>获得标签</a>] [<a>属</a>]
- soup.tag['Attr']---[<a>获得键值</a>] [<a>属</a>]
- soup.contents-----[<a>获得.descendants子节点`<html>#</html>`</a>] [<a>属</a>]
- soup.children------[<a>获得子节点</a>] [<a>属</a>]
- soup.descendants-[<a>获得子孙节点</a>] [<a>属</a>]
- soup.strings-----[<a>文档去标签化</a>] [<a>属</a>]
- soup.stripped_strings---[<a>忽略空行去标签化</a>] [<a>属</a>]
- soup.parent------[<a>父节点NONE</a>] [<a>属</a>]
- soup.original_encoding---[<a>模糊识别编码</a>] [<a>属</a>]

###<b style='color:#700000'>Method</b>

####<b style='color:#A00000'>Normal</b>
- soup.prettify([编码默认utf8])[<a>格式化输出</a>] [<a>IO</a>]
- soup.get_text()---[<a>获得文本</a>]
    - soup.get_text("character")[tag之间character分割]
    - soup.get_text("|", strip=True)[<a>以‘|’分割标签,移除空白</a>]
####<b style='color:#A00000'>Search</b>
- soup.new_tag("a", href="url")[<a>增加新标签</a>] [<a>增</a>]
- soup.new_string("string", Comment)[<a href='Comment' style='color:green'><b>Comment</b></a>] [<a>添加注释</a>] [<a>增</a>]
- soup.find_all('tag')---[查找.标签] [得到.列表] [<a>查</a>]
- soup.find(id='#')------[查找.定位] [得到.唯一] [<a>查</a>]
- soup.select("tag")[<a>CSS选择器</a>] [<a>直接查找</a>] [<a>查</a>]
    - soup.select("tag1 tag2")[<a>CSS选择器</a>] [<a>逐层查找</a>] [<a>tag1 的子孙是tag2皆可</a>] [<a>查</a>]
    - soup.select("tag1 > tag2")[<a>CSS选择器</a>] [<a>tag1的儿子tag2</a>] [<a>查</a>]
    - soup.select("#tag1 ~ .class")[<a>CSS选择器</a>] [<a>tag1的所有class兄弟</a>] [<a>查</a>]
    - soup.select("#tag1 + .class")[<a>CSS选择器</a>] [<a>tag1的紧邻的class兄弟</a>] [<a>查</a>]
    - soup.select(".class") [<a>CSS选择器</a>] [<a>所有的class节点</a>] [<a>查</a>]
    - soup.select("[class~=some_name]")[<a>CSS选择器</a>] [<a><b style='color:red'>忘了</b></a>] [<a>查</a>]
    - soup.select("#id") [<a>CSS选择器</a>] [<a>查</a>]
    - soup.select("tag#id") [<a>CSS选择器</a>] [<a>查</a>]
    - soup.select("#id1，#id2")[<a>CSS选择器</a>] [<a>查</a>]
    - soup.select('a[href]')[<a>具有属性</a>] [<a>查</a>]
        - soup.select('a[href="url"]')[<a>确定指定属性值</a>] [<a>查</a>]
        - soup.select('a[href^="url"]')[<a>属性值不为</a>] [<a>查</a>]
        - soup.select('a[href*="url"]')[<a>局部匹配</a>] [<a>查</a>]
        - soup.select('a[href*="url"]')[<a>局部匹配</a>] [<a>查</a>]
    - soup.select_one("选择器")[<a>返回查找回的第一个</a>] [<a>查</a>]

####<b style='color:#A00000'>Traversal</b>

##<b style='color:#580000'>周末放映室</b>
---
`没想到center标签不支持width属性,只能自定义<div>了`
<center >
<div style="width:644px">
![](https://raw.githubusercontent.com/srcmit/source/wiki/photo/Paprika.png)
</div>
</center>

<center>
<embed height="415" width="644" quality="high" allowfullscreen="true" type="application/x-shockwave-flash" src="//static.hdslb.com/miniloader.swf" flashvars="aid=4887477&page=1" pluginspage="//www.adobe.com/shockwave/download/download.cgi?P1_Prod_Version=ShockwaveFlash"></embed>
</center>
