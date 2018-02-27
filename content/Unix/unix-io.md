---
title: "unix io"
layout: page
date: 2017-01-28 12:00:00
collection: "[io]"
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---

[TOC]



###concept
####fprintf
先看代码
```c
<!-- glibc-2.6\stdio-common\vfprintf.c -->
  _IO_flockfile (s);
……
  f = lead_str_end = __find_specwc ((const UCHAR_T *) format);
……
  /* Process whole format string.  */
  do
    {
……
    /* Write the following constant string.  */
    outstring (specs[nspecs_done].end_of_fmt,
           specs[nspecs_done].next_fmt
           - specs[nspecs_done].end_of_fmt);
  }
all_done:
  if (__builtin_expect (workstart != NULL, 0))
    free (workstart);
  /* Unlock the stream.  */
  _IO_funlockfile (s);
  _IO_cleanup_region_end (0);
```
>glibc的做法就是化整为零，在处理一个格式化输出的时候首先加锁，这里的“锁”位于FILE结构中，通过相同的FILE结构访问到相同的锁，共享相同的互斥。反过来说，就是FILE中使用的fd相同，只要它们封装在不同的FILE结构中，它们之间的写入也并不互斥。加上锁之后，glibc不再一次性把所有的字符格式化完成之后传递给write函数执行，而是只处理自己“基本职责”功能，就是扫描一个格式化字符串中的描述并逐个处理。对于上面的printf("%s%s", str1, str2)例子，vfpintf函数根据%s来讲str1的内容通过outstring传递给更底层的FILE结构缓冲管理层。这个缓冲的大小默认情况下在文件打开时通过stat函数获得文件所在的文件系统的block大小，FILE缓冲一个block作为自己的缓冲区大小

看上面的描述,似乎是对进程描述符表进行上锁,总而言之,反正是针对进程内部的信息进行进行上锁,那么这种行为显而易见,fprintf只能在多线程之间实现,多进程之间完全无效,那么多进程情况应该使用对inode上锁的.
fprintf操作的是clib中一块缓冲区,这个缓冲回去结构中包括fd指向系统描述符表,这个前提是fopen打开文件,对于单纯的open打开只会返回fd(文件描述符),并不创建clib中的缓存区.


####write
 <!-- mutex保证了单次的write调用不会存在数据不一致性,但这并不意味着write是原子性操作,写过程中仍可能存在被中断的可能性. -->
```c
 常规文件write调用实现关系图:
 write--->(VFS 入口)sys_write-->>vfs_write->[file->f_op.write(不同文件不同实现方式)]-->>>(文件系统层入口)do_sync_write-->>generic_file_aio_write(根据filp->f_flags判断是否cache)---->cache层-->mpage_submit_bio->(通用块层入口将bio传送到IO调度层进行处理)generic_make_request--->(io调度层)bio合并并选择适当的调度算法形成请求队列--->(设备驱动层)request函数对请求队列中每个bio进行分别处理，根据bio中的信息向磁盘控制器发送命令。处理完成后，调用完成函数end_bio以通知上层完成.

 mutex_lock(&inode->i_mutex); 
 ret = __generic_file_aio_write_nolock(iocb, iov, nr_segs, &iocb->ki_pos); 
 mutex_unlock(&inode->i_mutex);

```


####EOF
```c
最初的理解认为EOF只是每个文件放在文件结尾的字符，占有一定的存储空间，后来学习了设备文件才发现大错特错，最开始的时候EOF只是一个宏，定义为-1，只是为了返回一个read调用的错误值，后来当操作终端设备文件时发现终端文件不能仅仅根据终端没有数据说明文件结束，所以使用了特殊的输入字符(CTRL+D[LINUX]&CTRL+Z(WIN))表示终端文件输入结束，即说这种特殊字符为EOF字符。所以说在经验传播过程中存在了个人语言上的歧义性，致使经验不够严谨，所以说一个称职的工程人员应该兼具工程师的直觉和数学家的严谨。
```
###Question
1. if u want to avoid overwriting an existing file,you can use `(O_CREAT|0_EXCL)`
首先应该知道，当文件存在时，使用open("filename",O_CREATE,0666)是会重写一个文件的，所以引入了(O_CREATE|0_EXCL)，但是有的人认为多此一举，可以使用
```c
if(access(fileName,R_OK)==-1)#如果文件不存在
    open(fileName,O_CREATE|0_RDWR,0666);
```
但是在操作系统课程老师讲过，系统调用是具有原子性的，即执行开始到结束不会切换进程，所以
`open("filename",O_CREATE|O_RDWR|O_EXCL,0666)`多了一个好处，当多道环境下只允许一个进程去打开文件，例如socket，此时便用原语实现了。

###messy
read调用对普通文件读取后没有字符返回eof，但是特殊设备文件却会阻塞等待数据？

###unix io
####ext3文件写操作
IO会走到sys_write的地方，然后执行file->f_op->write方法。对于EXT3 ，该方法注册的是do_sync_write。Do_sync_write的实现如下：
```c
ssize_t do_sync_write(struct file *filp, const char __user *buf, size_t len, loff_t *ppos)
{
  struct iovec iov = { .iov_base = (void __user *)buf, .iov_len = len };
  struct kiocb kiocb;
  ssize_t ret;
  init_sync_kiocb(&kiocb, filp);
  kiocb.ki_pos = *ppos;
  kiocb.ki_left = len;
  for (;;) {
    ret = filp->f_op->aio_write(&kiocb, &iov, 1, kiocb.ki_pos);
    if (ret != -EIOCBRETRY)
      break;
    wait_on_retry_sync_kiocb(&kiocb);
  }
if (-EIOCBQUEUED == ret)
ret = wait_on_sync_kiocb(&kiocb);
*ppos = kiocb.ki_pos;
return ret;
}
```
对EXT3文件写操作主要考虑两种情况
1. DIRECT IO方式(直接绕过缓存处理机制)
2. page cache的写方式(性能会比Direct IO高出不少
<b>Radix Tree[PAT位树(Patricia Trie or crit bit tree)]</b>
>对于每一个EXT3文件，都会在内存中维护一棵Radix tree，这棵radix tree就是用来管理来page cache页的.当IO想往磁盘上写入的时候，EXT3会查找其对应的radix tree，看是否已经存在与写入地址相匹配的page页，如果存在那么直接将数据合并到这个page 页中，并且结束一次IO过程，直接结束应用层请求。如果被访问的地址还没有对应的page页，那么需要为访问的地址空间分配page页，并且从磁盘上加载数据到page页内存，最后将这个page页加入到radix tree中。


