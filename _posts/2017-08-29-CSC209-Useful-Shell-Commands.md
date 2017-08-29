---
layout: post
title: CSC 209 Useful Shell Commands
date: 2017-08-29
tag: Unix, Shell, Linux, File System, CSC209
comments: true
---


This post works as a cheating sheet for CSC209 at University of Toronto.

### File Manipulation:

1. pwd:  (***P***rint the name of the current ***W***orking ***D***irectory)<br>
2. ls: （**L**ists the Names of Files （**S**egment）） 
```sh
ls -l           # Use a long listing format.
ls -i           # Print the index number of each file.
ls -a           # Do not ignore entries starting with ".", providing visibility to hidden files
ls -d           # List directory entries instead of contents
```
<br>
3. rm: (**R**e**M**ove file, e.g. deletes a file)

By default, it does not remove directories; The removal process unlinks a file name in a filesystem from data on the storage device, and marks that space as usable by future writes. The data itself is not destroyed, but after being unlinked, it becomes inaccessible. The effects of an rm operation cannot be undone.
```sh
rm YanZhaoMen.jpg   # delete this file(unlink the file name to data)
rm -r ExampleDirectory  # Remove directories and their contents recursively.
```

4. mv: (**M**o***V***e or rename files)

mv renames file SOURCE to DEST, or moves the SOURCE file (or files) to DIRECTORY.

```sh
mv existing-filename new-filename  # rename without copying the file
mv [OPTION]... SOURCE... DIRECTORY # move source to directory
mv YanZhaoMen.jpg ./home/SiXiangPingDeJiaoYu  # move this file to the directory
```

*mv* can destroy a file when there is the file has the same name with SOURCE in DESTINATION directory. The unlucky file with the same name will be overwritten by the SOURCE.

5.  cp: (**C**o***p***ies a File)

```sh
cp source_file destination_file     # copy this file and rename it in the same folder
cp origfile /directory/subdirectory  # copy the origfile to directory
cp origfile /directory/subdirectory/newfile  #  copy the origfile to directory and give it a new name
```
*cp* can destroy a file when there is the file has the same name with SOURCE in DESTINATION directory. The unlucky file with the same name will be overwritten by the SOURCE.


6. ln: (**LINK** files)
By default, create a hard link.

A link is an entry in your file system which connects a file name to the actual bytes of data on the disk. A Hard Link is something like this:
<img src="https://www.computerhope.com/unix/images/link-diagram.jpg"></img>

Deleting file1 or file2 will not effect each other.

A Soft link (Symbolic link) is something like this
<img src="https://www.computerhope.com/unix/images/symlink-diagram.jpg"></img>
remove file1 will affect file2.

```sh
ln file1.txt file2.txt
ln -s file1.txt file2.txt  
```


### File Content Manipulation
