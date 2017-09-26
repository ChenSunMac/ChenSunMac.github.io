---
layout: post
title: CSC209 String and Structs
date: 2017-09-26
tags: C 
categories: C
---
### Basic Types

#### 整型
|  **NAME** | **bits**  | RANGE  |
|---|---|---|
| unsigned short int  | 16  | 0~65535  |
|  short int          | 16  | -32768~32768  |
|int                  | 32 |-2147483648~+2147483647   |
| unsigned int        | 32 | 0~+4294967296  |
| long int            | 64 | -2147483648~+2147483647  |
|  unsigned long int  | 64 | 0~+4294967296  |

之所以long int 和int有区别是因为历史原因，以前的int是16位的

数字的书写形式
- 不已0打头的数字是10进制
- 0打头的数字是8进制
- 0x打头的是16进制

#### 浮点型
根据机器往往不同，一般以IEEE标准为准
- float 单精度 (32 bit)
- double 双精度 (64 bit)
- long double 扩展双精度 (128 bit)

#### Escape Sequence 转义字符
|  **NAME** | **ESCAPE SEQUENCE**  | **ESCAPE in HEX** |
|---|---|---|
| Newline  | \n  | \x0A  |
| Tab  | \t  | \x09  |
| BackSlash  | \\\  | \x5C  |
| Tab  | \t  | \x09  |

小写转换成大写
```c
if ('a' <= ch && ch <= 'z')
    ch = ch - 'a' + 'A';
```
### String Literal
String Literal 是双引号括起来的字符序列，其中也可以放转义字符,
<br>
C 语言把String Literal 当做 char array 来处理，他会为长度为n的String Literal 分配 n+1的内存空间，其中会在最后添加一个额外的字符，即空字符"\0"

NOTE: "" = \0 是空字符， 而0的ASCII为48


### String Array
C语言允许省略行数，但是一般却要说明列数
```c
char names[][8] = {"WangSiTu","JiangHaHa","XiBaoZi","WenKuKu"};
```
这样填不满一行的会用空字符来填补，太浪费空间
```c
char *names[] = {"WangSiTu","JiangHaHa","XiBaoZi","WenKuKu"};
```

### Struct
