---
title: "c++命名空间"
layout: page
collection: "[cpp]"
date: 2017-01-17 01:00:00
---
**文档状态：**<a style="color:red;background-color:gray">编辑中....</a>

---
> .

---
- **今日音乐**
```
暂无评论
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=386830&auto=0&height=66"></iframe>

---
[TOC]
###背景知识
- c/c++从预处理到编译链接的全过程熟悉
- 了解重载和命名空间奇技淫巧在编译器端的实现

###开始
1. code
```cpp
#include <iostream>
using std::cout;
namespace nameA{
    void sayhello(){
        cout<<"hello i am nameA";
    }
}
namespace nameB{
    void sayhello(){
        cout<<"hello i am nameB";
    }
}
using namespace nameB;
int main(){
    sayhello();
    return 0;
}
```
2. 查看符号表
```bash
Symbol table '.symtab' contains 22 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS namespace.cpp
     2: 0000000000000000     0 SECTION LOCAL  DEFAULT    1 
     3: 0000000000000000     0 SECTION LOCAL  DEFAULT    3 
     4: 0000000000000000     0 SECTION LOCAL  DEFAULT    4 
     5: 0000000000000000     1 OBJECT  LOCAL  DEFAULT    4 _ZStL8__ioinit
     6: 0000000000000000     0 SECTION LOCAL  DEFAULT    5 
     7: 000000000000003c    62 FUNC    LOCAL  DEFAULT    1 _Z41__static_initializati
     8: 000000000000007a    21 FUNC    LOCAL  DEFAULT    1 _GLOBAL__sub_I__ZN5nameA8
     9: 0000000000000000     0 SECTION LOCAL  DEFAULT    6 
    10: 0000000000000000     0 SECTION LOCAL  DEFAULT    9 
    11: 0000000000000000     0 SECTION LOCAL  DEFAULT   10 
    12: 0000000000000000     0 SECTION LOCAL  DEFAULT    8 
    13: 0000000000000000    22 FUNC    GLOBAL DEFAULT    1 _ZN5nameA8sayhelloEv
    14: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND _ZSt4cout
    15: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND _ZStlsISt11char_traitsIcE
    16: 0000000000000016    22 FUNC    GLOBAL DEFAULT    1 _ZN5nameB8sayhelloEv
    17: 000000000000002c    16 FUNC    GLOBAL DEFAULT    1 main
    18: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND _ZNSt8ios_base4InitC1Ev
    19: 0000000000000000     0 NOTYPE  GLOBAL HIDDEN   UND __dso_handle
    20: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND _ZNSt8ios_base4InitD1Ev
    21: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND __cxa_atexit
```
we can see the entry which named `_ZN5nameA8sayhelloEv`&`_ZN5nameB8sayhelloEv`,so there is a door which could help us to explore the rule of namespace.
我们反汇编一下
```asm

namespace.o:     file format elf64-x86-64


Disassembly of section .text:

0000000000000000 <_ZN5nameA8sayhelloEv>:
   0:   55                      push   %rbp
   1:   48 89 e5                mov    %rsp,%rbp
   4:   be 00 00 00 00          mov    $0x0,%esi
   9:   bf 00 00 00 00          mov    $0x0,%edi
   e:   e8 00 00 00 00          callq  13 <_ZN5nameA8sayhelloEv+0x13>
  13:   90                      nop
  14:   5d                      pop    %rbp
  15:   c3                      retq   

0000000000000016 <_ZN5nameB8sayhelloEv>:
  16:   55                      push   %rbp
  17:   48 89 e5                mov    %rsp,%rbp
  1a:   be 00 00 00 00          mov    $0x0,%esi
  1f:   bf 00 00 00 00          mov    $0x0,%edi
  24:   e8 00 00 00 00          callq  29 <_ZN5nameB8sayhelloEv+0x13>
  29:   90                      nop
  2a:   5d                      pop    %rbp
  2b:   c3                      retq   

000000000000002c <main>:
  2c:   55                      push   %rbp
  2d:   48 89 e5                mov    %rsp,%rbp
  30:   e8 00 00 00 00          callq  35 <main+0x9>
  35:   b8 00 00 00 00          mov    $0x0,%eax
  3a:   5d                      pop    %rbp
  3b:   c3                      retq   

000000000000003c <_Z41__static_initialization_and_destruction_0ii>:
  3c:   55                      push   %rbp
  3d:   48 89 e5                mov    %rsp,%rbp
  40:   48 83 ec 10             sub    $0x10,%rsp
  44:   89 7d fc                mov    %edi,-0x4(%rbp)
  47:   89 75 f8                mov    %esi,-0x8(%rbp)
  4a:   83 7d fc 01             cmpl   $0x1,-0x4(%rbp)
  4e:   75 27                   jne    77 <_Z41__static_initialization_and_destruction_0ii+0x3b>
  50:   81 7d f8 ff ff 00 00    cmpl   $0xffff,-0x8(%rbp)
  57:   75 1e                   jne    77 <_Z41__static_initialization_and_destruction_0ii+0x3b>
  59:   bf 00 00 00 00          mov    $0x0,%edi
  5e:   e8 00 00 00 00          callq  63 <_Z41__static_initialization_and_destruction_0ii+0x27>
  63:   ba 00 00 00 00          mov    $0x0,%edx
  68:   be 00 00 00 00          mov    $0x0,%esi
  6d:   bf 00 00 00 00          mov    $0x0,%edi
  72:   e8 00 00 00 00          callq  77 <_Z41__static_initialization_and_destruction_0ii+0x3b>
  77:   90                      nop
  78:   c9                      leaveq 
  79:   c3                      retq   

000000000000007a <_GLOBAL__sub_I__ZN5nameA8sayhelloEv>:
  7a:   55                      push   %rbp
  7b:   48 89 e5                mov    %rsp,%rbp
  7e:   be ff ff 00 00          mov    $0xffff,%esi
  83:   bf 01 00 00 00          mov    $0x1,%edi
  88:   e8 af ff ff ff          callq  3c <_Z41__static_initialization_and_destruction_0ii>
  8d:   5d                      pop    %rbp
  8e:   c3                      retq   

```
caution: e,29,30全都是e8 00 00 00 00，那么链接一下
贴一部分代码
```c
000000000040078c <_ZN5nameB8sayhelloEv>:
  40078c:   55                      push   %rbp
  40078d:   48 89 e5                mov    %rsp,%rbp
  400790:   be a5 08 40 00          mov    $0x4008a5,%esi
  400795:   bf 60 10 60 00          mov    $0x601060,%edi
  40079a:   e8 c1 fe ff ff          callq  400660 <_ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc@plt>
  40079f:   90                      nop
  4007a0:   5d                      pop    %rbp
  4007a1:   c3                      retq   

00000000004007a2 <main>:
  4007a2:   55                      push   %rbp
  4007a3:   48 89 e5                mov    %rsp,%rbp
  4007a6:   e8 e1 ff ff ff          callq  40078c <_ZN5nameB8sayhelloEv>
  4007ab:   b8 00 00 00 00          mov    $0x0,%eax
  4007b0:   5d                      pop    %rbp
  4007b1:   c3                      retq  
```
可知编译器记录了当前的namespace对函数名进行扩展后再寻找，看起来namespace是从名字角度上对变量进行分类，[具体规则](http://blog.csdn.net/u013230511/article/details/77427821)

###tools
- readelf
- objdump

###ref
[1]. [编译原理](http://blog.csdn.net/u013230511/article/details/77427821)
