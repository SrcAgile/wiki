---
title: "千里之行"
layout: page
collection: "[Python]"
date: 2015-09-11 21:00:00
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---
> 人生苦短

---
- **今日音乐**
```
暂无评论
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=386830&auto=0&height=66"></iframe>

---
[TOC]

##9.23更新

###特性
---
- 健壮性
- 易维护性
- 可移植
- 易读易学
- 面向对象
- 可拓展
- 可升级
- 高阶函数(haskell, lisp)
- 内存管理
- 非编译型

###命令行特性
---
- -c "cmd"
    `python -c "print (1+2)"`
- -d
    `调试输出`
- -v
    `冗余输出`
- -m mod
    `模块以脚本形式运行`
- -O
    `生成优化的字节码 .pyo`
- -S
- -Q

###脚本书写
---
- 文本开头搜索python解释器
    - 开头`#!Python解释器的路径`
        `#!/usr/bin/python`
    - 或者`#!/usr/bin/env python`
        `从env环境变量中搜索`     

###开发环境
---
- VIM+PYTHON插件
- pycharm
- spyder

###IO
---
####模块
- sys.stderr
    `标准错误输出`
####输出函数
- `交互使用repr()`
- `print调用str()`
- print输出默认加换行
    `使用print a, 加逗号即可改变,但是自动加空格`
- 格式化输出
    `print "%s %d" %(a,b)`
- 重定向输出
    `print >>`
####输入函数
- raw_input()`py 2.x`
- input `py 3.x`


###语法
---

####书写
- `\`继续上一行
- `;`两个语句在一行
- `:`将代码头体分离
- 缩进

####变量
- 命名
    `_xxx   -->不用'from module import 导入'`
    `__xx__ -->系统定义名字`
    `__xx   -->类中私有变量名`

####运算
#####运算符
- `**(乘方)`
- `//`浮点除法(四舍五入)真正的除法
- `/`传统除法(操作数为整型时执行地板除)
- `!= <>`都是不等于
- 逻辑运算 `and or not`
- 增量赋值 +=,-=
- 多重赋值 x =y =z=1
- 多元赋值 a,b = b+1, a[涉及元祖特性][可实现简单交换]
    `x,y = y,x[不明觉厉]显然赋值之前已经做好了计算`
- 友情提示:多使用括号,增强代码可读性

#####操作数
- 注意complex虽然不常用
- 字符串知识(切片,索引,克莱尼星号,连接)<-[只读]
- 列表元素个数和值可变 (切片,索引,克莱尼星号,连接)<-[读写]
- 元组元素个数和值不可变(可切片,克莱尼星号,索引,连接)<-[只读]
- 字典 (<b style='color:red'>有重点待续...</b>)


####控制结构
- <del>没什么好讲的</del>
- 注意缩进

#####循环
<del>不值得专门一节写</del>

- 搞清可迭代对象(序列/迭代器)



###函数
---
####分类
1. 有副作用
2. 无副作用

####常见内建函数

- abs()
    `取绝对值`
- dir([obj])
    `显示对象属性,如果无参数,则显示全局变量的名字`
- help([obj])
    `帮助`
- int([obj])
    `将对象转换为整数`
- len(obj)
    `返回对象长度`
- open(fn,mode)
    `fn为文件名或者handle, mode取['r','w']`
- range([start],stop[,step])
    `生成列表0~n-1`
    `常见搭配 for i in range(len(str))`

- strip([characters])
    `返回移除行首行尾的characters字符,缺省为\n`
- str(obj)
    `将一个对象转换为字符串`
- type(obj)
    `返回对象类型`

####参数问题
`暂时书写架构`
- 默认参数
- 可变参数
- 关键字参数
- etc



###类
---
###使用
- 定义
    `class classname(baseclass[es]):`
- 创建
    `a = classB(pram)`
####方法
`做到访问权限控制`
1. 特殊方法
    `__method__()`
    ``
