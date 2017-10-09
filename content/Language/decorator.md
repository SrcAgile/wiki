---
title: "装饰器模式DP"
layout: page
collection: "[设计目]"
date: 2017-08-26 21:00:00
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

- **今日音乐**

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=390258&auto=0&height=66"></iframe>

---

[TOC]

> OOP中的装饰器模式;java曾经使用组合加继承实现过装饰器模式[这好像不值得写：）]

##介绍
1. **[简.明]** 允许向一个现有的对象添加新的功能，同时又不改变其结构。这种类型的设计模式属于结构型模式，它是作为现有的类的一个包装。
2. **[界.限]** 在保持类方法签名完整性的前提下，提供了额外的功能[OOP]
3. **[场.景]** 扩展一个类的功能/动态增加功能，动态撤销。
4. **[萌.物]**  孙悟空[本质不变,动态七十二变]
5. **[缺.点]** 层层嵌套,过于复杂
6. **[优.点]** 无父类耦合问题
7. **[解.决]** 兄弟类们属性在同一维度相斥没必要都添加进子类使子类膨胀，也没必要各立山头
8. **[身.份]** 继承的替身
###问题起源
`InputStream inputStream = new BufferedInputStream(new FileInputStream(filePath));`
QAQ？看不懂咩～那别往下看了
**来自网络的释义**
```
装饰器模式 _主要用于为对象动态的添加功能_。比如说，最简单的 IO 使用可能是如下代码：

InputStream inputStream = new FileInputStream(filePath);

但是我们发现，这个代码 每次进行读取都会进行 一次 IO，IO是比较耗费时间的。 那么有没有什么方法降低 IO 次数呢，自然想到了缓冲。那么此时有两种做法：

定义一个 专门用于缓冲的文件读写 stream。
为 FileInputStream 类进行扩展，让它支持 缓冲。
肯定是第二种好，为什么呢。因为如果你按照1方案，
假如我要定义一个支持X功能，此时要定义一个 专门用于这个功能的stream，这个没有问题；
假如我又要定义一个支持Y功能，此时要定义一个专门用于这个功能的stream，这个也没有问题；

但是，假如我要定义一个 同时支持 X，Y功能，由于不能扩展，所以你必须再定义一个类，同时支持X，Y功能。这个就有问题了，问题就是重复了。

_但是，2 方案就能解决这个问题， 2方案中当你需要一个同时支持 X，Y 功能的类，那你动态扩展便是了，不用再新定义一个类，同时支持XY。_

这个就是装饰器模式的好处。


```
###UML

<center style="position:relative; left:80px; ">
![](https://raw.githubusercontent.com/srcmit/source/wiki/photo/decorator.png)
</center>

```
why要搞这么复杂呢，就是为了以后可以更好的扩展。像很多技术我们都知道它的原理，但是你看它们的源码，异常的捕获，各种检测，健壮性，扩展性，别人都考虑得很全，而且已经 code 下来了。' Talk is cheap, show me the code'

回到这个图，再对应着下面这个代码说：

InputStream inputStream = new BufferedInputStream(new FileInputStream(filePath));

Component：相当于 InputStream，Java IO 中很多的流都是继承于 InputStream。也就是一个基础组件。
ConcreteComponent：相当于 FileInputStream，一个基础组件的实现。
Decorator：所有具体装饰器类的抽象父类，在Java IO 包中 对应的是 FilterInputStream。
ConcreteDecorator：具体的装饰者。相当于BufferedInputStream。
```
###普通装饰器
####Python
```python
1.def decorator(func):
2.    def wrapper(*args, **keywords):
3.        #代码装饰
4.        return func(*args, **keywords)
5.    return wrapper
```
`注意line4 和 line5的return区别，一个返回的是对象，一个返回的是引用，所以无论外边再包几个高级函数`

####Java
```java
#定义
// The Window interface class
public interface Window {
	public void draw(); // Draws the Window
	public String getDescription(); // Returns a description of the Window
}
// implementation of a simple Window without any scrollbars
public class SimpleWindow implements Window {
	public void draw() {
		// Draw window
	}
	public String getDescription() {
		return "simple window";
	}
}
// abstract decorator class - note that it implements Window
public abstract class WindowDecorator implements Window {
    protected Window decoratedWindow; // the Window being decorated

    public WindowDecorator (Window decoratedWindow) {
        this.decoratedWindow = decoratedWindow;
    }
}


// The first concrete decorator which adds vertical scrollbar functionality
public class VerticalScrollBar extends WindowDecorator {
	public VerticalScrollBar(Window windowToBeDecorated) {
		super(windowToBeDecorated);
	}

	@Override
	public void draw() {
		super.draw();
		drawVerticalScrollBar();
	}

	private void drawVerticalScrollBar() {
		// Draw the vertical scrollbar
	}

	@Override
	public String getDescription() {
		return super.getDescription() + ", including vertical scrollbars";
	}
}


// The second concrete decorator which adds horizontal scrollbar functionality
public class HorizontalScrollBar extends WindowDecorator {
	public HorizontalScrollBar (Window windowToBeDecorated) {
		super(windowToBeDecorated);
	}

	@Override
	public void draw() {
		super.draw();
		drawHorizontalScrollBar();
	}

	private void drawHorizontalScrollBar() {
		// Draw the horizontal scrollbar
	}

	@Override
	public String getDescription() {
		return super.getDescription() + ", including horizontal scrollbars";
	}
}
```
```java
#测试
public class Main {
	// for print descriptions of the window subclasses
	static void printInfo(Window w) {
		System.out.println("description:"+w.getDescription());
	}
	public static void main(String[] args) {
		// original SimpleWindow
		SimpleWindow sw = new SimpleWindow();
		printInfo(sw);
		// HorizontalScrollBar  mixed Window
		HorizontalScrollBar hbw = new HorizontalScrollBar(sw);
		printInfo(hbw);
		// VerticalScrollBar mixed Window
		VerticalScrollBar vbw = new VerticalScrollBar(hbw);
		printInfo(vbw);
	}
}
```
```java
#结果
description:simple window
description:simple window, including horizontal scrollbars
description:simple window, including horizontal scrollbars, including vertical scrollbars
```
