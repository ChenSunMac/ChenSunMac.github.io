---
layout: post
title: Shell Scripting 01
date: 2017-08-09
tag: BELL, Shell, Linux
categories:
- Linux & C
comments: true
---

## Kernel and Shell
Kernel is heart of Linux Os. It manages resource of Linux Os.

<img src="/images/posts/Shell/shell_kernel.png" height="200" width="600"> 


In early days of computing, instruction are provided using binary language, which is difficult for all of us, to read and write. So in Os there is special program called Shell. Shell accepts your instruction or commands in English (mostly) and if its a valid command, it is pass to kernel.

One of the famous shell available is BASH ( Bourne-Again SHell ).

In MS-DOS, Shell name is COMMAND.COM which is also used for same purpose, but it's not as powerful as our Linux Shells are! Also, the windows explorer is a shell with graphical interface.

To find current shell:
```shell
$ echo $SHELL
```

## Why Shell Script

- Shell script can take input from user, file and output them on screen.
- Save lots of time.
- To automate some task of day today life.
- System Administration part can be also automated.

For example:
```sh
#!/bin/sh                  # 指定脚本解释器，这里是用/bin/sh做解释器的
cd ~                       # 切换到当前用户的home目录
mkdir shell_tut            # 创建一个目录shell_tut
cd shell_tut               # 切换到shell_tut目录
                
for ((i=0; i<10; i++)); do  # 循环条件，一共循环10次
    touch test_$i.txt       # 创建一个test_1…10.txt文件
done                        # 循环体结束
```

cd, mkdir, touch都是系统自带的程序，一般在/bin或者/usr/bin目录下。for, do, done是sh脚本语言的关键字。
















