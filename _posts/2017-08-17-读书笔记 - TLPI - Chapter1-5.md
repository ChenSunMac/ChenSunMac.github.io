---
layout: post
title: 读书笔记 - TLPI - Chapter1-5
date: 2017-08-17
tag: Unix, Shell, Linux, File System, Process, Thread
comments: true
---

## History of Unix, Linux and C

UNIX 是在1969年的BELL实验室（AT&T的一部分）诞生的，硬件平台是Digital PDP-7 minicomputer (With a cost of US$72,000). 
<img http="https://upload.wikimedia.org/wikipedia/commons/thumb/5/52/Pdp7-oslo-2005.jpeg/300px-Pdp7-oslo-2005.jpeg">博物馆里的PDP-7</img>. 

随后由于一系列原因，UCB成为了研究和开发UNIX的主力军，1979年时，UCB已经完成UNIX一个分支的开发，BSD.

<br>
Two different currents led to the development of (GNU/) Linux. One of these was the GNU project, founded by Richard Stallman. By the late 1980s, the GNU project had produced an almost complete, freely distributable UNIX implementation. The one part lacking was a working kernel. In 1991, inspired by the Minix kernel written by Andrew Tanenbaum, Linus Torvalds produced a working UNIX kernel for the Intel x86-32 architecture.


但是这些不同的Braches以及开源所带来的问题就是portability problems, (即使现在，笔者也曾经因为在linux上安装驱动兼容等问题二头疼不已)。 

The C language was standardized in 1989 (C89), and a revised standard was produced in 1999 (C99).

The first attempt to standardize the operating system interface yielded POSIX.1, ratified as an IEEE standard in 1988, and as an ISO standard in 1990.

Linux separates implementation from distribution. Consequently, there is no single “official” Linux distribution. Each Linux distributor’s offering consists of a snapshot of the current stable kernel, with various patches applied.
<br>

<a https="http://www.ruanyifeng.com/blog/2010/03/unix_copyright_history.html">A bit More on UNIX History</a>
## Fundamental Concepts

### Kernel

Kernel in UNIX is more like the core Operating System - the central software that manages and allocates computer resources (i.e., the CPU, RAM, and devices).

<br>
#### Tasks performed by Kernel

 - Process Scheduling
The Definition of Process will be discussed under File I/O section
 <br>
  (***preemptive multitasking*** operating system)
    + preemptive means the rules governing which processes receive use of the CPU and for how long are determined by the kernel process scheduler, rahter than processes themselves
    + Multitasking means that multiple processes can simultaneously reside in memory and each may receive use of the CPU
<br>
 - Memory Management
<br>
  Linux employs virtual memory management, a technique confers 2 main advantages 
  
    + processes are isolated from each other and the kernel, 一个独立的进程不能 read or modify another process or Kernel  
    + increases the likelihood that, at any moment in time, there is at least one process that the CPU(s) can execute.
    <br>
   - file system
   <br>
   - Access to devices
   <br>
   The kernel provides programs with an interface that standardizes and simplifies access to devices, while at the same time arbitrating access by multiple processes to each device.


  - Networking
  
  <br>
  
  - **system call application programming interface (API)**:
  <br>
Processes can request the kernel to perform various tasks using kernel entry points known as system calls.


### The Shell
<br>
A ***shell*** is a special-purpose program designed to read commands typed by a user and execute appropriate programs in response to those commands. On UNIX systems, the shell is a user process.


### Single Dierctory Hierarchy
<br>
The kernel maintains a **single hierarchical directory structure** to organize all files in the system. (This contrasts with operating systems such as *Microsoft Windows*, where each disk device has its own directory hierarchy.)


<br>
Every directory contains at least two entries: 

*. (dot)*, which is a link to the directory itself, and 


*.. (dot-dot)*, which is a link to its parent directory, for the root directory, it links to itself.

<br>
Each process has a ***current working directory (cwd)***. This is the process’s “current location” within the single directory hierarchy, and it is from this directory that relative pathnames are interpreted for the process.

#### File I/O Model

**Universality of I/O**
This means that the same system calls (*open(), read(), write(), close()*, and so on) are used to perform I/O on all types of files, including devices.

#### Process

a *process* is an instance of an executing program. When a program is executed, the kernel loads the code of the program into virtual memory, allocates space for program variables, and sets up kernel bookkeeping data structures to record various information (such as process ID, termination status, user IDs, and group IDs) about the process.

<br>

一个进程可以从逻辑上分为以下几个部分：
 - Text： Program Instructions, 参考MIPS Assembly 里的.text
 <br>
 - Data: static variables used by program, 参考Assembly里的.data
 <br>
 - Heap: area that program can dynamically allocate extra memory
<br>
- Stack: for function call, allocate storage for local variable and function call linkage information, 参考Assembly里recursion
 
##### Process creation and program execution
A process can create a new process using the *fork()* system call. （实际上是Process用fork()对kernel进行system call，然后kernel创建了新的Process）

The process that calls fork() is referred to as the parent process, and the new process is referred to as the child process.

The **child process** goes on either to execute a different set of functions in the same code as the parent, or, frequently, to use the *execve()* system call to load and execute an entirely new program. An *execve()* call destroys the existing text, data, stack, and heap segments, replacing them with new segments based on the code of the new program.

