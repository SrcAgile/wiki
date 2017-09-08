---
title: "[Regex纲]Regex概论"
layout: page
collection: "[四代目]"
date: 2017-08-31 21:00:00
---

**文档状态：**<a style="color:red;background-color:gray">思索中....</a>

---
- **今日音乐**
今日无音乐,只有乱.
---
> 混乱的思想,难以安眠的嘈杂,生命的歌与诗,坠落在绚烂多彩的深渊.

---

以下均为不成熟的思想,为了记录脑中知识,故将知识扔到这一片荒芜之地.
1. 规则即命题
2. 将表达式看做命题
3. 将匹配分为确定匹配与规则匹配

好了现在我要创造正则表达式,那么我知道正则表达式的目的就是描述模式,<b>描述</b>本身应该有什么性质,对的，他要遵循<b>精准性</b>,<b>客观性</b>还有<b>灵活性</b>这意味这要减小描述的复杂度,为了减小描述的复杂度，那就提高符号抽象程度。
此处引入集合论的混乱观点，毕竟数学都是建立在它上面的，我使用一下应该不会有太大的出入，那么开始吧。
1. 集合内的元素被认为元对象
2. 集合上面的运算形成新的集合
---
[草稿.篇]
```
大局观：代表对元素集合的描述，考虑的是整个元素集合的性质,例如数量
　微观：代表对单个元素的描述,考虑的是单个索引上元素的值，例如位置和值
```
上面也说过描述要具有精准性，我们要知道他的位置，他的值才能唯一描述他，他的值可能是常项也可能是变项，常项可能包括空集,变项引入符号‘|’,所以元素是变动的“null|{set}”<--->"{set}?"
$$Regex \Rightarrow \begin{cases} 大局观  &  \begin{cases}
                                  数量观 &  \begin{cases}有限 \\\
                                                        无限(有限是无限的子集)
                                            \end{cases}\\\    
                                  \end{cases}
                                  \\\
                        \\\
                        微观    &  \begin{cases}  
                                    基本元素 & \begin{cases}常项 & \begin{cases}特殊元素 \\\
                                                                              其他元素
                                                                   \end{cases}\\\    
                                                          变项 & \\\
                                                \end{cases}
                                    \\\
                                    位置 &     \begin{cases}绝对位置\\\
                                                           相对位置\\\
                                              \end{cases}
                                     \\\
                                  \end{cases}\\\
                         \end{cases}
$$

---
对草稿篇出现疑问
1. 量词的引入是为了简化描述吗?
2. 有点混乱...
3. 我一开始为什么要执着引入谓词逻辑？

以下为写在本子上的公式

$$运算\begin{smallmatrix}&抽象&\\\ &\Longleftrightarrow&\end{smallmatrix}规则\begin{smallmatrix}&实体化&\\\ &\Longleftrightarrow&\end{smallmatrix}命题\begin{smallmatrix}&使用&\\\ &\longrightarrow&\end{smallmatrix}谓词$$

看来真正的理解只能去写一个正则引擎了QAQ
