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

# 2. Manipulating data

**View a file's contents**
`cat`: prints the contents of files onto the screen. (Its name is short for "concatenate", meaning "to link things together", since it will print all the files whose names you give it, one after the other.)

**view a file's contents piece by piece**
When you `less` a file, one page is displayed at a time; you can press spacebar to page down or type q to quit.
If you give `less` the names of several files, you can type `:n` (colon and a lower-case 'n') to move to the next file, `:p` to go back to the previous one, or `:q` to quit.

**Look at the start of a file**
`head` seasonal/summer.csv

**Tab**
One of the shell's power tools is **tab completion**. If you start typing the name of a file and then press the tab key, the shell will do its best to auto-complete the path

**Control what commands do**

```
head -n 3 seasonal/summer.csv
```
will only display the first three lines of the file. If you run head `-n 100`, it will display the first 100 (assuming there are that many), and so on.

A flag's name usually indicates its purpose (for example, `-n` is meant to signal "number of lines"). Command flags don't have to be a - followed by a single letter, but it's a widely-used convention.

**List everything below a directory**

`ls` the flag `-R` (which means "recursive").
```
ls -R -F
```
To help you know what is what, `ls` has another flag `-F` that prints a `/` after the name of every directory and a `*` after the name of every runnable program. Run `ls` with the two flags, `-R` and `-F`, and the absolute path to your home directory to see everything it contains.

**get help for a command**

To find out what commands do, people used to use the `man` command (short for "manual"). 

For example, the command `man head` brings up this information:

```
HEAD(1)               BSD General Commands Manual              HEAD(1)

NAME
     head -- display first lines of a file

SYNOPSIS
     head [-n count | -c bytes] [file ...]

DESCRIPTION
     This filter displays the first count lines or bytes of each of
     the specified files, or of the standard input if no files are
     specified.  If count is omitted it defaults to 10.

     If more than a single file is specified, each file is preceded by
     a header consisting of the string ``==> XXX <=='' where ``XXX''
     is the name of the file.

SEE ALSO
     tail(1)
```

The one-line description under NAME tells you briefly what the command does, and the summary under SYNOPSIS lists all the flags it understands. Anything that is optional is shown in square brackets [...], either/or alternatives are separated by |, and things that can be repeated are shown by ..., so head's manual page is telling you that you can either give a line count with -n or a byte count with -c, and that you can give it any number of filenames.


Use `tail` with the flag -n +7 to display all but the first six lines of seasonal/spring.csv.

**Select columns from a file?**

```
cut -f 2-5,8 -d , values.csv
```
which means "select columns 2 through 5 and columns 8, using comma as the separator". cut uses -f (meaning "fields") to specify columns and -d (meaning "delimiter") to specify the separator. You need to specify the latter because some files may use spaces, tabs, or colons to separate columns.

`cut` is a simple-minded command. In particular, it doesn't understand quoted strings. If, for example, your file is:

```
Name,Age
"Johel,Ranjit",28
"Sharma,Rupinder",26
```

```
cut -f 2 -d , everyone.csv
```

```
Age
Ranjit"
Rupinder"
```

**Repeat commands**

`history` will print a list of commands you have run recently. Each one is preceded by a serial number to make it easy to re-run particular commands: just type `!55` to re-run the 55th command in your history
You can also re-run a command by typing an exclamation mark followed by the command's name, such as `!head` or `!cut`, which will re-run the most recent use of that command.

**Select lines containing specific values**

`head` and `tail` select rows, `cut` selects columns, and `grep` selects lines according to what they contains.

For example, `grep bicuspid seasonal/winter.csv` prints lines from winter.csv that contain "bicuspid".

some of `grep`'s more common flags:
* `-c`: print a count of matching lines rather than the lines themselves
* `-h`: do not print the names of files when searching multiple files
* `-i`: ignore case (e.g., treat "Regression" and "regression" as matches)
* `-l`: print the names of files that contain matches, not the matches
* `-n`: print line numbers for matching lines
* `-v`: invert the match, i.e., only show lines that don't match

Invert the match to find all of the lines that don't contain the word molar in seasonal/spring.csv, and show their line numbers

```
grep -v -n molar seasonal/spring.csv
```

Count how many lines contain the word `incisor` in `autumn.csv` and `winter.csv` combined. (Again, run a single command from your home directory.)

```
grep -c incisor seasonal/autumn.csv  seasonal/winter.csv
```

**Why isn't it always safe to treat data as text?**
`paste`:  run paste to combine the autumn and winter data files in a single table using a comma as a separator. The last few rows have the wrong number of columns.

# 3. Combining tools

**Store a command's output in a file**

```
head -n 5 seasonal/summer.csv > top.csv
```

**use a command's output as an input**

Select the last two lines from `seasonal/winter.csv` and save them in a file called `bottom.csv`.

Select the first line from `bottom.csv` in order to get the second-to-last line of the original file.

```
tail -n 2 seasonal/winter.csv > bottom.csv
head -n 1 bottom.csv
```

**Better way to combine commands**

Using `|`.

```
head -n 5 seasonal/summer.csv | tail -n 3
```

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth
```

**Combine many commands**

```
cut -d , -f 2 seasonal/summer.csv | grep -v Tooth | head -n 1
```

**Count the records in a file**

The command `wc` (short for "word count") prints the number of **characters**, **words**, and **lines** in a file. You can make it print only one of these using `-c`, `-w`, or `-l` respectively.
```
grep 2017-07 seasonal/spring.csv | wc -l
```

**Specify many files at once**

```
cut -d , -f 1 seasonal/winter.csv seasonal/spring.csv seasonal/summer.csv seasonal/autumn.csv
```

```
cut -d , -f 1 seasonal/*
```

```
cut -d , -f 1 seasonal/*.csv
```

Write a single command using head to get the first three lines from both `seasonal/spring.csv` and `seasonal/summer.csv`, a total of six lines of data, but not from the `autumn` or `winter` data files

```
head -n 3 seasonal/s* # ...or seasonal/s*.csv, or even s*/s*.csv
```

**What other wildcards can I use?**

The shell has other wildcards as well, though they are less commonly used:
* `?` matches a single character, so `201?.txt` will match `2017.txt` or `2018.txt`, but not `2017-01.txt`.
* `[...]` matches any one of the characters inside the square brackets, so `201[78].txt` matches `2017.txt` or `2018.txt`, but not `2016.txt`.
* `{...}` matches any of the comma-separated patterns inside the curly brackets, so `{*.txt, *.csv}` matches any file whose name ends with .txt or .csv, but not files whose names end with `.pdf`.

**Sort lines of text**

