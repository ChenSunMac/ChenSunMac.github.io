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

*Conversion Specification* 更通用的情况下可以有 *%m.pX* 的格式，其中X是字母，m 和 p 是数字

<br>
m 代表了字符的最小字段宽度 (总长度)
p 在整型数据里表示保留几位有效数字，在float里表示在小数点后保留几位有效数字

| Conversion Specification | Meaning |
|------------|-----------|
| %d         | integer         |
| %e         | exponential 的 float number         |
| %f          | float number       |
| %c         | single character       |
| %u          | unsigned integer        |

```c
int i = 40;
float x = 839.21;

printf("|%d|%5d|%-5d|%5.3d|\n", i , i, i, i); 
printf("|%10.3f|%10.3e|\n", x, x); 
```

所以
- %5.3d 的输出是 "  040"，前面补了两个空格
- %-5d 的输出是 "40   ",因为有负号，所有左对齐
- %10.3f 的输出是 "   839.210",前面三位空格以及后面的数字和小数点一共10个字符
- %10.3e 的输出是 " 8.392e+02", 保留小数点后三位有效数字，包含指数一共有9个字符，前面添加一个空格


#### scanf

```c
scanf(Format String, address1, address2, ...);  
```
和printf 一样，也是围绕着Format String 进行填空，但是注意这里是地址

```c
scanf("%d%f", &i, &x)
```

