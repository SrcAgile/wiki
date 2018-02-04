---
title: "IO层函数简易剖析"
layout: page
collection: "[系统纲]"
date: 2017-08-30 21:00:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---
>

---

- **今日音乐**
```
[网评]

```
###起因
今天写了关于scanf的部分功能,起初只是简单的写一写关于最短代码实现对输入数字检测,为数字则通过,不为则清空重新输入,鉴于scanf并不是一个小小的函数加上编写代码的平台是linux,所以出现了很多疑问与思考,结合对操作系统本身知识与系统工具的了解我准备剖析一下内部原理.
附:内部有技能小彩蛋<(**_**)>

###开始
- 功能要求:数字检测
```c
#代码实现
while(scanf("%d[0-9]",&num)!=1){
  fflush(stdin);
  puts("err...waiting for reinputing...");
}
```
<del>ojbk</del>,好了运行一下吧,呵呵"err.."提示字符死循环,难道fflush不起作用
但是现在没有网没办法上网查怎么办,让我们自己动手解决问题吧啦啦小魔仙!

- 首先
```c
$ man 3 fflush
```
找到DESCRIPTION:
因为我们使用的是fflush(stdin),找到 For input streams这一行,我们看到类似下面的一句话
<b style='background-color:black;color:green'>
For input streams, fflush() discards any buffered data that has been
fetched from the underlying file, but has not been consumed by the application.
</b>
<b>translation:</b>即对fflush传入的一个输入流，fflush会清除已经从<b>输入流</b>中取出但还没有交给程序的数据

现在涉及到系统的概念了,有的新生对系统的概念不太理解<b>输入流</b>和<b>stdin</b>和<b>stdout</b>等等,以至于脑子中没有一个直观的模型去了解其输入,这里不得不吐槽一下scanf,把用户当成高级程序员了...

####输入流、stdin，输入流、stdout关系
这里我不会讲太细致,因为这里需要的计算机基础是较为中等的,若有不会可联系我.
<b>输入流</b>:即输入设备的驱动程序的专门数据缓冲区,为什么要加缓冲区?自然是消除速度差异.
<b>stdin</b>:输入设备文件,这里涉及操作系统概念,设备的存储空间被映射到内存,可以以文件的形式被应用程序使用标准文件操作函数进行操作.

先看看stdin
```c
$ man 3 stdin
Note that in case stdin is
associated with a terminal, there may also be input buffering in the
terminal driver, entirely unrelated to stdio buffering. (Indeed, nor‐
mally terminal input is line buffered in the kernel.)
```
看就更明白了，终端驱动中接收你输入文本的缓冲区就相当于是输入流，和stdin的缓冲区是两回事.


附引:如何查看设备文件呢?
```
$　 ls -l /dev/*
```
你会发现打印很多信息，其中类似
`crw--w----  1 root tty       4,   7 1月   9 10:04 /dev/tty7`
分别代表三类权限+inode号+用户名+设备类型+主设备号+从设备号+加载时间+路径名
但是但你`ls -l /dev/stdin的时候你会发现他没有主设备号`,这里是因为他是一个链接,类似快捷方式,
然后你会发现
`lrwxrwxrwx 1 root root 15 1月   9 10:04 /dev/stdin -> /proc/self/fd/0`
`/proc/self`即当前进程的映射文件,不信你`ls`一下会发现他是一个对应进程号的软连接
这里面涉及到.即/proc/self对应的类似就是pcb结构,而进入这个目录你可以查看当前进程的运行信息,比如`/proc/self/fd`就是他的打开文件表表项们,你可以查看当前进程打开的所有文件号,0,1,2分别代表标准输入,标准输出,标准错误三大设备文件,好了,看到这里我们应该明白了,每个进程都应该有自己的标准输入,输入,错误三大设备文件,但是如果动态改动/dev下面就太烦了.故直接加了软连接引向动态改变的信息,做到`映射`.

这样看来其实关系明确了很多


|应用程序层 |scanf(包含应用缓冲区)|
|   :--:|:--:                     |
|设备驱动层 | 输入流(内核缓冲区)|
|<del>设备文件系统层  </del> |<del>(stdin)</del>|
|硬件层        |    键盘|

这样写其实不太合适,<del>但是适合这里理解,因为stdin是对硬件设备信息映射到文件系统块中,所以硬件层更适合写成文件系统层.,设备驱动可以直接和硬件层进行交互,但是如果文件系统层提供了缓冲</del>
因为设备驱动层的存在,当内核开机执行时会把找到设备将其映射到设备文件夹下即`/dev`,然后用户角度才有了`stdin`,所以用户对stdin的操作会被翻译为 用户->设备驱动-->硬件本身

这样就有了整体的结构,所以我们得知,`fflush`只是将应用程序层的应用缓冲区的缓冲数据掉了,但是设备驱动层的缓冲区还在,至于硬件本身,我们针对键盘来说,它是无法存放数据的,只能依靠驱动层缓冲数据,所以,下一次读的时候,设备驱动层的数据缓冲残留还在,麻鸡,这样是不行的!所以会出现死循环!

有人说vc提供的fflush怎么行,其实我对windows下的硬件布局不太清楚,但是可以肯定的是,他们的确清空了驱动层的数据,死循环并不会出现.

###怎么做
知道原理了,我们应该怎么做?很简单,使用笨方法将数据全读出来!怎么读?
看着!
```c
while(scanf("%d",&num)!=1){
  scanf("%*[^\n]");//**读到知道遇到遇见回车字符为止!
  getchar();//用这个吸收掉最后一个回车符
  puts("err...");
}
#为什么结尾会是回车?我猜测回车键的按下会向应用层发送设备准备好中断信号!让scanf进行读,
#为什么不是scanf轮询?这特么还用说么?轮询的一直抢占时间片,cpu会飙升的!
#这个如果有硬件基础,可以翻阅微机原理关于<IO控制方式章节>,即通知阻塞scanf进程进行读!属于中断
```


###scanf部分剖析
事到如今,我们对应用层以下的行为都比较了解了,但是应用层的规则还是不够了解,这里可以贴一篇我以前看的文章[scanf详解](http://m.blog.csdn.net/21aspnet/article/details/174326)
####scanf
- 区分%c%d%s对缓冲区操作遇见空格回车tab等的不同反应

- scanf<b style='color:red'>截止输入一个数据</b>包括三种情况
① 遇空格、"回车"、"跳格"键。
② 遇宽度结束。
③ 遇非法输入。
- 但是遇到这些东西他们对这些东西处理的方式不一样
① %d是跨过空格即删除掉这些空格然后放到应用缓冲区
② %c不删除,虽然会截止数据输入,但是当做普通字符
③ %s应该类似%d吧,这里挖坑,等待以后填上.
