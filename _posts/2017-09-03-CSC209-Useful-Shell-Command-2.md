---
layout: post
title: CSC 209 Useful Shell Commands 2
date: 2017-09-03
tags: Unix, Shell, Linux, File System, CSC209
categories: Linux & C
---

### File Content Manipulation

1. cat :(**catenate**)
It reads data from files, and outputs their contents. It can be used to:
- Display text files
- Copy text files into a new document
- Append the contents of a text file to the end of another text file, combining them
```sh
### No >, e.g. default using std out
cat mytext.txt      
# read from mytext.txt and send them to std out (terminal screen)
cat mytext.txt mytext2.txt 
# print the contents of those two text files as if they were a single file.
```
