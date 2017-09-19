---
layout: post
title: CSC209 C 语言初步以及Input Output
date: 2017-09-19
tags: Linux C CSC209
categories: C
---

### C - Compile and Link

对于C语言，要将其转化为机器能执行的形式，一般来说需要三步
1. Pre-Process 预处理
预处理器会执行以\# 开头的命令(directive),比如script里面的\#\!

2. Compile 编译
翻译成object code (.o) 但是并不能运行

3. Link 链接
目标代码和附加代码整合在一起，形成.exe

### Formated Input and Output
*printf* 和 *scanf* 当中的 *f* 都是 格式化format的意思

#### printf
Printf 是用来显示格式串（Format String）的内容，并且在指定位置插入可能的值的函数。
```c
printf(Format String, exp1, exp2, ...);  
```

Format String 格式串中包含了 普通字符 和 转换说明 *Conversion Specification* （以%开头， 后面的信息指定了把数值从二进制转换成字符的方法） 

```c
int i = 10;
float j = 11.11;

printf("i = %d, j = %f \n", i, j);   /* CORRECT */
printf("%f, %d", i, j);  /* WRONG - output meanings less*/
```