<br>
Each process has a unique ***integer process identifier (PID)***. Each process also has a **parent process identifier (PPID)** attribute, which identifies the process that requested the kernel to create this process.

### Interprocess Communication and Synchronization

Linux and Unix provide rich set of mechanisms for ***interprocess communication (IPC)***:
<br>
- *signals*: to indicate that an event has occurred;  Think it as ***Software Interrupt***
<br>
- *Pipes*: to transfer data between processes;
<br>
- *Sockets*: to transfer data from one process to another;
<br>
- *file locking*: allows a process to lock regions of a file in order to prevent other processes from reading or updating the file contents
<br>
- *message queues*: exchange messages (packets of data) between processes;
<br>
- *semaphores*: synchronize the actions of processes
<br>
- *shared memory*: Allows 2 or more processes to share a piece of memory


### Threads

In modern UNIX implementations, each process can have multiple threads of execution.

Each thread is executing the same program code and shares the same data area and heap. However, each thread has it own stack containing local variables and function call linkage information.


### Session
A session is a collection of process groups ( jobs).

<br>
Sessions are used mainly by job-control shells. All of the process groups created
by a job-control shell belong to the same session as the shell, which is the session
leader.



### File Abstraction

Everything is a file.

And UNIX has unified file interface with *open, read, write, close* methods.

<br>
#### inode

The data for each file is managed by an array of on-disk data structure called ***inodes***.

One inode is allocated for each file and each directory.


理解inode首先要理解以下两个部分

第一是文件在硬盘上的存储，硬盘（磁盘）的最小存储单位是“扇区”（sector）, each sector store around 0.5KB;

OS在读取硬盘时，一般一次连续读取多个sector，也就是一个“块”(block), 一般是4KB，也就是8个sector.

第二呢，就是文件，任何文件实际都是binary code，那么他们都存储在块里 ***（硬盘数据区）***

但是除了纯粹的文件内容，我们还需要知道文件的一些***meta data（data that describe data）***，比如文件的大小，创建日期，权限等等。

那么 这些metadata的存放区域就是inode,当然也在硬盘里***(inode table区域)***。
<br>
硬盘格式化的时候，操作系统自动将硬盘分成两个区域。一个是数据区，存放文件数据；另一个是inode区（inode table），存放inode所包含的信息。

<br>
inode 里包含了
- 文件字节数
- 拥有者id
- group id
- 读写以及执行权限
- ctime (上次inode*change*的时间)；  mtime (上一次内容(manipulate)变动的时间);  atime(上一次文件打开的时间)
- 链接数 （多少文件名指向这个inode）
- 文件数据block 位置

查看inode 信息
```sh
stat example.txt //查看example.txt这个文件的inode信息
ls -i //列出所在路径的文件并且显示inode号码  List Segment
ls -i example.txt //找到example.txt对应的inode
```


每个inode节点的大小，一般是128字节或256字节。inode节点的总数，在格式化时就给定，一般是每1KB或每2KB就设置一个inode。假定在一块1GB的硬盘中，每个inode节点的大小为128字节，每1KB就设置一个inode，那么inode table的大小就会达到128MB，占整块硬盘的12.8%.

```sh
df -i  //Disk space being used by File systems 查看每个硬盘分区的inode总数和已经使用的数量
```

系统内部这个过程分成三步：首先，系统找到这个文件名对应的inode号码；其次，通过inode号码，获取inode信息；最后，根据inode信息，找到文件数据所在的block，读出数据。

<br>

### 文件名与inode 分离的UNIX世界 HARD link and SOFT link
Unix/Linux系统允许，多个文件名指向同一个inode号码。

这意味着，可以用不同的文件名访问同样的内容；对文件内容进行修改，会影响到所有文件名；但是，删除一个文件名，不影响另一个文件名的访问。这种情况就被称为"硬链接"（hard link）。

```sh
ln file1.txt file2.txt // create hard LiNk to files
```

创建目录时，默认会生成两个目录项："."和".."。前者的inode号码就是当前目录的inode号码，等同于当前目录的"硬链接"；后者的inode号码就是当前目录的父目录的inode号码，等同于父目录的"硬链接"。所以，任何一个目录的"硬链接"总数，总是等于2加上它的子目录总数（含隐藏目录）。


<br>

文件A和文件B的inode号码虽然不一样，但是文件A的内容是文件B的路径。读取文件A时，系统会自动将访问者导向文件B。因此，无论打开哪一个文件，最终读取的都是文件B。这时，文件A就称为文件B的"软链接"（soft link）或者"符号链接（symbolic link）。

这个和shortcut很像

```sh
ln -s sourceFile1.txt linkfile.txt // create -soft LiNk to files
```

打开一个文件以后，系统就以inode号码来识别这个文件，不再考虑文件名。因此，通常来说，系统无法从inode号码得知文件名。

第3点使得软件更新变得简单，可以在不关闭软件的情况下进行更新，不需要重启。因为系统通过inode号码，识别运行中的文件，不通过文件名。更新的时候，新版文件以同样的文件名，生成一个新的inode，不会影响到运行中的文件。等到下一次运行这个软件的时候，文件名就自动指向新版文件，旧版文件的inode则被回收。
