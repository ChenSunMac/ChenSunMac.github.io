---
layout: post
title: 读书笔记 - TLPI - User, Permission and IO Redirection
date: 2017-08-22
tag: Unix, Shell, Linux, File System, Process, Thread
comments: true
---

### User, Group, SuperUser
<br>
In Linux, there are two types of users: system users and regular users. Traditionally, system users are used to run non-interactive or background processes on a system, while regular users used for logging in and running processes interactively.
<br>
注:unix系统本身是针对实验室里老贵的机器的，那么就像实验室里其他死贵的机器一样，一开始就是很多人一起用的，现在家用机这个功能基本没啥用，但是在公司里区分员工权限还是比较靠谱的。
<br>
all users information are contained in the */etc/passwd* file. One can print the passwd file with the following command
```sh
cat /etc/passwd    #catenate reads data from files, and outputs their contents standard output
```
<br>

***Groups*** are collections of zero or more users. A user belongs to a default group, and can also be a member of any of the other groups on a server.

```sh
cat /etc/group    #catenate reads data from files, and outputs their contents standard output
```
Or simply using *groups* command
```sh
groups    #List a user’s group memberships.
groups JoJoGroup #list user under this JoJoGroup
```
<br>
***SuperUser***
<br>
**sudo** ("*SUperuser DO*") allows a user with proper permissions to execute a command as another user, such as the superuser.

**su** ("*Switch User*") Change the current user ID to that of the superuser, or another user.
<br>

Syntax:
su [options] [username]
<br>

### Viewing Ownership and Permissions

```sh
ls -l      #list segment using long listing option
```
A example screen shot of the ls -l output
<img src="https:#assets.digitalocean.com/articles/linux_basics/ls-l.png"> </img>
<br>

#### What is Mode?
<img src="https:#assets.digitalocean.com/articles/linux_basics/mode.png"> </img>

**File Type**

2 Basic file types in linux: *Normal* and *Special*.

**Normal files** are just plain files that can contain data.
Identified by files with a hyphen **(-)**

**Special files** can be identified by files that have a non-hyphen character,

For example, a Directory, which is the most common kind of special file, is identified by the **d** character


<br>

**Permission Class**

- **r**, Read
- **w**, Write
- **x**, Execute  

<br>

One can use number to present the permission:

| Number       | Permission           | Ref  |
| ------------- |:-------------:| -----:|
| 0      | No permission | --- |
| 1     | Execute permission     |  --x |
| 2 |Write permission    |   -w- |
| 3     |Execute and write permission: 1 (execute) + 2 (write) = 3 | -wx |
| 4     | Read permission    |  r-- |
| 5 |Read and execute permission: 4 (read) + 1 (execute) = 5    |   r-x |
| 6     | Read and write permission: 4 (read) + 2 (write) = 6   |  rw- |
| 7 |All permissions: 4 (read) + 2 (write) + 1 (execute) = 7   |   rwx |

<br>
### How to change mode, ownership, group

**chmod** (CHange MODe) change the permissions of files or directories.
Syntax：
chmod [OPTION]... MODE[,MODE]... FILE...
<br>
```sh
chmod u=rwx,g=rx,o=r myfile 
```
Equivalent command using octal 八进制(然而只是因为有8个而已) permission
```sh
chmod 754 myfile
```

[Mode] has the following format:

[ugoa...][[+-=][perms...]...]

A combination of the letters u, g, o, and a controls which users' access to the file will be changed: the user who owns it (u), other users in the file's group (g), other users not in the file's group (o), or all users (a).

"+" adding permission, "-" remove permission, "=" add or remove mentioned permission

```sh
chmod u+rwx myfile         #user owns myfile add rwx mode to permission
chmod go-x                 #group and other user remove execute permission
chmod a=x                  #all user add execute permission to the file 
```


<br>

**chown** (CHange OWNer)  changes the owner and owning group of files.
**chgrp** (CHange GRouP)  Changes group ownership of a file or files.

```sh
chown Chain textfile.txt  #Set the owner of textfile.txt to user Chain.
chown -R Chain /files/work  # Recursively grant ownership of the directory /files/work, and all files and subdirectories, to user Chain.
chgrp JoJoGroup textfile.txt # Change the owning group of the textfile to the group named JoJoGroup.
```


### I/O Redirection
我对UNIX I/O stream 出现的理解是工程师为了在硬件上编程方便debug出现的产物。你可以想象对着芯片和一堆硬件电路debug的场景，拿着电表到处去测量值，而且每次进行输入和输出都很麻烦，所以出现了I/O，但是又面临标准化的问题，所以有了这个东东。

Input and output in the Linux environment is distributed across three *streams*. These streams are:
- standard input (stdin) 0；
- standard output (stdout) 1；
- standard error (stderr) 2

那么什么是stream呢，我查到的一个解释如下:
A Linux *stream* is data traveling in a Linux shell from one process to another through a *pipe*, or from one file to another as a *redirect*.

也就是说Stream就是数据流，类比水流，可以是在linux shell里的进程之间通过**pipe**传输，也可以是从文件之间互相传输，叫做redirect

pipe和process的概念在ch1-5的那篇里面提了一点，在这里我觉得wiki上的这张图对pipe解释的很清楚，显示了terminal上一个包含三个程序的管道

<img src="https:#en.wikipedia.org/wiki/Pipeline_(Unix)#/media/File:Pipeline.svg"> </img>

