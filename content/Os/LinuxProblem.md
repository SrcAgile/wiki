---
title: "指令[8086]"
layout:  page
collection: "[语法目]"
date: 2016-09-22 19:00:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑....</a>

---
- **今日音乐**
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=286999&auto=0&height=66"></iframe>

---
> 关注问题的发生形成有利于发展的思路

---

<b style="color:red"></b>


[TOC]

###Problem
- 文章推荐
    [IBM-软硬链接](https://www.ibm.com/developerworks/cn/linux/l-cn-hardandsymb-links/index.html)
- /usr/bin 与 /bin
    [故障实例](http://slayer.blog.51cto.com/4845839/1872014)
- 软链接与硬链接
- 理解 /proc/bus/input/devices信息对应/dev/input的信息
```c
    这个很好玩！可以直接用c去操作设备
    //#include <input.h> 引用内核头文件 linux/input.h
    //主要使用input.h的数据结构 input_event
    int fd;
    struct input_event ie;
    fd = open("/dev/input/event5", O_RDONLY);
    read(fd, &ie, sizeof(struct input_event));
    printf("type = %d  code = %d  value = %d\n",
            ie.type, ie.code, ie.value);
    close(fd);
```
- linux/unix 查看文件元数据
    `$ stat xxx 或 ls -i[查看inode]`
    ` AIX 系统 使用 istat`

-  dd if=/dev/zero of=mo.img bs=5120k count=1的意义
    `1. ls -lh mo.img `
    `2. mkfs -t ext4  -F ./mo.img`
    `3. mount -o loop ./mo.img /mnt `
    `4. df -iT /mnt/; du -sh /mnt/ `
- 查看inode值
    `// 查看磁盘分区 /dev/sda7 上的 inode 值`
    `# dumpe2fs -h /dev/sda7 | grep "Inode size"`
    `dumpe2fs 1.42 (29-Nov-2011)
      Inode size:              256 `
    `# tune2fs -l /dev/sda7 | grep "Inode size"`
    ` Inode size:              256`

- 硬链接目录环问题
