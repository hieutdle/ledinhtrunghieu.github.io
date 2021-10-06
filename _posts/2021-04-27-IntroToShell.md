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
* If you are in the directory `/home/repl/seasonal`, the relative path `winter.csv` specifies the same file as the absolute path `/home/repl/seasonal/winter.csv`.

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

**Make a copy of seasonal/summer.csv in the backup directory (which is also in /home/repl), calling the new file summer.bck.**
```
cp seasonal/summer.csv backup/summer.bck
```

**Copy spring.csv and summer.csv from the seasonal directory into the backup directory without changing your current working directory (/home/repl).**
```
cp seasonal/autumn.csv seasonal/winter.csv backup
```


**Move files**
`mv`: moves

**Rename Files**
`mv` can also be used to rename files. If you run:
```
mv course.txt old-course.txt
```
then the file `course.txt` in the current working directory is "moved" to the file `old-course.txt`. This is different from the way file browsers work, but is often handy.

**You are in /home/repl, which has sub-directories seasonal and backup. Using a single command, move spring.csv and summer.csv from seasonal to backup.**

```bash
mv seasonal/spring.csv seasonal/summer.csv backup
```

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

```
cat course.txt
```

**view a file's contents piece by piece**
When you `less` a file, one page is displayed at a time; you can press spacebar to page down or type q to quit.
If you give `less` the names of several files, you can type `:n` (colon and a lower-case 'n') to move to the next file, `:p` to go back to the previous one, or `:q` to quit.

```
less seasonal/spring.csv seasonal/summer.csv
```

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


**Display the first 5 lines of winter.csv in the seasonal directory.**
```
$ head -n 5 seasonal/winter.csv
```


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

**What is the output of cut -d : -f 2-4 on the line:**
```
first:second:third:
```

```
second:third:
```




**Repeat commands**

`history` will print a list of commands you have run recently. Each one is preceded by a serial number to make it easy to re-run particular commands: just type `!55` to re-run the 55th command in your history
You can also re-run a command by typing an exclamation mark followed by the command's name, such as `!head` or `!cut`, which will re-run the most recent use of that command.

```
$ head summer.csv
head: cannot open 'summer.csv' for reading: No such file or directory
```

```
$ cd seasonal
$ !head
head summer.csv
Date,Tooth
2017-01-11,canine
2017-01-18,wisdom
2017-01-21,bicuspid
2017-02-02,molar
2017-02-27,wisdom
2017-02-27,wisdom
2017-03-07,bicuspid
2017-03-15,wisdom
2017-03-20,canine
```
Use `history` to look at what you have done.




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


Print the contents of all of the lines containing the word molar in seasonal/autumn.csv by running a single command while in your home directory. Don't use any flags.
```
grep molar seasonal/autumn.csv
```

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

**Which expression would match singh.pdf and johel.txt but not sandhu.pdf or sandhu.txt?**
```
{singh.pdf, j*.txt}
```


**Sort lines of text**

`sort` puts data in order. By default it does this in ascending alphabetical order, but the flags `-n` and `-r` can be used to sort numerically and reverse the order of its output, while `-b` tells it to ignore leading blanks and `-f` tells it to fold case (i.e., be case-insensitive). Pipelines often use `grep` to get rid of unwanted records and then sort to put the remaining records in order.

```
cut -d , -f 2 seasonal/winter.csv | grep -v Tooth | sort -r
 ```

 **Remove duplicate lines**

 Using `uniq`

 Write a pipeline to:
* get the second column from seasonal/winter.csv,
* remove the word "Tooth" from the output so that only tooth names are displayed,
* sort the output so that all occurrences of a particular tooth name are adjacent; and
* display each tooth name once along with a count of how often it occurs.

```
cut -d , -f 2 seasonal/winter.csv | grep -v Tooth | sort | uniq -c
```

**Save the output of a pipe**

The shell lets us redirect the output of a sequence of piped commands:

```
cut -d , -f 2 seasonal/*.csv | grep -v Tooth > teeth-only.txt
```

However, `>` must appear at the end of the pipeline: if we try to use it in the middle, like this:

```
cut -d , -f 2 seasonal/*.csv > teeth-only.txt | grep -v Tooth
```

then all of the output from `cut` is written to `teeth-only.txt`, so there is nothing left for `grep` and it waits forever for some input.

**stop a running program**

`Ctrl` + `C`

