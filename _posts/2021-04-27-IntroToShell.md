---
layout: post
author: ledinhtrunghieu
title: Lesson 6 - Introduction to Shell
---
    
# 1. Manipulating files and directories

A brief introduction to the Unix shell. Why it is still in use after almost 50 years, how it compares to the graphical tools, how to move around in the shell, and how to create, modify, and delete files and folders.

## 1.1. Introduction

An **operating system** like Windows, Linux, or Mac OS is a special kind of program. It controls the computer's processor, hard drive, and network connection, but its most important job is to run other programs.

Since human beings aren't digital, they need an **interface** to interact with the **operating system**. The most common one these days is a **graphical file explorer**, which **translates clicks and double-clicks into commands** to open files and run programs.

Before computers had graphical displays, though, people typed instructions into a program called a **command-line shell**. Each time a command is entered, the shell runs some other programs, prints their output in human-readable form, and then displays a **prompt** to signal that it's ready to accept the next command. (Its name comes from the notion that it's the "outer shell" of the computer.)

## 1.2. Basic directories commands

The **filesystem** manages files and directories (or folders). Each is identified by an **absolute path** that shows how to reach it from the filesystem's **root directory**: `/home/repl` is the directory repl in the directory home, while `/home/repl/course.txt` is a file course.txt in that directory, and `/` on its own is the root directory.

**Where I am**
`pwd`: print working directory. This prints the absolute path of your current working directory

**List**
`ls`: lists the contents of your current directory

An **absolute path** is like a latitude and longitude: it has the same value no matter where you are. A **relative path**, on the other hand, specifies a location starting from where you are: it's like saying "20 kilometers north".

As examples: 
* If you are in the directory `/home/repl`, the relative path `seasonal` specifies the same directory as the absolute path `/home/repl/seasonal`.
* If you are in the directory `/home/repl/seasonal`, the relative path `winter.csv` specifies the same file as the absolute `path /home/repl/seasonal/winter.csv`.

**Move to another directory**
`cd`: change directory

`..` (two dots with no spaces) means "the directory above the one I'm currently in

`.` means "the current directory". you can use `ls .`

`~` meams your home directory. `ls ~` will always list the contents of your home directory, and `cd ~` will always take you home.

`cd ~/../.` : The path means 'home directory', 'up a level', 'here'.

## 1.3. Basic files commands


**Copy Files**
`cp` : copy

```
cp original.txt duplicate.txt
```

creates a copy of `original.txt` called `duplicate.txt`. If there already was a file called `duplicate.txt`, it is overwritten. If the last parameter to cp is an existing directory, then a command like:

```
cp seasonal/autumn.csv seasonal/winter.csv backup
```

copies all of the files into that directory (backup)

**Move files**
`mv`: moves

**Rename Files**
`mv` can also be used to rename files. If you run:
```
mv course.txt old-course.txt
```
then the file `course.txt` in the current working directory is "moved" to the file `old-course.txt`. This is different from the way file browsers work, but is often handy.

**Delete files**
`rm`: removes,unlike graphical file browsers, the shell doesn't have a trash can, so when you type the command above, your thesis is gone for good.

```
rm thesis.txt backup/thesis-2017-08.txt
```
removes both `thesis.txt` and `backup/thesis-2017-08.txt`

**How can I create and delete directories?**

`mv` treats directories the same way it treats files: if you are in your home directory and run 
```
mv seasonal by-season
```
`mv` changes the name of the seasonal directory to by-season

If you try to `rm` a directory, the shell prints an error message telling you it can't do that, primarily to stop you from accidentally deleting an entire directory full of work. Instead, you can use a separate command called `rmdir`. For added safety, it only works when the directory is empty, so you must delete the files in a directory before you delete the directory. (Experienced users can use the -r option to rm to get the same effect; we will discuss command options later.)

Since a directory is not a file, you must use the command `mkdir` directory_name to create a new (empty) directory

