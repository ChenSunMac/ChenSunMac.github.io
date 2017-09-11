---
layout: post
title: Linux 内核分析
date: 2017-09-11
tags: Linux Kernel C
categories: Linux
---

### Computer Structure

CPU的体系架构从数据和指令的位置上分类可以分为 *Von Neumann architecture* (存储程序计算机) 以及 *Harvard architecture*.

这两者的区分在于 *Harvard architecture* 将指令存储和数据存储分开，CPU首先到Instruction Memory中读取Instruction，Decode to get data address，then get the data from memory，并进行下一步的操作（通常是执行）。程序指令存储和数据存储分开，可以使指令和数据有不同的数据宽度.

However, the difference between these two blurred these days by introducing cache.

#### RISC and CISC

Here we introduce additional point on CPU structure:

- CISC (复杂指令集)
    + x86, x64...
- RISC (精简指令集)
    + MIPS, ARM...

### Von Neumann architecture

从硬件角度来看，CPU和内存之间由总线架接起来，通过CPU内部的 *IP* 去访问内存中的代码段(Code Segment).

从软件的角度来看，
Memory holds instructions and data, CPU interpreter of instructions
```C
for (;;){
    next instruction
}

```


#### X86 EIP
Instruction Pointer 相当于 MIPS里面的PC， 作为指定当前指令以及数据的用处。
