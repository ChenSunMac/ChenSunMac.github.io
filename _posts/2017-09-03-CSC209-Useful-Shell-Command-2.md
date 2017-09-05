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


2. less： less is more
When you want to view a file that is longer than one screen, you can use either the **less** utility or the **more** utility. Each of these utilities pauses after displaying a screen of text; press the SPACE bar to display the next screen of text. 

```sh
less [options] [files-list]
```

less is more 这种命名方式真的是恶趣味.
<br>
3. head： (prints the first 10 lines of each file to std out)
head makes it easy to output the first part of files.

```sh
head myfile.txt     # print first 10 lines 
head -15 myfile.txt     # print first 15 lines
head myfile.txt myfile2.txt 
# Display the first ten lines of both myfile.txt and myfile2.txt
#######
head -c 20 myfile.txt   
#output only the first twenty bytes (characters) of myfile.txt
```
<br>
4. tail (tail prints the last 10 lines of each FILE to standard output. )
同上
<br>
5. sort: (rearrange lines in a text file)
- 数字比字母优先
- 小数字、小字母优先
- 同样字母，小写比大写优先
<br>
6. uniq: (reports or filters out repeated lines in a file.)
Let's say we have an eight-line text file, myfile.txt, which contains the following text:
```
This is a line.
This is a line.
This is a line.
This is also a line.
This is also a line.
This is also also a line.
```
Using **uniq** will filters out repeated lines in the file.
```sh
uniq myfile.txt
```
we will get 
```
This is a line.
This is also a line.
This is also also a line.
```
Using *-d* will print out only the duplicated lines
```sh
uniq -d myfile.txt
```
we will get
```
This is a line. 
This is also a line. 
```
Using *-u* will print out only the unique lines.
```sh
uniq -u myfile.txt
```
we will get
```
This is also also a line.
```

7. wc: (word count)
prints a count of newlines, words, and bytes for each input file
```sh
wc myfile.txt       #Displays information about the file myfile.txt
```
Output will look like this
```
5 13 57 myfile.txt
```
Where 5 is the number of lines, 13 is the number of words, and 57 is the number of characters.

8. comm: (Compare two sorted files line-by-line.)
With no options, comm produces three-column output. Column one contains lines unique to FILE1, column two contains lines unique to FILE2, and column three contains lines common to both files.

9. diff
analyzes two files and prints the lines that are different. Essentially, it outputs a set of instructions for how to change one file to make it identical to the second file.


10. grep: (global regular expression print)

processes text line by line and prints any lines which match a specified pattern.
```sh
grep chope /etc/passwd  #Search /etc/passwd for user chope.
grep "May 31 03" /etc/httpd/logs/error_log 
# search the  error_log file for any error entries 
# that happened on May 31st at 3AM
grep -w "hope" myfile.txt  #Search the file myfile.txt for 
# lines containing the word "hope".
```

regular expression will be addressed in other post