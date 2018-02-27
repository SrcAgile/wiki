---
title: "sublime中文输入"
layout: page
date: 2016-09-21 02:00:00
collection: "[编辑器目]"
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---
> 我打死都不会用sublime!
> emmmm!
> 好用

---
- **今日音乐**
```
暂无评论
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=386830&auto=0&height=66"></iframe>

---


<center style="position:relative; ">
</center>

[TOC]
###question
1. 重装系统后安装sublime发现不能使用，仔细检查缺乏一些共享库文件，需要自己编译一下

###work
1. 安装Dev工具
```bash
sudo apt-get install build-essential
sudo apt-get install libgtk2.0-dev #含gtk/gtk.h
```
2. coding
```c
/*
 * name:  sublime-imfix.c
 * make:  gcc -shared -o libsublime-imfix.so sublime-imfix.c `pkg-config --libs --cflags gtk+-2.0` -fPIC
*/
#include <gtk/gtk.h>
#include <gdk/gdkx.h>
typedef GdkSegment GdkRegionBox;
struct _GdkRegion
{
  long size;
  long numRects;
  GdkRegionBox *rects;
  GdkRegionBox extents;
};
GtkIMContext *local_context;
void
gdk_region_get_clipbox (const GdkRegion *region,
            GdkRectangle    *rectangle)
{
  g_return_if_fail (region != NULL);
  g_return_if_fail (rectangle != NULL);
  rectangle->x = region->extents.x1;
  rectangle->y = region->extents.y1;
  rectangle->width = region->extents.x2 - region->extents.x1;
  rectangle->height = region->extents.y2 - region->extents.y1;
  GdkRectangle rect;
  rect.x = rectangle->x;
  rect.y = rectangle->y;
  rect.width = 0;
  rect.height = rectangle->height;
  //The caret width is 2;
  //Maybe sometimes we will make a mistake, but for most of the time, it should be the caret.
  if(rectangle->width == 2 && GTK_IS_IM_CONTEXT(local_context)) {
        gtk_im_context_set_cursor_location(local_context, rectangle);
  }
}
 
//this is needed, for example, if you input something in file dialog and return back the edit area
//context will lost, so here we set it again.
 
static GdkFilterReturn event_filter (GdkXEvent *xevent, GdkEvent *event, gpointer im_context)
{
    XEvent *xev = (XEvent *)xevent;
    if(xev->type == KeyRelease && GTK_IS_IM_CONTEXT(im_context)) {
       GdkWindow * win = g_object_get_data(G_OBJECT(im_context),"window");
       if(GDK_IS_WINDOW(win))
         gtk_im_context_set_client_window(im_context, win);
    }
    return GDK_FILTER_CONTINUE;
}
 
void gtk_im_context_set_client_window (GtkIMContext *context,
          GdkWindow    *window)
{
  GtkIMContextClass *klass;
  g_return_if_fail (GTK_IS_IM_CONTEXT (context));
  klass = GTK_IM_CONTEXT_GET_CLASS (context);
  if (klass->set_client_window)
    klass->set_client_window (context, window);
 
  if(!GDK_IS_WINDOW (window))
    return;
  g_object_set_data(G_OBJECT(context),"window",window);
  int width = gdk_window_get_width(window);
  int height = gdk_window_get_height(window);
  if(width != 0 && height !=0) {
    gtk_im_context_focus_in(context);
    local_context = context;
  }
  gdk_window_add_filter (window, event_filter, context);
}
```
3. 修改/usr/share/applications/sublime_text.desktop文件
```bash
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text %F"
```
4. 重启打开即可

###出现问题
1. 无法编译
```
#直接下载已经编译好的链接库
git clone https://github.com/MagicalBird/sublime-text-imfix.git
```

###激活序列
```c
—– BEGIN LICENSE —–
Morin
2 User License
EA7E-924018
184B9FDB 02612F57 33B15E69 BBC567F1
E20FA231 C077EA95 CC14B48B 71DD2536
E209843A 94D13692 03AC2FAA 895B688D
B8F4A0E6 FDC15964 A5573FD7 6405ED1E
6F205469 7F34C69D 3D36E475 52AF6A5B
DFD15C31 85BA64EF F95DD592 4B42C314
AC655762 C0F0F5A1 018824E4 17C56E16
AC5AA84C 034F7A53 2C9A801B 8AED239F
—— END LICENSE ——
```
```c
—– BEGIN LICENSE —–
TwitterInc
200 User License
EA7E-890007
1D77F72E 390CDD93 4DCBA022 FAF60790
61AA12C0 A37081C5 D0316412 4584D136
94D7F7D4 95BC8C1C 527DA828 560BB037
D1EDDD8C AE7B379F 50C9D69D B35179EF
2FE898C4 8E4277A8 555CE714 E1FB0E43
D5D52613 C3D12E98 BC49967F 7652EED2
9D2D2E61 67610860 6D338B72 5CF95C69
E36B85CC 84991F19 7575D828 470A92AB
—— END LICENSE ——
```