####对象
- 自身实例引用
    `self类似this`
- [python对象详解](./pyobject.md)




###模块
---
`这个是我现在还比较方的东西`
1. 每个python脚本文件都可以当做一个模块
2. 模块太大?可以自己<b style='color:red'>拆</b>



####常见
- sys
    - platform
    - version
    - stdout

###软件工程
---
####语言风格
- (见变量命名准则)
- [pythonic](http://www.Python.org/dev/peps/pep-0007/)
```python
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
>>>

```

####模块结构和布局
```c
结构示意图:

# (1) 起始行(Unix)
        |___[有起始行就能够仅输入脚本名字来执行脚本]
# (2) 模块文档
        |___[简要介绍模块的功能及重要全局变量的含义,
             模块外可通过 module.__doc__ 访问这些内容]
# (3) 模块导入
        |___[导入当前模块的代码需要的所有模块;每个模块仅导入一次(当前模块被加载时);
             函数内部的模块导入代码不会被执行, 除非该函数正在执行]
# (4) 变量定义
        |___[这里定义的变量为全局变量,除非必须,否则就要尽量使用局部变量代替全局变量]
# (5) 类定义
        |___[所有的类都需要在这里定义。当模块被导入时 class 语句会被执行, 类也就会被定义
             类的文档变量是 class.__doc__]
# (6) 函数定义
        |___[此处定义的函数可以通过 module.function()在外部被访问到,当模块被导入时
             def 语句会被执行, 函数也就都会定义好,函数的文档变量是 function.__doc__]
# (7) 主程序
        |___[论这个模块是被别的模块导入还是作为脚本直接执行,都会执行这部分代码
             不会有太多功能性代码,而是根据执行的模式调用不同的函数]
```

```python
EXAMPLE:hello.py
#!/usr/bin/env python            [1]#$ chmod +x;./hello.py

["document"|'document']                      [2]

import sys                     [3]

debug =true                   [4]

class new(object):            [5]
    pass           

def real():                  [6]
    pass

if __name__ == '__main__':   [7]#模块测试专用,决定了模块的运行导向
    real()
```

####文档
- 注释
    `#单行`
    `自定义范围 ''' comment '''`
- 动态获得文档字串
    `FORMAT:obj.__doc__`
    `EXAMPLE:print sys.__doc__`

####异常控制
- 捕获try-except-else
```python
  try:
    expression;
  except:
    expression;
  else:
    when try do not has any error;
```
- 生成`raise()`
- 检验文件存在
    `os.path.exists()`
####优化
1. 尽量将模块变量替换为本地变量,降低询问时间
```python
#如果经常使用 os.linesep,那就替换吧,因为每次使用 1.确认os是模块 2.确认linesep全局变量在os中存在太麻烦了
temp = os.linesep
```


###高级特性
---
- 列表解析
    `暂时不写 特牛x`
- 内存管理[我在python剖析里面会说]
    `有本书上面写引用计数,应该不对,因为一旦出现引用环就出现故障,还有一个循环垃圾收集器吧`
    `但是无可厚非,python虚拟机实现了内存管理`
    `del的使用,显式销毁`



###FAQ
---
1. 为什么python不需要变量名和变量类型声明?
2.不太了解元组赋值机制呢?
3.如何修改python命令提示符?

###获取帮助
---
1. 使用help(函数名)`交互模式使用`
2. 什么是PEP?
    `PYTHON增强提案`
3. 查询关键字
    `keyword模块`
    `   |___iskeyword()`
    `   |___关键字列表`
4. 查询系统保留字(build_in)
    `__builtins__模块`
5. 获得模块文档帮助
    `print module.__doc__`
6. [关键字帮助](../Language/pythonkey.html)
7. 工具
    - Debugger --->pdb
    - logger   --->logging[five layer log system]
    - Profilers--->profile,hotshot,cProfile//性能测试
###更新日志
---
1. 9/19添加大量知识框架,属于低级水平知识
2. 9/20
