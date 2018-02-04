---
title: "映射类型"
layout: page
collection: "[Python]"
date: 2015-11-07 20:20:37
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---
> ...

---
- **今日音乐**
```
暂无评论
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=386830&auto=0&height=66"></iframe>

---

[TOC]

###Intro
PYTHON中唯一的映射类型字典,字典是无序的,因为他不需要顺序,他是通过键值进行索引的,他的键必须是不可更改的,因为受哈希算法的约束,一旦更换对象后,根据哈希算法就再也需找不到索引的对象了.

- 与list&tuple的关系
    `通过keys() or values() 返回一个列表`
    `通过items()返回一个[(key1,value1),(key2,value2),....]的以元组为列表项`
    `sorted(dict)返回一个list对象,当然只是key的排序`

- 相关联的内建函数
    `fromkeys()可以创建一个默认字典,字典key可以有相同的value`
    `>> {}.fromkeys(('x','y'),-1)`
    `如果不提供value则默认NONE`
- 键值检测[获取状态信息]
    `dict1.has_key('server')`
- 键值获得[获得数据信息]
    `dict1[key]`
- 更新&添加
    `dict1[key]=new value`
- 删除
    `del dict1[key1] #delete a specific value`
    `dict1.clear()   #del whole dict`
    `dict1.pop(key1) #del and return`
- 操作符[分为标准类型和映射类型 只介绍映射类型操作符]
    `[]   键查找操作符`
    `in/not in 键成员操作符`
- 内建函数和工厂函数
    `不谈标准类型函数`
    `dict工厂函数很有意思,可以迭代初始化参数生成字典 见->EX1`

 <b>映射类型相关函数</b>

    `dict()     工厂函数`
    `len()      返回键值对的数目`
    `hash()     可哈希化判断返回哈希值,就是判断一个对象是否可以作为key,如果可以返回hash值,否则返回error错误栈`

```python
  EX1:
    >>>dict(x=1,y=2)
    {'y':2,'x':1}
    >>> dict([(i-1,i+1) for i in range(1,4)])
    {0: 2, 1: 3, 2: 4}

```

- <b style='color:red'>再谈几个字典类型函数</b>
    `dict.clear() 全部清空`
    `dict.copy()  浅拷贝`
    `[重要]dict.get(key,defalut=NONE)--->对字典 dict 中的键 key,返回它对应的值 value,如果字典中不存在此键,则返回 default 的值(注意默认值为NONE)`
    `dict.pop(key[,default])   --->类似get方法,但是其一default没有给出,其二这个是删除并且返回,如果没有defalut则return ERR`
    `dict.iter()`
    `[重要]dict.update(dict2) 将字典 dict2 的键-值对添加到字典 dict,注意因为dict是可变对象,对可变对象的修改并不会返回值,还要注意的是更新时遇到key一样但value不一样的会出现更新失败`
    `dict.setdefault(key,default=None) 若存在key则返回其value,否则添加或者<del>更新</del>键值对,如果存在却没有返回证明插入键值对出现键相同值冲突现象,此种情况默认存储原有对象`
- 字典比较算法[单独讲]
    `step1: 比较长度    return max else step2`
    `step2: 依次比较key return max else step3`
    `step3: 依次比较val return max `


- 拷贝
    `//[写错地方了]从浅拷贝和深拷贝角度上来看,class也是一种type,和其他的序列类型`


###集合[sets]
####分类
- 可变集合[set]
    `可增删`
    `不可哈希`
- 不可变集合[frozenset]
    `不可增删`
    `可哈希[故可以作为字典键]`

- 两个工厂函数
    `set(object)`
    `frozenset(object)`
    `[Caution]提供的object必须是可迭代的(序列或者迭代器或者支持迭代的对象例如文件或者字典)`

- 一个特殊方法
    `a^b  对称差分,返回一个要么只属于a,要么只属于b的集合元素组成的集合`
- <b>混合运算[科学人员需要知道]</b>
    `指的是参与运算的左值和右值一个是set and the other is frozenset`
    `混合运算的结果的类型取决于运算符的左值`
```python
  >>> a = set("abcdw")
  >>> b = frozenset("ahijk")
  >>> a|b
  set(['a', 'c', 'b', 'd', 'i', 'h', 'k', 'j', 'w'])
  >>> b|a
  frozenset(['a', 'c', 'b', 'd', 'i', 'h', 'k', 'j', 'w'])
  #[!]因为运算产生新对象,所以不必考虑frozenset变不变的问题了!
```
- 仅可以运用在可变集合的操作
    `涉及科学运算而不牵扯工程过多,仅列举`
    `[1] (Union) Update ( |= )   这个更新方法从已存在的集合中添加(可能多个)成员,此方法和 update()等价.`
    `[2] 保留/交集更新( &= )       保留(或交集更新)操作保留与其他集合的共有成员。此方法和 intersection_update()等价`
    `[3] 差更新 ( –= )  对集合 s 和 t 进行差更新操作 s-=t,差更新操作会返回一个集合,该集合中的成员是集合 s 去除掉集合 t 中元素后剩余的元素。此方法和 difference_update()等价.`
    `[4] 对称差分更新( ^= )对集合 s 和 t 进行对称差分更新操作(s^=t),对称差分更新操作会返回一个集合,该集合中的成员仅是原集合 s 或仅是另一集合 t 中的成员。此方法和 symmetric_difference_update()等价.`

- <b style='color:red'>集合的内建方法</b>
    `[!]其实我并不是原因写关于集合的内建方法的,因为他是面向对象的产物,功能大部分都是对集合类型运算符的函数化实现,至于形成方法自然是有它的好处的,但是这些功能我们已经在运算符里面看到,所以他们的好处到底是什么?`
    `本来我以为好处就是将返回值覆盖原有对象,但是通过测试发现并没有覆盖,关于这个问题以后再谈论`

    <b style='color:red'>像你看到的, 很多内建的方法几乎和操作符等价。我们说"几乎等价",意思是它们间是有一个重要区别: 当用操作符时,操作符两边的操作数必须是集合。 在使用内建方法时,对象也可以是迭 代 类 型 的 。 为 什 么 要 用 这 种 方 式 来 实 现 呢 ? Python 的 文 档 里 写 明 : 采 用 易 懂 的set('abc').intersection('cbs') 可以避免用 set('abc') [and] 'cbs' 这样容易出错的构建方法。</b><b style='color:green'>这里说的对象就是指集合中的元素对象.</b>
    表 7.5 可变集合类型的方法
方法名
操作
s.update(t)
用 t 中的元素修改 s, 即,s 现在包含 s 或 t 的成员
s.intersection_update(t) s 中的成员是共同属于 s 和 t 的元素。
s.difference_update(t)
s 中的成员是属于 s 但不包含在 t 中的元素
s.symmetric_difference_update(t) s 中的成员更新为那些包含在 s 或 t 中,但不
是s
和 t 共有的元素
s.add(obj)
在集合 s 中添加对象 obj
s.remove(obj)
从集合 s 中删除对象 obj;如果 obj 不是集合 s 中的元素(obj not
in s),将引发 KeyError 错误
s.discard(obj)
如果 obj 是集合 s 中的元素,从集合 s 中删除对象 obj;
s.pop()
删除集合 s 中的任意一个对象,并返回它
s.clear()
删除集合 s 中的所有元素
