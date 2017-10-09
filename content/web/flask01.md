---
title: "Flask"
layout:  page
collection: "[server目]"
date: 2017-09-07 21:00:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑....</a>

---
- **今日音乐**
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=144199&auto=0&height=66"></iframe>

---

> 无FUCK说

---

[TOC]

###MMP
我刚学,有很多问题...
###设计杂论
---
关于在flask中request对象的概念定位
作为单次参数传递给视图函数的话,每个视图函数都要写,如果增加其他对象,增加了了调用的混乱度,如果设置成全局变量的话,在多线程服务器中是不可能的,毕竟每个线程都会处理不同的request对象,作为全局对象,无法实现多线程,所以flask引入context(上下文)控制request在单个线程里面称为全局对象.
对于上面的话,目前我还有一部分<b style='color:red'>疑问</b>
context 有点像缓冲区

- 请求调度 路由指定请求方法

- 四种钩子 java类似监视器

###9/18 Post
---
本次主要是注重实践,在一下午看完了前四章,还没来的及吸收
开发工具: VIM
语言选择: PY
模板选择: Jinja2
模块选择: python_script, python_bootstrap, python_mement,flask_wtf

<del><del>我说这么多屁话干嘛</del></del>

####信息

1. Flask 中有两种上下文:程序上下文和请求上下文
`Flask 在分发请求之前激活(或推送)程序和请求上下文,请求处理完成后再将其删除。程序上下文被推送后,就可以在线程中使用 current_app 和 g 变量。类似地,请求上下文被推送后,就可以使用 request 和 session 变量。如果使用这些变量时我们没有激活程序上下文或请求上下文,就会导致错误。`
```python
>>> from hello import app
>>> from flask import current_app
>>> current_app.name
RuntimeError: working outside of application context
>>> app_ctx = app.app_context()
>>> app_ctx.push()
>>> current_app.name
'hello'
>>> app_ctx.pop()

#在程序实例上调用 app.app_context() 可获得一个程序上下文
```
<b style='color:red'>至于为什么输出hello,我们以后解释</b>

####使用
1. 先用Flask创造一个app对象
    `Flask初始化参数必须只有一个指定的参数,即程序主模块或包的名字`
    `Flask 用这个参数决定程序的根目录,以便稍后能够找到相对于程
序根目录的资源文件位置`
2. 为了灵活使用app.run(),我们加入<b style='color:yellow'>python_script</b>
`看看Moment对app进行的劳动改造`

```python
class Moment(object):
    def __init__(self, app=None):
        if app is not None:
            self.init_app(app)

    def init_app(self, app):
        if not hasattr(app, 'extensions'):
            app.extensions = {}
        app.extensions['moment'] = _moment
        app.context_processor(self.context_processor)

    @staticmethod
    def context_processor():
        return {
            'moment': current_app.extensions['moment']
        }

    def create(self, timestamp=None):
        return current_app.extensions['moment'](timestamp)

```
3. 动态url匹配的路由<b style='color:red' id='tro001'>存在问题</b>
```python
  @app.route('/user/<name>')
  def user(name):
    return '<h1>Hello, % s!</h1>' % name
  #这三个name必须都是一样的变量名吗?
```

4. 请求 - 响应循环
- 程序和请求上下文
    `Flask 从客户端收到请求时,要让视图函数能访问一些对象,这样才能处理请求。请求对象就是一个很好的例子,它封装了客户端发送的 HTTP 请求。`
    `Flask 使用上下文临时把某些对象变为全局可访问。有了上下文,就可以写出下面的视图函数:`
```python
  from flask import request
  @app.route('/')
  def index():
    user_agent = request.headers.get('User-Agent')
    return '<p>Your browser is % s</p>' % user_agent

  //===============================================
  事实上, request 不可能是
` 全局变量。试想,在多线程服务器中,多个线程同时处理不同客户端发送的不同请求时,每个线程看到的 request 对象必然不同。Falsk 使用上下文让特定的变量在一个线程中全局可访问,与此同时却不会干扰其他线程。
```

###<b style='color:red'>已解决</b>
---
1. the flowing
```python
  if __name__ == '__main__':
    app.run(debug=True)


__name__== ' __main__ ' 是 Python 的惯常用法,在这里确保直接执行这个脚本时才启动开发Web 服务器。如果这个脚本由其他脚本引入,程序假定父级脚本会启动不同的服务器,因此不会执行 app.run() 。只有真正执行到才会运行哦,所以经常用来做测试,我以前说过...
```



###<b style='color:red'>待解决</b>

####基础不稳
---
1. import的加载问题
2. 不太理解每一个工程目录下的隐式的__init__文件
3. [使用顺序-3](#tro001)
4. <del>current_app是什么?</del>
    `当前激活程序的程序实例`
5. current_app有什么用?
