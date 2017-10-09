---
title: "[windows纲]api编程"
layout: page
collection: "[W-GUI]"
date: 2017-08-31 21:00:00
---

**文档状态：**<a style="color:red;background-color:gray">电脑维修中....</a>

---
- **今日音乐**

---
> 沉浸在思想的牢笼里.

---

[TOC]

windows 编程是基于消息驱动的,当消息到来时,应用程序就调用消息映射函数,但他是如何做到的？
    - [乱答]这个应该是把消息的调用方提升给了操作系统,操作系统驱动API？
    - 找到答案了，果然出现了控制权的移交
    `在程序执行时,系统会创建一个能控制他运行的数据结构,叫做pcb,pcb不断的查询消息队列`
    `一旦出现消息,就会根据相应的消息号去调用消息映射函数`

###Win图形设计与接口
####设备接口[图形]
- 图形设备接口[GDI]
- [K1]GDI提供了一系列函数与数据结构
    `[作用] 实现了程序开发者与硬件设备的隔离`
    `[1].  GDI管理了一个结构叫做设备环境，其中包含一些有关设备的信息`
- [K2]应用程序可以使用属性函数设置设备的<b>操作方式</b>和<b>当前的选择</b>
    `[操作方式] 文本和背景颜色/混色方式/映射方式`
    `[当前选择] 指绘图时使用哪个绘图对象`
- [K3] 映像模式储存在设备环境中    
- [FAQ1]GDI为什么能够实现相关函数？
    `[1]. windows下图形图像和文字设备驱动和硬件都支持这些函数，这些设备驱动都实现图形设备函数`
    `[2]. 所以绘图前要加载驱动程序`
- [FAQ2]什么是设备描述表？它是怎样被创建的
    `[1]. 简称DC，定义了一系列的图形对象及其属性的结构`
    `[2]. 应用程序使用设备环境函数创建DC来获得设备环境句柄，设备环境句柄指向设备相关信息，用户通过设备环境句柄来访问输出设备`

- [FAQ3]窗口与视口的关系
    `窗口依赖于逻辑坐标，可以是程序员想要的规定的所有尺度，由映像模式确定`
    `视口依赖于设备坐标，由输出设备决定（如像素点）`
    `窗口与视口的原点和范围都可以由程序员指定`

- [FAQ4]窗口实例与窗口句柄有什么关系
    `HWND与HINSTANCE`

###代码实现

```c
/*You should concern about the window 'class', it's not the concept of OOP
*if you are familar with c programing,it's easy for you to know the meaning
*of the 'class' , JUST A 'Struct' type....
*
*/

#include <windows.h>
LRESULT CALLBACK WndProc(HWND,UINT,WPARAM,LPARAM);
void Ipaint(HWND);
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPreInst,LPSTR lpszCmdLine,int nCmdShow){
	MSG msg;
	WNDCLASS wndclass;
	char classname[] ="painting....";

	wndclass.style =0;//defalut type
	wndclass.lpfnWndProc = WndProc;//msg resolve function bundle
	wndclass.cbClsExtra = 0; //no extends for window class
	wndclass.cbWndExtra=0;//not extends for window instance
	wndclass.hInstance = hInstance;// current instance handle
	wndclass.hIcon = LoadIcon(NULL,IDI_APPLICATION);
	wndclass.hCursor = LoadCursor(NULL, IDI_APPLICATION);
	wndclass.hbrBackground = (HBRUSH)GetStockObject(WHITE_BRUSH);
	wndclass.lpszClassName = classname;
	wndclass.lpszMenuName = NULL; // NO MENU
	if(!RegisterClass(&wndclass)){//register window class
		MessageBox(NULL,"register fault!!!","提示",MB_ICONERROR);
		return FALSE;

	}
	HWND hwnd;
	char lpszTitle[] = "绘图";
	hwnd= CreateWindow(
				classname,
				lpszTitle,
				WS_OVERLAPPEDWINDOW,// window type
				CW_USEDEFAULT,
				CW_USEDEFAULT,
				CW_USEDEFAULT,
				CW_USEDEFAULT,
				NULL,
				NULL,
				hInstance,
				NULL
		);
	ShowWindow(hwnd,nCmdShow);//nCmdShow---> the type of appearence
	UpdateWindow(hwnd);// paint the user area
	while(GetMessage(&msg,NULL,0,0)){
		TranslateMessage(&msg);
		DispatchMessage(&msg);
	}
	return msg.wParam;// additional message
}

LRESULT CALLBACK WndProc(HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam){
	switch(message){
		case WM_PAINT:
			Ipaint(hwnd);
			break;
		case WM_DESTROY:
			PostQuitMessage(0);
			break;
		default:
			return DefWindowProc(hwnd,message,wParam,lParam);

	}
	return 0;
}

void Ipaint(HWND hwnd){
	PAINTSTRUCT pt;
	HDC hdc;
	HPEN hpen;
	HBRUSH hbrushHatch;
	HBRUSH hbrush;
	hdc = BeginPaint(hwnd,&pt);// get device context handle
	hpen = (HPEN)GetStockObject(BLACK_PEN);// get sys's black brush
	SelectObject(hdc,hpen);// choose the hpen in hdc
	Rectangle(hdc,60,100,100,200);
	hbrush = CreateSolidBrush(RGB(0,255,0));
	SelectObject(hdc,hbrush);
	RoundRect(hdc,150,100,200,200,20,20);// paint round rect
	DeleteObject(hbrush);
	// create cross-shaped shade brush
	hbrushHatch = CreateHatchBrush(HS_CROSS,RGB(0,0,255));
	SelectObject(hdc,hbrushHatch);
	Ellipse(hdc,300,100,500,200);
	DeleteObject(hbrushHatch);
	EndPaint(hwnd,&pt);
}
```

###效果

<center style="position:relative;">
![](https://raw.githubusercontent.com/srcmit/source/wiki/photo/EX2.2.png)
</center>
