---
title: "unix高级环境编程"
layout: page
collection: "[阅读纲]"
date: 2017-04-28 20:00:00
---

**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---
[TOC]

### chapter 01
OverView:

#### errorno

```c
The errors defined in <errno.h> can be divided into two categories: fatal and nonfatal.
[1]A fatal error has no recovery action. The best we can do is print an error message on the user ’s screen or to a log file, and then exit.
Nonfatal errors, on the other hand, can sometimes be dealt with more robustly.
Nonfatal Errors
    |___resource shortage(EAGAIN, ENFILE, ENOBUFS, ENOLCK,ENOSPC,EWOULDBLOCK)
[2]The typical recovery action for a resource-related nonfatal error is to delay and retry later.
```
#### User id &Grounp id

```c
[key] We call the user whose user ID is 0 either root or the superuser.

[FAQ]
_______________________________________________________________________________
Q: what is the function of /etc/passwd and /etc/group
A: Map. /etc/group maps group names into numeric group IDs./etc/passwd maps user names into numeric user IDs.
_______________________________________________________________________________
Q: why  should we need user/grounp id?
A: the file system stores both the user ID and the group ID of a
file’s owner. Storing both of these values requires only four bytes, assuming that each is stored as a two-byte integer. If the full ASCII login name and group name were used instead, additional disk space would be required. In addition, comparing strings during permission checks is more expensive than comparing integers.
_______________________________________________________________________________
Q: whereas why does we just use the user id to instead user id+user name?
A: Users, however, work better with names than with numbers, so the password file maintains the mapping between login names and user IDs, and the group file provides the mapping between group names and group IDs.
```
#### signal

```c
Many conditions generate signals. 
[HardWare]
    |___Two terminal keys
    |        |_____interrupt key(DELETE key or Control-C)
    |        |_____quit key(Control-backslash)
 
[SoftWare]
    |___calling the kill function
Naturally, there are [limitations]: we have to be the owner of the other
process (or the superuser) to be able to send it a signal.
```

---

### chapter 2
#### unix Standardization
 `ISO C`
 `IEEE POSIX`

#### LIMIT
- TYPE
    `[1]. Compile-time limits`
    `[2]. Runtime limits`


```c
compile-time limits can be defined in header that any program can include at compile time. but runtime limits require the process to call a function to obtain the limit''s value.
```
- terms
    `one’s-complement arithmetic[汉语:关于1的补数亦称之为反码]`
    `two’s-complement arithmetic[汉语:关于2的补数亦称之为补码]`
To solve the problem which writed on the page 36 in the `UNIX Adanvanced Programming` ,three types of limits are provide:
```c
1. compile-time limits(headers)
2. runtime limits not associated with a file or directory(the sysconf function)
3. runtime limits that are associated with a  file or directory(the pathconf and fpathconf function)
```


#### Primitive System Data Types

> 起因:certain C data types have been associated with certain UNIX system
variables.

> These data types are defined in the headers with the C typedef facility. Most end in _t.

such as clock_t,pid_t

### [chapter3]File IO
unbuffered I/O与第五章的standard I/O routines区分开来.
[标准]:不属于ISOC,但属于POSIX.1和SUS

```
TOCTTOU
errors in the file system namespace generally deal with attempts to subvert file system
permissions by tricking a privileged program into either reducing permissions on a
privileged file or modifying a privileged file to open up a security hole.
```