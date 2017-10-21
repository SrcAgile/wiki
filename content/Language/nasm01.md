---
title: "语言[A]开发"
layout: page
collection: "[二代目]"
date: 2017-10-14 11:25:49
---

**文档状态：**<a style="color:red;background-color:gray">思索中....</a>

---
- **今日音乐**
今日无音乐.
---
> waiting

---

##intro

1. 开发环境:NASM+ld
2. 语法使用:Intel语法
3. 平台环境:X86

---
###NASM简介
---
1. 语法：nasm -f \<format> \<filename> [-o \<output>] #[更多细节](https://baike.baidu.com/item/nasm/10798233?fr=aladdin)
    `nasm -f elf hello.asm 汇编成'ELF'格式`
    `nasm -f bin myfile.asm -o myfile.com 汇编成纯二进制格式的文件`
2.

---
###ld简介
---

```python
$ ld -s -o f f.o
  ld: i386 architecture of input file 'f.o' is incompatible with i386:x86-64 output
#原因很简单，ld默认以x86-64输出，所以得知
#-m EMULATION         Set emulation
#Supported emulations: elf_x86_64 elf32_x86_64 elf_i386 i386linux elf_l1om elf_k1om i386pep i386pe
#所以我们修改命令
$ ld -m elf_i386 -o f f.o
$ ./f
#ok successful!

```