Pipe 是一个由标准输入输出链接起来的进程集合，所以每一个进程的输出（stdout）被直接作为下一个进程的输入（stdin）。 

那么我们绕回来还是得去看看这些stdin 和stdout 是个啥

#### Standard Input

**stdin** carries data from user/program to program.


Standard input is terminated by reaching EOF (end-of-file)

To see standard input in action, run the cat program. Cat stands for concatenate, which means to link or combine something. It is commonly used to combine the contents of two files. When run on its own, cat opens a looping prompt.

```sh
cat
```
After opening cat, type a series of numbers as it is running.
```
1
2
3
ctrl-d
```
When you type a number and press enter, you are sending standard input to the running cat program, which is expecting said input. In turn, the cat program is sending your input back to the terminal display as standard output.
<br>
EOF can be input by the user by pressing **ctrl-d**. After the cat program receives EOF, it stops.


<br>
**stdout** writes the data that is generated by a program.

```sh
echo Sent to terminal through stdout    #displays any argument that is passed to it on the terminal
```
<br>
**stderr**  writes the errors generated by a program that has failed at some point in its execution.

When run without an argument, ls lists the contents within the current directory. If ls is run with a directory as an argument, it will list the contents of the provided directory.
```sh
ls %
```

Since % is not an existing directory, this will send the following text to standard error:
```
ls: cannot access %: No such file or directory
```


### Stream Redirection
上面的stream默认都是从程序导入终端，或者是从终端导入程序（默认用作debug），那么如果想把stream导入文件里，就需要用 **Redirection** 了

<br>

**Overwrite**

- ">", stdout
- "<", stdin
- "2>", stderr

```sh
cat > writeToMe.txt
a
b
c
ctrl-d
```
Here, cat is being used to write to a file, which is created as a result of the loop.


```
cat writeToMe.txt  #View the contents of writeToMe.txt using cat:
```
we can see a\n b\n c there

<br>

**Append**
- ">>", stdout
- "<<", stdin
- "2>>", stderr


### Pipes

redirect 能够让我们把stdin out err的数据流导入 文件，那么如何让stream从program到program呢？

这就用到Pipe了，
The Linux pipe is represented by a vertical bar.
```sh
*|*
```

An example of a command using a pipe:
```sh
ls | less
```

This takes the output of ls, which displays the contents of your current directory, and pipes it to the less program. less displays the data sent to it one line at a time.


*Note*: 
Though the functionality of the pipe may appear to be similar to that of > and >> (standard output redirect), the distinction is that pipes redirect data from one command to another, while > and >> are used to redirect exclusively to files.

### Filters

Filters are commands that alter piped redirection and output. Note that filter commands are also standard Linux commands that can be used without pipes.

- find 
    + return files with filenames that match the argument passed to find
```sh
# locate and print a list of every file in and beneath the current directory
find      
# locate and print a list of every file in the current directory
find.
# locate and print a list of every file in the jojo1 and jojo2
find . /home/jojo1 /home/jojo2

# Locate and print all files and directories in and beneath either of the directories /usr/bin which contains the text "zip" anywhere in the file or directory name.
find /usr/bin -name '*zip*'

# locate and print a complete list of all files in and beneath the directory /home/jeff/fruit, pipe this listing to grep, which filters out any file name which does not contain the text "apple".
find /home/jeff/fruit | grep 'apple'

# Locate and print a list of any file in or below the current directory whose name is exactly "apple"
find . -name 'apple'

#Locate and print a list of any file in or below the current directory whose name is "apple", but match the letters case-INsensitively.
find . -iname 'apple'
```


- grep(Global Regular Expression Print)
    + return text that matches the string pattern passed to grep
```sh
$ls -l | grep "Aug"        #list all segment contain Aug in line
```

We will talk about regular expression later

**Note** normal matching control option include:
- -i, --ignore-case :insensitive
- -v, --invert-match :select non-matching lines
- -x, --line-regexp : select only that exactly match the whole line

- tee(T-splitter)
    + redirects stdin to both stdout and one or more files
The tee command is named after the T-splitter in plumbing, which splits water into two directions and is shaped like an uppercase T.
```ls
ls -1 *.txt | wc -l | tee count.txt
```
In the above example, the **ls** command lists all files in the current directory in *single column* that have the file name extension .txt, one file per line; this output is piped to **wc**, which counts the lines and outputs the number; this output is piped to **tee**, which writes the output to the terminal, and writes the same information to the file count.txt. If count.txt already exists, it is overwritten.

- tr (translate)
    + finds and replaces one string with another
这个解释并不是很清楚，实际上 tr (TRanslate)最主要的作用是应用REGEX规则替换、压缩、删除 原来的字符，其实可以达到简单的文本数据预处理的目的

```sh
echo "HELLO WORLD" | tr 'A-Z' 'a-z' #echo 的结果pipe给tr,然后小写替换所有的大写
$ hello world
echo "hello 123 world 456" | tr -d '0-9'  #删除数字
$ hello world 
```


- wc (word count)
    + counts character, lines and words(prints a count of newlines, words, and bytes for each input file.)
```sh
# displays information about, output 1. number of lines, 2. number of words, 3. number of characters
wc myfile.txt
$ 5 13 22 myfile.txt

# return the number of objects in the current directory
ls -1 | wc -l  # listing directory contents in single -column(-1), and piped to wc, which counts the lines (-l)

```