**Wrapping up**
* Use `wc` with appropriate parameters to list the number of lines in all of the seasonal data files. (Use a wildcard for the filenames instead of typing them all in by hand.)
* Add another command to the previous one using a pipe to remove the line containing the word "total".
* Add two more stages to the pipeline that use `sort -n` and `head -n 1` to find the file containing the fewest lines.
```
wc -l seasonal/* | grep -v total | sort -n | head -n 1
```

# 4. Batch processing

**How does the shell store information?**

The shell stores information in variables. Some of these, called **environment variables**, are available all the time.

**Print a variable's value**

A simpler way to find a variable's value is to use a command called `echo`, which prints its arguments. 

```
echo hello DataCamp!
```

prints
```
hello DataCamp!
```

`echo USER`: it will print the variable's name, `USER`.

To get the variable's value, you must put a dollar sign `$` in front of it. Typing

echo `$USER` prints `repl`

```
$ echo $OSTYPE
linux-gnu
```

**How else does the shell store information**

The other kind of variable is called a **shell variable**, which is like a local variable in a programming language.

To create a shell variable, you simply assign a value to a name:
```
training=seasonal/summer.csv
```
```
echo $training
seasonal/summer.csv
```

```
$ testing=seasonal/winter.csv
$ head -n 1 $testing
Date,Tooth
```

**Repeat a command many times**

Shell variables are also used in **loops**, which repeat commands many times. If we run this command:

```
for filetype in gif jpg png; do echo $filetype; done
```

it produces:
```
gif
jpg
png
```

Notice these things about the loop:
* The structure is `for` …variable… `in` …list… `; do` …body… `; done`
* The list of things the loop is to process (in our case, the words `gif`, `jpg`, and `png`).
* The variable that keeps track of which thing the loop is currently processing (in our case, `filetype`).
* The body of the loop that does the processing (in our case, echo `$filetype`).

**Repeat a command once for each file**

```
for filename in seasonal/*.csv; do echo $filename; done
for filename in people/*; do echo $filename; done
```

**Record the names of a set of files**

```
files=seasonal/*.csv
for f in $files; do echo $f; done
```

**Run many commands in a single loop**

```
for file in seasonal/*.csv; do head -n 2 $file | tail -n 1; done
```

**Why shouldn't I use spaces in filenames?**

```
mv July 2017.csv 2017 July data.csv
```
It's easy and sensible to give files multi-word names like July 2017.csv when you are using a graphical file explorer. However, this causes problems when you are working in the shell. Because it looks to the shell as though you are trying to move four files called `July`, `2017.csv`, `2017`, and `July` (again) into a directory called `data.csv`. Instead, you have to quote the files' names so that the shell treats each one as a single parameter:

```
mv 'July 2017.csv' '2017 July data.csv'

```

**How can I do many things in a single loop?**

The loops you have seen so far all have a single command or pipeline in their body, but a loop can contain any number of commands. To tell the shell where one ends and the next begins, you must separate them with semi-colons:

```
for f in seasonal/*.csv; do echo $f; head -n 2 $f | tail -n 1; done
```

```
seasonal/autumn.csv
2017-01-05,canine
seasonal/spring.csv
2017-01-25,wisdom
seasonal/summer.csv
2017-01-11,canine
seasonal/winter.csv
2017-01-03,bicuspid
```

Suppose you forget the semi-colon between the echo and head commands in the previous loop, so that you ask the shell to run:

```
for f in seasonal/*.csv; do echo $f head -n 2 $f | tail -n 1; done
```

```
seasonal/autumn.csv head -n 2 seasonal/autumn.csv
seasonal/spring.csv head -n 2 seasonal/spring.csv
seasonal/summer.csv head -n 2 seasonal/summer.csv
seasonal/winter.csv head -n 2 seasonal/winter.csv
``` 

Other test

```
for f in seasonal/*.csv; do echo $f | head -n 2 $f | tail -n 1; done
```

```
2017-01-05,canine
2017-01-25,wisdom
2017-01-11,canine
2017-01-03,bicuspid
```

# 5. Creating new tools

**How can I edit a file?**

