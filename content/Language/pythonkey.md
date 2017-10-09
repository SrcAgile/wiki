---
title: "关键字[P]概念"
layout: page
collection: "[三代目]"
date: 2017-08-28 21:00:00
---

**文档状态：**<a style="color:red;background-color:gray">更新....</a>
**文档类型：**<b>[ [引](https://www.ibm.com/developerworks/cn/opensource/os-cn-pythonwith/) ]</b>

- **今日音乐**
```
[自.评论]
这个女的竟然不为大众所知
```
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=409102911&auto=0&height=66"></iframe>
---

[TOC]
##关键字
###<b style='color:#67ac82'>with</b>
---
####概念
- <b>上下文管理协议（Context Management Protocol）</b>：包含方法 `__enter__()` 和 `__exit__()`，支持该协议的对象要实现这两个方法。
- <b>上下文管理器（Context Manager）</b>：支持上下文管理协议的对象，这种对象实现了`__enter__()` 和 `__exit__()` 方法。上下文管理器定义执行with 语句时要建立的运行时上下文，负责执行 with 语句块上下文中的进入与退出操作。通常使用 with 语句调用上下文管理器，也可以通过直接调用其方法来使用。
- <b>运行时上下文（runtime context）</b>：由上下文管理器创建，通过上下文管理器的 `__enter__()`和`__exit__()` 方法实现，`__enter__()` 方法在语句体执行之前进入运行时上下文，`__exit__()` 在语句体执行完后从运行时上下文退出。with 语句支持运行时上下文这一概念。
- <b>上下文表达式（Context Expression）</b>：with 语句中跟在关键字 with 之后的表达式，该表达式要返回一个上下文管理器对象。
- <b>语句体（with-body）</b>：with 语句包裹起来的代码块，在执行语句体之前会调用上下文管理器的 `__enter__()` 方法，执行完语句体之后执行`__exit__()` 方法。
####格式
- 标准用法
```python
1:    with context_expression [as target(s)]:
2:         with-body
```
```
这里 context_expression 要返回一个上下文管理器对象，该对象并不赋值给 as 子句中的 target(s) ，
如果指定了 as 子句的话，会将上下文管理器的 __enter__() 方法的返回值赋值给 target(s)。
target(s) 可以是单个变量，或者由“()”括起来的元组（不能是仅仅由“,”分隔的变量列表，必须加“()”）。
Python 对一些内建对象进行改进，加入了对上下文管理器的支持，可以用于 with 语句中，比如可以自动关闭
文件、线程锁的自动获取和释放等。假设要对一个文件进行操作，使用 with 语句可以有如下代码：
```

- 操作文件对象[with]
```python
1.with open(r'somefileName') as somefile:
2.    for line in somefile:
3.        print line
4.        # ...more code
```
- 操作文件对象[try/except]
```python
1.somefile = open(r'somefileName')
2.try:
3.    for line in somefile:
4.        print line
5.        # ...more code
6.finally:
7.    somefile.close()
```
- with 语句执行过程描述
```python
1.context_manager = context_expression
2.exit = type(context_manager).__exit__ 
3.value = 4.type(context_manager).__enter__(context_manager)
5.exc = True   # True 表示正常执行，即便有异常也忽略；False 表示重新抛出异常，需要对异常进行处理
6.try:
7.    try:
8.        target = value  # 如果使用了 as 子句
9.        with-body     # 执行 with-body
10.    except:
11.        # 执行过程中有异常发生
12.        exc = False
13.        # 如果 __exit__ 返回 True，则异常被忽略；如果返回 False，则重新抛出异常
14.        # 由外层代码对异常进行处理
15.        if not exit(context_manager, *sys.exc_info()):
16.            raise
17.finally:
18.    # 正常退出，或者通过 statement-body 中的 break/continue/return 语句退出
19.    # 或者忽略异常退出
20.    if exc:
21.        exit(context_manager, None, None, None)
22.    # 缺省返回 None，None 在布尔上下文中看做是 False

```
```python
1.执行 context_expression，生成上下文管理器 context_manager
调用上下文管理器的 __enter__() 方法；如果使用了 as 子句，则将 __enter__()
方法的返回值赋值给 as 子句中的 target(s)执行语句体 with-body
2.不管是否执行过程中是否发生了异常，执行上下文管理器的 __exit__() 方法，
__exit__() 方法负责执行“清理”工作，如释放资源等。如果执行过程中没有出现异常，
或者语句体中执行了语句 break/continue/return，则以 None 作为参数
调用 __exit__(None, None, None) ；
3.如果执行过程中出现异常，则使用 sys.exc_info 得到的异常信息为参数
调用 __exit__(exc_type, exc_value, exc_traceback)出现异常时，
如果 __exit__(type, value, traceback) 返回 False，则会重新抛出
异常，让with 之外的语句逻辑来处理异常，这也是通用做法；如果返回 True，
则忽略异常，不再对异常进行处理
```
####自定义上下文管理器
开发人员可以自定义支持上下文管理协议的类。自定义的上下文管理器要实现上下文管理协议所需要的 __enter__() 和 __exit__() 两个方法：
context_manager.__enter__() ：进入上下文管理器的运行时上下文，在语句体执行前调用。with 语句将该方法的返回值赋值给 as 子句中的 target，如果指定了 as 子句的话
context_manager.__exit__(exc_type, exc_value, exc_traceback) ：退出与上下文管理器相关的运行时上下文，返回一个布尔值表示是否对发生的异常进行处理。参数表示引起退出操作的异常，如果退出时没有发生异常，则3个参数都为None。如果发生异常，返回
True 表示不处理异常，否则会在退出该方法后重新抛出异常以由 with 语句之外的代码逻辑进行处理。如果该方法内部产生异常，则会取代由 statement-body 中语句产生的异常。要处理异常时，不要显示重新抛出异常，即不能重新抛出通过参数传递进来的异常，只需要将返回值设置为 False 就可以了。之后，上下文管理代码会检测是否 __exit__() 失败来处理异常
下面通过一个简单的示例来演示如何构建自定义的上下文管理器。注意，上下文管理器必须同时提供 __enter__() 和 __exit__() 方法的定义，缺少任何一个都会导致 AttributeError；with 语句会先检查是否提供了 __exit__() 方法，然后检查是否定义了 __enter__() 方法。
假设有一个资源 DummyResource，这种资源需要在访问前先分配，使用完后再释放掉；分配操作可以放到 __enter__() 方法中，释放操作可以放到 __exit__() 方法中。简单起见，这里只通过打印语句来表明当前的操作，并没有实际的资源分配与释放。

- 自定义支持 with 语句的对象
```python
1.class DummyResource:
2.def __init__(self, tag):
3.        self.tag = tag
4.        print 'Resource [%s]' % tag
5.    def __enter__(self):
6.        print '[Enter %s]: Allocate resource.' % self.tag
7.       return self   # 可以返回不同的对象
8.    def __exit__(self, exc_type, exc_value, exc_tb):
9.        print '[Exit %s]: Free resource.' % self.tag
10.        if exc_tb is None:
11.            print '[Exit %s]: Exited without exception.' % self.tag
12.        else:
13.            print '[Exit %s]: Exited with exception raised.' % self.tag
14.            return False   # 可以省略，缺省的None也是被看做是False
```
DummyResource 中的 __enter__() 返回的是自身的引用，这个引用可以赋值给 as 子句中的 target 变量；返回值的类型可以根据实际需要设置为不同的类型，不必是上下文管理器对象本身。
__exit__() 方法中对变量 exc_tb 进行检测，如果不为 None，表示发生了异常，返回 False 表示需要由外部代码逻辑对异常进行处理；注意到如果没有发生异常，缺省的返回值为 None，在布尔环境中也是被看做 False，但是由于没有异常发生，__exit__() 的三个参数都为 None，上下文管理代码可以检测这种情况，做正常处理。
下面在 with 语句中访问 DummyResource ：

- 使用自定义的支持 with 语句的对象
```python
1.with DummyResource('Normal'):
2.   print '[with-body] Run without exceptions.'
3.
4.with DummyResource('With-Exception'):
5.    print '[with-body] Run with exception.'
6.    raise Exception
7.    print '[with-body] Run with exception. Failed to finish statement-body!'
```
第1个 with 语句的执行结果如下：

- with 语句1执行结果

```python
Resource [Normal]
[Enter Normal]: Allocate resource.
[with-body] Run without exceptions.
[Exit Normal]: Free resource.
[Exit Normal]: Exited without exception.
```
可以看到，正常执行时会先执行完语句体 with-body，然后执行 __exit__() 方法释放资源。
第2个 with 语句的执行结果如下：

- with 语句2执行结果

```python
Resource [With-Exception]
[Enter With-Exception]: Allocate resource.
[with-body] Run with exception.
[Exit With-Exception]: Free resource.
[Exit With-Exception]: Exited with exception raised.

Traceback (most recent call last):
  File "G:/demo", line 20, in <module>
   raise Exception
Exception
```
可以看到，with-body 中发生异常时with-body 并没有执行完，但资源会保证被释放掉，同时产生的异常由 with 语句之外的代码逻辑来捕获处理。
可以自定义上下文管理器来对软件系统中的资源进行管理，比如数据库连接、共享资源的访问控制等。Python 在线文档 <b>[Writing Context Managers](http://docs.python.org/release/2.6/whatsnew/2.6.html#module-contextlib)</b> 提供了一个针对数据库连接进行管理的上下文管理器的简单范例。

###<b style='color:#67ac82'>变量</b>
---
####<b>sys.argv</b>
`sys`模块有一个`argv`变量，用list存储了命令行的所有参数。argv至少有一个元素，因为第一个参数永远是该`.py`文件的名称，例如：

运行`python hello.py`获得的`sys.argv`就是``['hello.py']``；

运行`python hello.py -t`获得的sys.argv就是`['hello.py', '-t']`。

####<b>`__name__`</b>
```python
#hello.py
1. def hello():
2.    print "hello"
3.if __name__=='__main__':
4.    hello()
```
当我们在命令行运行hello模块文件时，Python解释器把一个特殊变量`__name__`置为`__main__`，而如果在其他地方导入该hello模块时，if判断将失败，因此，这种if测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是<b style='color:green'>运行测试</b>。

```python
正经测试:
1. 模块导入测试
>>> import hello
>>> hello.__name__
'hello'
>>>

2. 模块运行测试
$ python hello
$ hello

答案:
可以证明导入时 __name__为模块名字
直接运行脚本时 __name__为__main__
```

- 测试
```bash
$ python3 hello.py
Hello, world!
$ python hello.py World
Hello, World!
```
```bash
$ python3
Python 3.4.3 (v3.4.3:9b73f1c3e601, Feb 23 2015, 02:52:03)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import hello
>>>
#屁都没有
```

###<b style='color:#67ac82'>方法</b>
---
#### <b>`__str__`</b>
- 与object绑定print方法
```python
>>> a = 10
>>> print a   #调用__str__()
>>> 10
```
####<b>`__repr__`</b>
- 交互模式下输出调用
```python
>>> a = 10
>>> a   #调用__repr__()
>>> 10
```
####<b>`__iter__`</b>
- 返回迭代对象
####<b>`__next__`</b>
- 迭代对象调用
####<b>`__getitem__`</b>
- 下标取值
- 切片
####<b>`__getattr__`</b>
- 动态返回属性
- 动态返回函数 [可以写成高级函数或return 朗姆达表达式]
- 可以实现链式调用chain()<b style='color:green'>[很棒]</b>
####<b>`__call__`</b>
`任何类，只需要定义一个__call__()方法，就可以直接对实例进行调用`
```python
1.class Student(object):
2.    def __init__(self, name):
3.        self.name = name
4.
5.    def __call__(self):
6.        print('My name is %s.' % self.name)
```
```bash
>>> s = Student('hi')
>>> s() # self参数不要传入
My name is hi.
```
