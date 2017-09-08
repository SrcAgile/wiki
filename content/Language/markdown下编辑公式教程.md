---
title: "页面渲染Latex字符"
layout: page
collection: "[四代目]"
date: 2017-08-26 00:00
---
[TOC]

**文档状态：**<a style="color:red;background-color:gray">已完善</a>

# 前言
满怀欢喜的以为Markdown支持了Latex渲染，然而并没有在此WIKI使用,没有办法只能自己渲染，还好网络上有提供的开源的库直接使用。
# MathJax功能引入
## 外联引入使用

```
#官方有可能慢
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
#国内
<script src="//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
```
## 内联Customize Configuration
```
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  #customize
});
</script>
<script type="text/javascript" src="path-to-MathJax/MathJax.js"></script>
```
# 页面使用
## 基本用法
行间公式：
```
$$
"公式"
$$

```
行内公式:```$"公式"$```  注：有的地方可能需要用小括号将公式括起来：```$("公式")$```

## 常见的转字符

- 求和: \sum_{i=1}^n{x_i}  ($(\sum_{i=1}^n{x_i})$)

- 趋近于: \to  ($(\to)$)

- 无穷大: \infty ($(\infty)$)

- 二元关系: \times ($(\times)$), \div ($(\div)$), \pm ($(\pm)$), \circ ($(\circ)$), \cdot ($(\cdot)$)

- 关系运算符：如\leq(≤), \geq(≥), \subset(⊂), \supset(⊃), \in(∈), \bigcup $(\bigcup)$, \bigcap $(\bigcap)$, \iint $(\iint)$, \int $(\int)$;

- 否定关系运算符：如\not=(≠), \not<(≮), \not\supset (⊅);

- 箭头, \leftarrow(←), \rightarrow(→), \longrightarrow(⟶), \uparrow(↑)等;

- 绝对值， \vert{x}\vert ($(\vert{x}\vert)$), \Vert{x}\Vert ($(\Vert{x}\Vert)$), \langle{x}\rangle ($(\langle{x}\rangle)$)

- 其它符号, \nabla(∇), \angle(∠), \forall(∀), \exists(∃), \prime(导数的撇′).

- 而对于专有名词,如一些函数名, 如sin x中的sin, 就要用罗马体, 而不是一般的数学斜体排印,我们可以用sinx, 也可以用TeX提供的直接在函数名前加”\”的方法: sinx,一般的函数均有定义, 如\sin, \cos, \lim, \log等.

## 希腊字母

| 字母名称 | 大写 | markdown语法 | 小写 | markdown语法|
| :-------: |:---:| :--------:|:----:| :-------:|
|alpha |$(A)$|A|$(\alpha)$|\alpha|
|beta|$(B)$|B|$(\beta)$|\beta|
|gamma|$(\Gamma)$|\Gamma|$(\gamma)$|\gamma|
|delta|$(\Delta)$|\Delta|$(\delta)$|\delta|
|epsilon|$(E)$|E|$(\epsilon)$|\epsilon|
||||$(\varepsilon)$|\varepsilon|
|zeta|$(Z)$|Z|$(\zeta)$|\zeta|
|eta|$(E)$|E|$(\eta)$|\eta|
|theta|$(\Theta)$|\Theta|$(\theta)$|\theta|
|iota|$(I)$|I|$(\iota)$|\iota|
|kappa|$(K)$|K|$(\kappa)$|\kappa|
|lambda|$(\Lambda)$|\Lambda|$(\lambda)$|\lambda|
|Mu|$(M)$|M|$(\mu)$	|\mu|
|nu|$(N)$|N|$(\nu)$|\nu|
|xi|$(\Xi)$|\Xi|$(\Xi)$|\xi|
|omicron|$(O)$|O|$(\omicron)$|\omicron|
|pi|$(Pi)$|	\Pi|$(\pi)$|pi|
|rho|$(P)$|	P|$(\rho)$|\rho|
|sigma|$(\Sigma)$|\Sigma|$(\sigma)$|\sigma|
|tau|$(T)$|T|$(\tau)$|\tau|
|upsilon|$(\Upsilon)$|\Upsilon|$(\upsilon)$|\upsilon|
|phi|$(\Phi)$|\Phi|$(\phi)$|\phi|
||||$(\varphi)$|\varphi|
|chi|$(X)$|X|$(\chi)$|\chi|
|psi|$(\Psi)$|\Psi|$(\psi)$|\psi|

## 字体
- Use \mathbb or \Bbb for "blackboard bold":

    - $(\mathbb{abcefghijklmnopqrstuvwxyz})$
    - $(\mathbb{ABCDEFGHIJKLMNOPQRSTUVWXYZ})$

- Use \mathbf for boldface:
    - $(\mathbf{abcefghijklmnopqrstuvwxyz})$.
    - $(\mathbf{ABCDEFGHIJKLMNOPQRSTUVWXYZ})$.

- Use \mathtt for "typewriter" font:
    - $(\mathtt{abcefghijklmnopqrstuvwxyz})$
    - $(\mathtt{ABCDEFGHIJKLMNOPQRSTUVWXYZ})$

- Use \mathrm for roman font:
    - $(\mathrm{abcefghijklmnopqrstuvwxyz})$
    - $(\mathrm{ABCDEFGHIJKLMNOPQRSTUVWXYZ})$

- Use \mathsf for sans-serif font:
    - $(\mathsf{abcefghijklmnopqrstuvwxyz})$
    - $(\mathsf{ABCDEFGHIJKLMNOPQRSTUVWXYZ})$

- Use \mathcal for "calligraphic" letters:
    - $(\mathcal{abcefghijklmnopqrstuvwxyz})$
    - $(\mathcal{ABCDEFGHIJKLMNOPQRSTUVWXYZ})$

- Use \mathscr for script letters:
    - $(\mathscr{abcefghijklmnopqrstuvwxyz})$
    - $(\mathscr{ABCDEFGHIJKLMNOPQRSTUVWXYZ})$

- Use \mathfrak for "Fraktur" (old German style) letters:
    - $(\mathfrak{abcefghijklmnopqrstuvwxyz})$
    - $(\mathfrak{ABCDEFGHIJKLMNOPQRSTUVWXYZ})$

## 常见的表达式

- 分数：$$\frac{a+1}{a-1}$$
语法： \frac{a+1}{a-1}
- $$\sum_{i=0}^n {i^2} ={\frac{(i+1)(i^2+6)}{i-1}}$$
语法：\sum_{i=0}^n {i^2} ={\frac{(i+1)(i^2+6)}{i-1}}

# 参考文献
[MarkDown(LaTex) 数学公式](http://blog.csdn.net/Linear_Luo/article/details/52224996)
[MATHEMATICS meta](https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference)