Unix has a bewildering variety of text editors. For this course, we will use a simple one called **Nano**. If you type `nano filename`, it will open filename for editing (or create it if it doesn't already exist). You can move around with the arrow keys, delete characters using backspace, and do other operations with control-key combinations:
* `Ctrl` + `K`: delete a line.
* `Ctrl` + `U`: un-delete a line.
* `Ctrl` + `O`: save the file ('O' stands for 'output'). You will also need to press Enter to confirm the filename!
* `Ctrl` + `X`: exit the editor.

**How can I record what I just did**
```
head steps.txt

Copy the files seasonal/spring.csv and seasonal/summer.csv to your home directory.
cp seasonal/spring.csv seasonal/summer.csv ~
    
Use grep with the -h flag (to stop it from printing filenames) and -v Tooth (to select lines that don't match the header line) to select the data records from spring.csv and summer.csv in that order and redirect the output to temp.csv.
grep -h -v Tooth spring.csv summer.csv > temp.csv

Pipe history into tail -n 3 and redirect the output to steps.txt to save the last three commands in a file. (You need to save three instead of just two because the history command itself will be in the list.)
history | tail -n 3 > steps.csv
```

**Save commands to re-run later?**
You have been using the shell interactively so far. But since the commands you type in are just text, you can store them in files for the shell to run over and over again. 

Use `nano dates.sh` to create a file called dates.sh that contains this command:

```
cut -d , -f 1 seasonal/*.csv
```
to extract the first column from all of the CSV files in seasonal

```
bash dates.sh
```

**How can I re-use pipes?**

A file full of shell commands is called a **shell script**, or sometimes just a "script" for short. Scripts don't have to have names ending in .sh, but this lesson will use that convention to help you keep track of which files are scripts.

Scripts can also contain pipes. For example, if all-dates.sh contains this line:
```
cut -d , -f 1 seasonal/*.csv | grep -v Date | sort | uniq
```
then:
```
bash all-dates.sh > dates.out
```
will extract the unique dates from the seasonal data files and save them in dates.out.

**Pass filenames to scripts**

A script that processes specific files is useful as a record of what you did, but one that allows you to process any files you want is more useful. To support this, you can use the special expression `$@` (dollar sign immediately followed by at-sign) to mean "all of the command-line parameters given to the script".

For example, if `unique-lines.sh` contains sort `$@ | uniq`, when you run:

```
bash unique-lines.sh seasonal/summer.csv
```
the shell replaces `$@` with `seasonal/summer.csv` and processes one file. If you run this:
```
bash unique-lines.sh seasonal/summer.csv seasonal/autumn.csv
```

it processes two data files, and so on.


```
nano count-records.sh

bash count-records.sh seasonal/*.csv > num-records.out
```
**How can I process a single argument?**

As well as `$@`, the shell lets you use `$1`, `$2`, and so on to refer to specific command-line parameters. You can use this to write commands that feel simpler or more natural than the shell's. For example, you can create a script called `column.sh` that selects a single column from a CSV file when the user provides the filename as the first parameter and the column as the second:
```
cut -d , -f $2 $1
```

and then run it using:

```
bash column.sh seasonal/autumn.csv 1
```

Notice how the script uses the two parameters in reverse order.


The script get-field.sh is supposed to take a filename, the number of the row to select, the number of the column to select, and print just that field from a CSV file. For example:

bash get-field.sh seasonal/summer.csv 4 2
should select the second field from line 4 of seasonal/summer.csv. Which of the following commands should be put in get-field.sh to do that?

```
head -n $2 $1 | tail -n 1 | cut -d , -f $3
```

**How can one shell script do many things?**

Our shells scripts so far have had a single command or pipe, but a script can contain many lines of commands. For example, you can create one that tells you how many records are in the shortest and longest of your data files, i.e., the range of your datasets' lengths.

Note that in Nano, "copy and paste" is achieved by navigating to the line you want to copy, pressing CTRL + K to cut the line, then CTRL + U twice to paste two copies of it.

As a reminder, to save what you have written in Nano, type Ctrl + O to write the file out, then Enter to confirm the filename, then Ctrl + X to exit the editor.


**How can I write loops in a shell script?**


```
# Print the first and last data records of each file.
for filename in $@
do
    head -n 2 $filename | tail -n 1
    tail -n 1 $filename
done

```
Run date-range.sh on all four of the seasonal data files using `seasonal/*.csv` to match their names, and pipe its output to `sort` to see that your scripts can be used just like Unix's built-in commands.


```
$ nano date-range.sh
$ bash date-range.sh seasonal/*.csv
$ bash date-range.sh seasonal/*.csv | sort
```

**What happens when I don't provide filenames?**

tail: cannot open 'somefile.txt' for reading: No such file or directory
You should use Ctrl + C to stop a running program.

# 6. Reference

1. [Introduction to Shell - DataCamp](https://learn.datacamp.com/courses/introduction-to-shell)

