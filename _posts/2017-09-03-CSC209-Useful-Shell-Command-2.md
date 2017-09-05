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
### OverWriting using >
cat mytext.txt > newfile.txt    
# the shell will redirect the output to the file newfile.txt, 
# if newfile.txt does not exist, it will be created, 
# if newfile.txt already exists, it will be overwritten.
cat mytext.txt mytext2.txt > newfile.txt 
# read the contents of mytext.txt and mytext2.txt 
# and write the combined text to the file newfile.txt

### Append using >>
cat mytext.txt >> another-text-file.txt 
# read the contents of mytext.txt, and 
# write them at the end of another-text-file.txt
```
