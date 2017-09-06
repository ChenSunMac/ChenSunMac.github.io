---
layout: post
title: Network Automation with Scripts
date: 2017-09-05
tags: Communication Network Script Python Shell
categories: Script Network
---

Managing a large domain of devices, such as ATM switches, DSLAMs (Digital Subscriber Line Access Multiplexers), and routers, (for 2nd, 3rd layer of OSI) can be problematic. 

1. Security Issue
<img src="http://collaboration.cmc.ec.gc.ca/science/rpn/biblio/ddj/Website/articles/SA/v16/i06/a2_f1.gif"></img>

Direct telnet access, from the support organization's network, to the managed devices cannot be allowed because packets (containing password information) traverse the public Internet.

**Solution**: Establishing a secure shell session (using SSH) to a jump server located within the ISP's network.

2. Convoluted process

It would be useful to automate certain management functions such that they could be carried out on multiple machines.

**Solution**:  Piping a series of command statements through the telnet process. The command-line interface, however, does not really provide a convenient "API" for the purpose of scripting management functions. The output from commands will be presented in a format intended for viewing on the screen, which can make post-processing in a script difficult. 

### SSH Tunnel to DSLAM
We want to log on to the device before any scripting. Rather than logging onto the jump server (with ssh) and then running a telnet client from the jump server to the DSLAM, we can use ssh tunnel:
```sh
$ ssh -N jump.myisp.net -L 9012:st1.myisp.net:23 &
```
where jump.myisp.net is the jump server and st1.myisp.net is the DSLAM (stinger card).

Then one can telnet through the tunnel from my local machine
```sh
$ telnet localhost 9012
```

To clear down the ssh tunnel, simply kill the background process:
```sh
$ kill $!
```


### Scripts

There is a saying "Scripts are read more than they are written." as we strive to write readable code. Also, one of my teacher said "Scripts are run more than they read", hence it is essential to know how to run scripts.

<br>
Suppose we write a hello.py script with only one line
```py
print("hello!")
```
Then in Linux shell, in the directory where hello.py sits, we can run it by
```sh
python hello.py
```
However, if we directly type in 
```sh
hello.py
```
in the shell command line, it is because Linux will only search for excutable files in the folder specify in our path, which is defined as environment variable (we can specify it using ./). Also, we may need to make the hello.py file executable
```sh
chmod +x hello.py
```
and then by calling
```sh
./hello.py
```
we can see that Linux is now execute the file, however there is some error during executing the scripts. This is because Linux treats the Python Scripts Command as Shell Command. Hence we need to add some additional lines in the scripts to tell Linux kernel that you should use python here.

#### SHEBANG\#!

\#! is short for HASH (\#) BANG (!). When Linux sees this, it knows sequence, it knows the file is a script. For example:
```
#!/bin/sh                  Use Shell
#!/usr/bin/env python      Use Python 
```

Then by adding the second line above to our hello.py, we can then execute our script by just calling *./hello.py* without any error.




