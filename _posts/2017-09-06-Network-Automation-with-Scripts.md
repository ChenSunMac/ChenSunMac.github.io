---
layout: post
title: Network Automation with Scripts
date: 2017-09-06
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


### Scripting

#### Bash Scripts
After we know how to log on to devices and how to run scripts, the following example shows how to get information from the Stinger device.
```sh
#!/bin/bash
JMPSERVER=jump.myisp.net                # Name of Jump Server 
PORT=9012                               # Port Name
LOGINSEQ=('secret1' 'myadm' 'secret2')  # telnet password, user name, and user password
CMDS=('date' 'info' 'uptime' 'quit')    # commands issued to the DSLAM command line
while read device
do
   # In background, run an ssh tunnel
   ssh  -N ${JMPSERVER} -L ${PORT}:${device}:23 &
   sleep 2 #insert a delay (in seconds) between the echo of each string value
   for n in 1
   do
      # login sequence
      typeset -i i=0
      while test ${i} -lt ${#LOGINSEQ[*]}
      do
          echo ${LOGINSEQ[${i}]}
          i+=1
          sleep 1
      done
      # run commands
      typeset -i j=0
      while test ${j} -lt ${#CMDS[*]} 
      do
          echo ${CMDS[${j}]}
          j+=1
          sleep 1
      done
   done | telnet localhost ${PORT}
   kill $! # terminate the ssh background process
done
```


When you run the script, it waits for standard input. Enter the domain name (or IP address) of a network device (or alternatively redirect input from a file containing a list of devices):
<br>
$ ./get_info
st1.myisp.net
Trying 127.0.0.1...
Connected to localhost.
Escape character is  
(City Stgr 1) Enter password:
User: myadm
Password: myadm> date
Thu Feb 15 11:06:26 2007
myadm> info
Platform         : Lucent Stinger FS+
System Name      : City Stgr 1
Serial Number    : 1519531234
Software Version : TAOS 9.9.1 (stngrcm2)
Boot Version     : TAOS 9.9.1
Installed Memory : 128MB
Controller Role  : Primary
Hardware revision: 2.2 Model E - IP (Version B)
myadm> uptime
Current system time: 11:06:28
{ shelf-1 first-control-module } cm-v2 63 days 07:13:06
( PRIMARY )
myadm> quit
<br>

This bash Scripts  has some limitations. We use a pre-specified **sleep** time between the echo of each string value. Thus, we assume that the DSLAM will respond within the specified sleep time. 

However, it would be desirable to synchronize communication with the response of the device. For example, send the telnet password only after reception of the password prompt, instead of some after arbitrary delay.
```py
#!/usr/bin/env python

import sys

from automgmt import *

def proc_info(data,t):
    dd = {'device' : t.stinger}
    for line in data:
        if re.search(':', line):
            key,value = line.split(':')
            dd[key.strip().lower()] = value.strip()
    return dd

if __name__=='__main__':

    info = []

    # read standard input
    for stinger in sys.stdin.readlines():

        # initiate Telnet session to DSLAM
        try:
            t = stcon(stinger.strip('\n'))
        except TelnetError, e:
            # if there's an error, report and
            # try next DSLAM
            print "%s: %s" % (stinger, e.args)
            continue

        # issue "info" command, store each
        # line of output in a list of lists
        info.append(t.do_cmd("info"))
        t.close()

    for dslam in info:
        for line in dslam:
            print line
```