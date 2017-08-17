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
