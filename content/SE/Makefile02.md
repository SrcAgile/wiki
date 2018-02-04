---
title: "Makefile实例"
layout:  page
collection: "[工程目]"
date: 2017-11-11 11:11:11
---

**文档状态：**<a style="color:red;background-color:gray">编辑....</a>

---
- **今日音乐**
NONE

---
>

---
[TOC]

###Instance
>以下为内核模块编译链接的一个实例，虽然很多人会写但是对具体的含义还是不太了解，因为我接触的也不够多，所以仅仅是写出自己理解的。


```makefile
#makefile.hello
ifneq ($(KERNELRELEASE),)
obj-m:=hello.o
else
KERNELDIR:=/lib/modules/$(shell uname -r)/build
PWD:=$(shell pwd)
default:
        $(MAKE) -C $(KERNELDIR)  M=$(PWD) modules
clean:
        rm -rf *.o *.mod.c *.mod.o *.ko
endif
```
- [始]. 在shell执行`$  make`
- [0]. 整个阶段我们会执行两次`makefile.hello`，不理解？细细道来。
- [1]. 第一次加载`makefile.hello`，首先因为我们没有定义`KERNELRELEASE`所以一开始`obj-m:=hello.o`不会执行
- [2]. 所以开始从else执行，其中因为[始]中输入的只是`make`,所以默认执行`default`，其中要理解`-C`&`M`的作用见[makefile01](./makefile01.html)我所写的
    `[引]# Use make M=dir to specify directory of external module to build`
- [3]. 好了精彩部分来了，make 在-C链接到的目录下执行makefile，然后寻找`modules`对象开始进行构造所以就跳转到找`Modules`，很好我们找到了

```makefile
PHONY += modules
modules: $(vmlinux-dirs) $(if $(KBUILD_BUILTIN),vmlinux)
	$(Q)$(AWK) '!x[$$0]++' $(vmlinux-dirs:%=$(objtree)/%/modules.order) >

$(objtree)/modules.order
	@$(kecho) '  Building modules, stage 2.';
	$(Q)$(MAKE) -f $(srctree)/scripts/Makefile.modpost
	$(Q)$(MAKE) -f $(srctree)/scripts/Makefile.fwinst obj=firmware __fw_modbuild
```
- [4]. 当执行到`$(Q)$(MAKE) -f $(srctree)/scripts/Makefile.modpost`我们要看看`Makefile.modpost`为何方神圣
```makefile
#意思是如果在KBUILD_EXTMOD路径下找到Kbuild就include $(KBUILD_EXTMOD)/Kbuild,
#否则include $(KBUILD_EXTMOD)/Makefile。这个路径正好包括我们自己的代码路径，
#因此，模块顺利的被编译链接。
include $(if $(wildcard $(KBUILD_EXTMOD)/Kbuild), \
             $(KBUILD_EXTMOD)/Kbuild, $(KBUILD_EXTMOD)/Makefile)
```
- [5]. 那么问题来了我到底是引入`$(KBUILD_EXTMOD)/Kbuild`，还是` $(KBUILD_EXTMOD)/Makefile)`？另外`$(KBUILD_EXTMOD)`是什么鬼啊？
```makefile
#`$(KBUILD_EXTMOD)`是这种鬼啊
# Use make M=dir to specify directory of external module to build
# Old syntax make ... SUBDIRS=$PWD is still supported
# Setting the environment variable KBUILD_EXTMOD take precedence
ifdef SUBDIRS
KBUILD_EXTMOD ?= $(SUBDIRS)
endif

ifeq ("$(origin M)", "command line")
KBUILD_EXTMOD := $(M)
endif
#大致意思就是如果使用make命令且带有命令行参数 M="XXX",那就用M初始化这个KBUILD_EXTMOD := $(M)，好了最开始我们设置的M是什么？原来是$(shell pwd)，就是一开始执行命令的当前目录，那就好说了，现在我特喵又执行了一次makefile，而这一次我们的KERNELRELEASE已经被定义了，所以只执行obj-m就好了，什么不知道为什么已经定义了，因为-C指向的/lib/modules/$(shell uname -r)/build是一个软连接指向/usr/src/linux，而/usr/src/linux目录下就有一个makefile里面写了
KERNELRELEASE=$(VERSION).$(PATCHLEVEL).$(SUBLEVEL)$(EXTRAVERSION)$(LOCALVERSION)
ok至此已经执行到obj-m了
```
- [6]. obj-m干了什么?
    `obj-m +=xxx.o该模块不会编译到zImage 但会生成一个独立的xxx.ko 静态编译,至于obj-m在哪里声明，我暂时不清楚，但是你可用make -n测试`

###几个命令
> 这几个命令对模块使用时有用，但是什么用还是不要做伸手党了

- modinfo
- dmesg |tail -1#查看最后一条就行了

###问题
- 有人说为什么我的insmod之后内核没有打印而卸载(rmmmod)就全部打印出来了？
    `这个问题和缓冲区的机制有关系，直接在printk("")后面加个\n就行了即printk("***\n"),强制缓冲区刷新即可！`
    `关于这个缓冲区的问题我会再讲的.`

###Reference

- [make M option](http://m.blog.csdn.net/lixiangminghate/article/details/49340527)
- [make refer](http://blog.csdn.net/isnipe/article/details/3933941)
- [refer2](https://www.cnblogs.com/cyc2009/p/3954249.html)
