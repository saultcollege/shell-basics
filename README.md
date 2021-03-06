# Command Line Shell Basics

<!-- TOC -->

- [Command Line Shell Basics](#command-line-shell-basics)
    - [Introduction](#introduction)
    - [What is a Shell?](#what-is-a-shell)
        - [Examples of Command Line Shells](#examples-of-command-line-shells)
    - [The Interface](#the-interface)
            - [Sample CLI session](#sample-cli-session)
        - [Basic Command Format](#basic-command-format)
        - [A Note on Documentation](#a-note-on-documentation)
        - [Shell Comments](#shell-comments)
        - [Command Arguments](#command-arguments)
        - [Command Options](#command-options)
            - [Multiple Options](#multiple-options)
            - [Order of Options](#order-of-options)
            - [Multiple Short Form Options](#multiple-short-form-options)
            - [Parameterized Options](#parameterized-options)
    - [Cancelling Commands](#cancelling-commands)
    - [Getting Information About Commands](#getting-information-about-commands)
    - [File System Navigation](#file-system-navigation)
        - [The Working Directory](#the-working-directory)
        - [Changing the Working Directory](#changing-the-working-directory)
        - [Paths](#paths)
            - [Absolute Paths](#absolute-paths)
            - [Relative Paths](#relative-paths)
            - [Paths and Spaces](#paths-and-spaces)
            - [Paths with Wildcards (AKA Globs)](#paths-with-wildcards-aka-globs)
    - [Environment Inspection](#environment-inspection)
        - [Environment Variables](#environment-variables)
        - [Setting Environment Variables](#setting-environment-variables)
        - [echo](#echo)
    - [File System Inspection](#file-system-inspection)
        - [cat](#cat)
        - [less](#less)
        - [head & tail](#head--tail)
        - [nano](#nano)
    - [Redirecting Output](#redirecting-output)
        - [Redirecting to a File](#redirecting-to-a-file)
        - [Redirecting to Other Commands](#redirecting-to-other-commands)
    - [File System Manipulation](#file-system-manipulation)
        - [mkdir & rmdir](#mkdir--rmdir)
        - [touch](#touch)
        - [cp](#cp)
        - [mv](#mv)
            - [Renaming Using mv](#renaming-using-mv)
        - [rm](#rm)
    - [The End!](#the-end)

<!-- /TOC -->

## Introduction

This tutorial introduces basic interaction with Unix-style command line shells such as Bash (although the concepts introduced here will also be useful in Windows Powershell).  By the end of the tutorial you should understand how to run commands in a shell, and you should be able to use a shell to navigate, inspect, and manipulate a file system.

## What is a Shell?

A shell is a user interface that allows a person to interact with a computer operating system such as Windows, MacOS, or Linux.  Some shells have a Graphical User Interface (GUI), but many have a simple Command Line Interface (CLI) into which the user may type commands that the operating system will then execute.  Commands may or may not produce text output that is also presented in the CLI.

A CLI shell is one of the primary tools used by programmers and computer administrators to inspect and configure the devices they are working on.

> **_NOTE:_**
> The terms `console` and `terminal` are often used interchangeably with the term `shell`.  For this tutorial that is all you need to know, but there are technically some differences between the three concepts.  You can read more in [this StackExchange answer](https://superuser.com/a/144668) if you are interested.

### Examples of Command Line Shells

There are many CLI shells available, but the table below lists the ones you will encounter most often in a typical computer environment.

| Shell Name | Environment |
|------------|------------------|
| bash       | MacOS or Linux terminal |
| cmd | Windows |
| Git Bash | Packaged with git on Windows |
| PowerShell | Windows |
| zsh        | Linux terminal |

> **_NOTE:_**
> This tutorial focuses on Unix-style shells, which all support a common set of commands.  Git Bash, bash, and zsh are all Unix-style shells.  If you are on a Windows device, you should be able to follow along in this tutorial using Git Bash.  If you are using PowerShell, see [this reference](files/PowerShell-equivalents-for-common-linux-commands.pdf) of PowerShell equivalents for common bash commands.

## The Interface

A typical CLI shell session involves entering a sequence of text commands into the shell to either obtain some information about the user's system, or alter some aspect of the user's system, or both.

#### Sample CLI session

```bash
> cd ~
> ls
file1.txt file2.txt
> mkdir newfolder
> ls
file1.txt file2.txt newfolder
> cd newfolder
> touch newfile.txt
> ls
newfile.txt
```

When a shell session begins, the user is usually given a prompt which which may be a simple symbol like `>` or `$` but often contains some information about the environment that the shell is runing in.

The user may then type a command, followed by the `enter` or `return` key to submit that command to the shell, at which point the command is executed, and any output generated by the command is displayed in the shell.

After a command has finished executing, the shell once again displays the prompt and the process can repeat.

With this simple usage pattern, you can examine and manipulate almost any aspect of your computer system if you know the right commands!

> **_NOTE:_**
> Your shell will probably show a different prompt than the ones you see in this tutorial, but the usage pattern will be the same.

### Basic Command Format

Commands in a shell usually follow a typical format:  the name of a command, followed by zero or more options, followed by zero or more arguments.

For example,

```bash
ls -a /Users/ali
```

is a command that lists the contents of the directory `/Users/ali`, including hidden files.  The name of the command is `ls`.  The option `-a` causes the `ls` command to include hidden files in its output.  Finally, the argument `/Users/ali` is the directory for which the contents are to be listed.

Many commands can be used without any options or arguments at all.  For example, the command `ls` by itself simply lists all non-hidden files and directories in the current working directory.  (Don't worry if you don't understand the term "working directory". We will define that soon!)

### A Note on Documentation

You may notice the use of terms inside of angle brackets in the documentation for command line tools, as in `ls <pathname>`. The angle-bracketed terms are usually placeholders describing what you should *actually* type when using the command.  You should *not* include the angle brackets in an actual command.  For example, `ls </Users/ali>` would be an incorrect interpretation of the previous documentation; `ls /Users/ali` would be correct.  

Some documentation formats also use an italic (slanted) font to indicate placeholder values, as in 
<code>ls *pathname*</code>.  Here again the 'pathname' part is intended to be replaced.

You will also encounter terms inside of square brackets, as in `ls [--all]`.  Terms inside of square brackets are optional.  Again the square brackets are **not** part of the command, they are simply there to indicate that the part inside the brackets may or may not be included in the command.  So `ls` and `ls --all` are both valid interpretations of a documentation fragment like `ls [--all]`, but `ls [--all]` is not itself a valid command.

### Shell Comments

And one more note before we move on:  any text after a `#` symbol is ignored by most shells.  Such sections of text are usually called 'comments.  For example:

```sh
# This is a comment

ls -la /Users/ali # Comments can be after a command

# The next line is ignored because it starts with a #
# ls -la /Users/ali
```

### Command Arguments

The "argument(s)" of a command are usually used to specify the thing(s) that the command will operate on.  Very often, the arguments are file or directory names, but arguments may also refer to other kinds of objects depending on the command.

### Command Options

Most commands have many options that allow the user to adjust the output or behaviour of the command in some way.  Options can often be specified in one of two ways: a one-letter flag that begins with a single hyphen `-`, or an equivalent long-form name that begins with two hyphens `--`.

For example, the above command could also have been written as

```bash
ls --all /Users/ali
```

> **_NOTE:_**
> Depending on your platform, the `ls` command may not support the long-form options

Long-form option names are usually more descriptive and easier to remember, but you will want to learn the most common short forms for the most common commands to make typing the commands quicker.

#### Multiple Options

Multiple options may usually be specified at once.  The command below lists all the files in `/Users/ali`, including hidden files (via the `--all` option), with one file per line in the output (via the `-1` option).

```bash
ls --all -1 /Users/ali
```

#### Order of Options

The order of options *usually* does not matter.  The command below does the same thing as the one above.

```bash
ls -1 --all /Users/ali
```

Note that we have mixed the use of short and long-form option names.  The command could of course also be written using only short form options:

```bash
ls -a -1 /Users/ali
```

#### Multiple Short Form Options

When multiple short form options are specified, they can be combined using a single hyphen followed immediately by all the short form options, as in

```bash
ls -a1 /Users/ali
```

#### Parameterized Options

Some options may accept parameters.  For example, the `--sort` option of the `ls` command can be set to `size` or `time` to sort the results accordingly, like this:

```bash
ls --sort=size /Users/ali
```

Of course, the two versions of the `--sort` option indicated above have short forms: `-S` and `-t`, respectively, so the above command could also be written as

```bash
ls -S /Users/ali
```

## Cancelling Commands

In some situations, especially for commands that take a long time to run, you may wish to cancel a command that you have issued to a shell.  You can do so by pressing `Ctrl+c`.

## Getting Information About Commands

Most commands come with extensive documentation in a "manual page" that can be viewed using the `man` (short for "manual") command.

For example, the command below prints out the manual page for the `ls` command.

```bash
man ls
```

> **_NOTE:_**
> Unfortunately, Git Bash does not include manual pages, but you can view the manuals for most commands using either the `--help` option or on [this website](https://linux.die.net/man/).

> **_NOTE:_**
> Manual pages can be quite long, and some shells will allow you to move up and down within the text using the `j` and `k` keys, respectively. You can also exit the manual page before you reach the end by typing `q` (for quit).

The `whatis` command can be used to print a short description of other commands.

Many commands also have a `--help` option that prints a short message describing the command and its most common usages.

It is often important to know which version of a command is on your system, which most commands will display when run with the `--version` option.

## File System Navigation

If you do not already have a shell open on your computer, you should do so now.  The best way to learn how to use a shell is to actually use one!  From this point on try to follow along by entering the commands into your shell.

### The Working Directory

Every shell command is executed in the context of a **working directory**.  You can print the current working directory using the `pwd` command:

```bash
> pwd
/Users/ali
```

The output of the `pwd` command is the directory in which the shell is currently operating.  Any commands that manipulate files will do so relative to this directory unless otherwise specified in the command.

### Changing the Working Directory

You can change the working directory using the `cd` command followed by a path to a directory.  (`cd` is short for "change directory")

For example, the command `cd /` changes the working directory to the root directory of the file system.

### Paths

Before going any further, let's make sure you understand what is meant by a path.  A path describes the set of directories that must be opened to get to some final directory or file on a computer system.

For example, suppose you have the following directory structure on your computer:

```
    /
    +-- www/ 
    |   +-- images/
    |   |   +-- logo.png
    |   |   +-- my-face.jpg
    |   |
    |   +-- about me/
    |   |   +-- index.html
    |   |
    |   +-- index.html
    |
    +-- Users/ 
        +-- ali/
            +-- Documents/
```

Here are a few valid paths in this structure:

```bash
/
/www
/www/images/logo.png
/Users
/Users/ali
```

> **_NOTE:_**
> In Unix-style shells, directory names are separated using the `/` symbol, but in Windows shells, the `\` symbol is the default.  However, in both PowerShell and Git Bash, either symbol may be used when typing paths.

#### Absolute Paths

All of the above paths are absolute paths.  That is, they begin at the root of the file system, and specify the sequence of directories from there to the final directory or file in the path.  

> **_NOTE:_**
> If you are using Git Bash, the root directory is **not** the root of your C drive.  You can change to the root of your C drive using the command `cd /c`.  (A similar command can be used to change to the root of any drive letter on your system.)

You must list **every** sequential directory in a path, otherwise the path is invalid and the command will not be able to find the file or directory.

For example, given the directory structure shown above, all the following paths are invalid:

```bash
/images/logo.png
/www/logo.png
/Users/bobbie
```

The first path specifies a directory named 'images' in the root directory, but there is no such directory.  The second path specifies a file named 'logo.png' inside the 'www' directory, but there is no such file.  The third path specifies a directory named 'bobbie' inside the 'Users' directory, but there is no such directory.

> **_EXERCISE_**
>
> Use the `cd` command to navigate to several different absolute paths in your system.  Use the `pwd` command to verify that your `cd` commands worked.

> **_TIP:_** Most shells features 'tab completion', meaning that you can begin typing the name of a directory in a path and then press the `tab` key.  If what you have typed so far can only be one possible directory or file then the shell will complete the name automatically.  If what you have typed so far is *not* a unique prefix, pressing `tab` a second time will print a list of directory and file names that start with what you have typed so far.

#### Relative Paths

It is also possible to use paths that are relative to the current working directory.  

The `.` symbol at the beginning of a path represents the current working directory.

Consider the following shell session:

```bash
> cd /www
> pwd
/www
> cd ./images
> pwd
/www/images
```

Here, we change to the `/www` directory, and then change from there to the `images` directory which is inside the `www` directory.

> **_NOTE:_**
> If the `./` symbol is the first part of a path it can usually be left off.  So the commands `cd ./images` and `cd images` have the same meaning.

The `..` symbol in a path represents the parent directory of the current directory in the path.  If the `..` symbol is at the beginning of the path it represents the parent directory of the current working directory.

Example:

```bash
> cd /www/images
> cd ..
> pwd
/www
```

You may use multiple instances of `..` in a path to navigate up multiple directories from the working directory:

Example:

```bash
> cd /Users/ali/
> cd ../../www/images
> pwd
/www/images
```

The `~` symbol at the beginning of a path represents the user's home directory.

Example:

```bash
> cd /www
> cd ~
> pwd
/Users/ali
> cd ~/Documents
> pwd
/Users/ali/Documents
```

And here as an example that combines the `~` and `..` symbols to change to the parent directory of the user's home directory.  (The `..` symbol does not need to be at the beginning of a path.  You can read this path as "start at the user's home directory, then move one directory up from there".)

```bash
> cd ~/..
> pwd
/Users
```

> **_EXERCISE_**
>
> Use the `cd` command to navigate to several different directories in your system.  Use relative paths, and experiment with using various combinations of the `.`, `..`, and `~` symbols.  (Experiment without fear!  You can't break anything with the `cd` command.)

#### Paths and Spaces

Some file and directory names contain spaces, which can cause problems for shells because spaces separate the different parts of a shell command.

For example, the command `cd /www/about me` would not work because the shell would interpret this as a command with two separate arguments, `/www/about` and `me`.

To avoid this problem, spaces in paths must be 'escaped' using the `\` symbol.  (Note that this is *not* the directory separator symbol `/`.)

So the above command could be written correctly as

```bash
cd /www/about\ me
```

When the shell encounters the `\` symbol before a space it will interpret that to mean that the space is part of the path rather than a separator of different parts of the command.

> **_EXERCISE_**
> 
> Use the `cd` command to navigate to a directory in your system that contains a space in its name

#### Paths with Wildcards (AKA Globs)

You have already encountered the `ls` command to list files.  You should now be able to list files in directories using either an absolute or relative path.

Examples:

```bash
# List files in the parent directory
ls ..
# List files in the user home directory
ls ~
# List files in the images folder inside the current working directory
ls ./images
# List the files in the directory three directories above the current working directory
ls ../../../
```

There is one other useful way of writing paths that can be used to specify multiple files or directories in a single path:  a wildcard, or 'glob pattern'.

The `*` symbol can be used in a path to represent any whole name or part of a name.

For example, the path `*` represents *all* the files in the current working directory.  The path `*.png` represents all the files with the extension `.png` in the current working directory.

Thus, a command like `ls *` will list all the files in the current working directory **and** all the files in the directories inside the current working directory.  Go ahead and try it in your shell!

And a command like `ls *.png` will list all the files with the extension `.png` in the current working directory.

A common way to list just the files and not the directories inside a particular directory is a command like `ls *.*`.  (But **be careful**: the `*.*` glob selects any directory or file names that have a `.` somewhere in the name, so this will not work if you have directories with a `.` in the name.)

## Environment Inspection

### Environment Variables

Your operating system and many of the programs on your computer rely on 'environment variables' for some configuration.  Environment variables are simply named values.  

For example, the `PATH` environment variable is a list of paths that the operating system will search to find executable commands or programs.  When you enter a command in the shell, if a program with that name does not exist in one of the directories in `PATH` then the command will fail.

> **_NOTE:_** Yes, that's right—each command you use in a shell is simply a small stand-alone program!  You can see which dierctory the program is in by using the `which` command, as in `which cd` or `which ls` or even `which which` :-)

There are many other environment variables that you will encounter as a programmer or computer administrator, but for now we'll leave them for you to discover!

### Setting Environment Variables

You may set the value of an environment variable in a shell by typing the name of the environment variable followed immediately (no spaces) by an `=` followed immediately (again, no spaces!) by the value.

For example, 

```bash
> MY_ENV_VAR=somevalue
> echo $MY_ENV_VAR
somevalue
> MY_ENV_VAR="a value with spaces needs quotes"
```

You can also use the value of other environment variables by prefixing the other variable's name with a `$`.  For example, the command below sets an environment variable named `MY_ENV_VAR` to have the same value as the `PATH` environment variable.

```bash
> MY_ENV_VAR=$PATH
```

You can even embed the value of other variables within some text, like so:

```bash
> MY_NAME=Ali
> MY_GREETING="Hi, my name is $MY_NAME."
> echo $MY_GREETING
Hi, my name is Ali!
```

### echo

The `echo` command is useful for examining the values of environment variables.  The basic functionality of `echo` is to produce (or 'echo') output to the shell.  For example

```bash
> echo Hi!
Hi!
> echo "Use quotes to echo text with spaces"
Use quotes to echo text with spaces
```

That example is obviously not very useful. However, consider the following shell session instead:

```bash
> echo $PATH
/usr/local/opt/openssl@1.1/bin:/usr/local/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Applications/Wireshark.app/Contents/MacOS
```

When you add a `$` in front of the name of an environment variable, `echo` will echo the the value of that environment variable to the shell!  (The command `echo PATH` would simply produce the text `PATH` on the shell.)

## File System Inspection

There are a few commands aside from `ls` that are useful for inspecting files and their contents.

### cat

The `cat` command can be used to con**cat**enate the contents of any number of files and print them to the shell.

Example:

```bash
> cd /www
> cat index.html
<!DOCTYPE html>
<html>
<head><title>My Website</title></head>
<body>Hello, world!</body>
</html>
```

The following command would print the contents of all `.html` files in the `/www` directory:

```bash
cat /www/*.html
```

> **_NOTE:_**  
> If you are wondering why this command isn't called `con`, you are not alone.  The reason is that when `cat` was first created there was already another command with the name `con`.  The `con` command is no longer a part of most Unix-style shells, but the `cat` command is still a useful tool today and has kept its name.

### less

The `cat` command usually prints the entire contents of a file to the shell.  In some environments (eg. a 'headless' server in which the only interface is a CLI shell) there is no way to 'scroll back' in the shell to view previous output.

The `less` command can be used to view the contents of a file or other output, and it has the ability to navigate both forward and backward within the file.

In this section you will learn how to use `less` to view the contents of a file.  See the [Redirecting Output](#redirecting-output) section for how to use `less` to view the output of other commands.

As an example, suppose the `/www/index.html` file was a largish file that does not fit in a single shell screen.  The following command could be used to view its contents:

`less /www/index.html`

Once the command is running, it will display only the first page of lines but will not exit.  Instead, you may press the `j` 
key to move down one line.  Pressing the `k` key will move back up one line (unless you are already at the top of the file).

> **_NOTE:_** Why `j` and `k` for up and down?  It is an ergonomic choice: when you are touch typing, your right index finger is on the `j` key and the main thing you want to do in `less` is move down the output.  Similarly, the `k` key is under your right middle finger.  Thus, you do not have to move your hand to move up and down in the output.  

If you would like to skip forward or back by a whole page, press `Ctrl+f` or `Ctrl+b`, respectively.

You can also search for specific words in the output.  When `less` is running, press the `/` key and type your search term then press `enter` (or `return` depending on your keyboard).  `less` will automatically move to and highlight the first search result.  You can navigate forward and back through the search results by pressing `n` (for 'next') and `N` respectively

To exit `less`, press `q` (quit).

> **_NOTE:_** You might be wondering about the reason for the name `less`.  There is also a command called `more` which existed before `less` and works much the same as `less` except it does not allow reverse movement through the output.  The name `more` referred to the ability to manually load 'more' of the output into the shell; the name `less` was a silly play on the name `more`.

### head & tail

The `head` and `tail` commands can be used to peek at the first or last lines of a file, respectively.  The number of lines can be controlled using the `-n` option, as in:

```bash
> cd /www
> head -n3 index.html
<!DOCTYPE html>
<html>
<head><title>My Website</title></head>
>
```

> **_NOTE:_** If the `-n` option is not specified, the default number of lines is usually 10

The `-f` option of the `tail` command is particularly useful for inspecting log files that may be getting written to continuously:

`tail -f somelog.txt`

This command will prevent the tail command from stopping when it hits the end of the file.  Instead, it keeps running, and any new text added to the file will get printed to the shell in real time!  To stop a `tail -f` press `Ctrl+c`.

### nano

The `nano` command loads a simple CLI text editor with which you can edit plain text files.  It is beyond the scope of this tutorial to explain in depth how to use nano.  The bottom of the nano user interface shows the commands that can be used within nano.  For example: `^X Exit`.  The `^` symbol in nano means the `Ctrl` button, so to exit nano press `Ctrl+x`.

> **_NOTE:_** You will probably encounter two other command line text editors as you learn to use Unix-style shells: `emacs` and `vi`.  Both are excellent tools to learn, but they have steep learning curves that are beyond the scope of this tutorial.  They are also the source of endless online debates about which is superior.  Nobody argues that `nano` is a great text editor, but it is easy to use and it gets the job done :-) 

> **_EXERCISE_**
>
> - Use the `cat`, `less`, `head`, `tail`, and `nano` commands to examine the contents of several files on your computer.
> - Open two separate shell windows/tabs.  In one, use `nano` to create a new file.  Add some text to this file and save it (but do not close `nano`).  In the other shell, use the `tail` command with the `-f` option to watch the end of the file you just created.  Now go back to your `nano` shell and repeatedly add new lines to your file then save.  You should see the contents of your file printed in your `tail` shell each time you save!

## Redirecting Output

### Redirecting to a File

Sometimes commands can produce a lot of output.  It can be useful to save this output in a file instead of simply printing it to the shell.

After *any* command, you can add the `>` symbol followed by a path to a file to store the output of that command to the specified file!

For example, the command below is one way to make a copy of the `/www/index.html` file:

```bash
cat /www/index.html > /www/index-copy.html
```

### Redirecting to Other Commands

You can also redirect the output of any command to any other command using the pipe symbol `|`.

For example, suppose you want to quickly examine the contents of a large file like `/www/index.html`.  The following command could be used:

```bash
cat /www/index.html | less
```

The first part of the command would usually cause the contents of `/www/index.html` to be printed to the shell, but here the `|` symbol causes the output to be 'piped' to `less` instead.

You might wonder why you would use the command above instead of one like `less /www/index.html`.  In fact, this latter way is probably the more direct approach in this case.

However, the usefulness of the pipe symbol becomes more obvious when you consider commands that are not simply printing the contents of a file.

For example, suppose you are trying to list the contents of a directory named 'tmp' with so many files in it that they do not all fit in one screen.  In this case, a command like

```bash
ls tmp
```

will produce too much output and you will only see the last set of files that fit in the shell screen.  But a command like

```bash
ls tmp | less
```

allows you to navigate and search through the list of files using `less`!

> **_NOTE:_**  The pipe symbol is a very powerful part of Unix-style shells.  Essentially any command output can be piped as the input for any other command.  Furthermore, pipes can be chained (as in `somecommand | anothercommand | another | yetanother`).  This allows you to compose multiple commands into a single entry in the shell to perform quite complex tasks! 


## File System Manipulation

### mkdir & rmdir

You can create a new directory using the `mkdir` (make directory) command with the name of the directory as the argument.

Example:

```bash
mkdir mynewdir
```

If (and only if!) a directory is empty, it can be removed using the `rmdir` command, as in

```bash
rmdir mynewdir
```

(See the `rm` command below for how to remove directories that are not empty.)

> **_EXERCISE_**
> 
> **_NOTE:_** This and the following exercises are meant to be completed as a set.  You will create and manipulate several files and directories.  By the end of the exercises your file system should be as it was before you started.
>
> Use appropriate commands to complete the following:
> - Create a new directory named `deleteme` inside your user home directory.  
> - Inside the `deleteme` directory, create three new directories named `d1`, `d2`, and `d3`.
> - Inside the `d1` directory, create a new directory named `sub`
> - Remove the `d3` directory
>
> If you have done this exercise correctly, you should have the following directory structure:
> ```
> ~
> +-- deleteme
>     +-- d1
>     |   +-- sub
>     |
>     +-- d2
> ```
>

### touch

The `touch` command can be used to create an empty file, as in

```bash
> cd ~
> ls
file1.txt file2.txt
> touch myfile.txt
> ls
file1.txt file2.txt myfile.txt
```

> **_EXERCISE_**
> 
> Use appropriate commands to complete the following:
> - Inside your `~/deleteme/d1` directory, create two empty files named `f1.txt` and `f2.txt`
> - Inside your `~/deleteme/d1/sub` directory, create a file named `f3.txt`
>
> If you have done this exercise correctly, you should have the following directory structure:
> ```
> ~
> +-- deleteme
>     +-- d1
>     |   +-- sub
>     |   |   +-- f3.txt
>     |   |
>     |   +-- f1.txt
>     |   +-- f2.txt
>     |
>     +-- d2
> ```
>

### cp

You can copy files using the `cp` command, which requires two arguments: a source and a destination.

For example:

```bash
> cd ~
> ls
file1.txt file2.txt
> cp file1.txt ./file1-copy.txt
> ls
file1.txt file1-copy.txt file2.txt
```

If the destination is a directory other than the current working directory, then the file will be copied with the same name to the destination directory.

Example:

```bash
> cd ~
> ls
file1.txt file2.txt
> ls ..  # no files in parent directory

> cp file1.txt ..
> ls ..
file1.txt
```

Globs can be used to copy multiple files at once:

```bash
> cd ~
> ls
file1.txt file2.txt
> ls ..  # no files in parent directory

> cp *.txt ..
> ls ..
file1.txt file2.txt
```

The `-R` option can be used to copy entire directory structures recursively.  The following command copies the `/User/ali` directory and all its contents into a directory named `/ali-backup`

```bash
> cp -R /Users/ali /ali-backup
```

> **_EXERCISE_**
> 
> Use appropriate commands to complete the following:
> - Copy all three of the text files you have created into your `d2` directory.  (Challenge: can you do it in a single command?)
> - Make a copy of `~/deleteme/d1/f2.txt` at `~/deleteme/d1/f4.txt`
> - Copy the contents of the `d1` directory into a new directory at `~/deleteme/d4`.
>
> If you have done this exercise correctly, you should have the following directory structure:
> ```
> ~
> +-- deleteme
>     +-- d1
>     |   +-- sub
>     |   |   +-- f3.txt
>     |   |
>     |   +-- f1.txt
>     |   +-- f2.txt
>     |   +-- f4.txt
>     |
>     +-- d2
>     |   +-- f1.txt
>     |   +-- f2.txt
>     |   +-- f3.txt
>     |
>     +-- d4
>     |   +-- sub
>     |   |   +-- f3.txt
>     |   |
>     |   +-- f1.txt
>     |   +-- f2.txt
>     |   +-- f4.txt
> ```
>

### mv

You can move files to new locations using the `mv` command with two arguments:  the current path, and the desired path.

For example, the command `mv file1.txt ..` moves `file1.txt` into the parent directory.

#### Renaming Using mv

You can also rename files using the `mv`:

```bash
> cd ~
> ls
file1.txt file2.txt
> mv file1.txt file1renamed.txt
> ls
file1renamed.txt file2.txt
```

Or, you can move and rename a file simultaneously.  The following command moves the file named `file1.txt` into the `/www` directory with and renames it to `file.txt`:

```bash
mv file1.txt /www/file.txt
```

Moving and renaming can also be done with directories.  Unlike the `cp` command, no `-R` option is necessary to move an entire directory and its contents.  For example, the following command renames the `/www/about me` directory to `about-me`

```bash
mv /www/about\ me /www/about-me
```

(Note the use of the escaped space in the source path.  See [Paths and Spaces](#paths-and-spaces) above.)

> **_CAUTION:_** If you specify a destination file name that already exists, the `mv` command will overwrite that file without warning!
>
> For example, consider the following shell session:
>```bash
>> cd ~
>> ls
>file1.txt file2.txt
>> mv file1.txt file2.txt
>> ls
>file2.txt
> # The original file2.txt was overwritten!  Oops!
>```

Finally, you can use globs to move multiple files at once.  The following command moves all files ending with `.txt` into the `/www` directory:

```bash
mv *.txt /www
```

> **_EXERCISE_**
> 
> Use appropriate commands to complete the following:
> - Rename the `~/deleteme/d4` directory to `~/deleteme/d3`
> - Rename the `~/deleteme/d1/f4.txt` file to `f5.txt`
> - Move all the `.txt` files in `~/deleteme/d1` into the `d1/sub` directory.
>
> If you have done this exercise correctly, you should have the following directory structure:
> ```
> ~
> +-- deleteme
>     +-- d1
>     |   +-- sub
>     |       +-- f1.txt
>     |       +-- f2.txt
>     |       +-- f3.txt
>     |       +-- f5.txt
>     |
>     +-- d2
>     |   +-- f1.txt
>     |   +-- f2.txt
>     |   +-- f3.txt
>     |
>     +-- d3
>     |   +-- sub
>     |   |   +-- f3.txt
>     |   |
>     |   +-- f1.txt
>     |   +-- f2.txt
>     |   +-- f4.txt
> ```
>

### rm

The `rm` command can be used to remove files and directories.

> **_CAUTION:_** Files removed using `rm` will *not* appear in your system's "trash can" or "recycle bin", meaning that an `rm` cannot be undone.  So be very careful when using this command!
>
> If you wish to be cautious, you can use the `-i` (interactive) option, which will cause the command to prompt for confirmation before each file it removes

Here are some typical `rm` commands.

Remove a specific file:

```bash
rm file.txt
```

Remove a list of files:

```bash
rm file1.txt file2.txt file3.txt
```

Remove a list of files using a glob (this command removes all `.txt` files in the working directory):

```bash
rm *.txt
```

Remove a directory and all its contents using the `-r` (recursive) option:

```bash
rm -r mydir
```

> **_EXERCISE_**
> 
> Use appropriate commands to complete the following:
> - Remove the `d3/f4.txt` file
> - Remove all the `.txt` files in the `d2` directory
> - Remove the `~/deleteme` directory and all its contents!
>
> If you have done this exercise correctly, you should no longer have any of the files you created in these exercises.  Squeaky clean!

## The End!

Congratulations, you now have the basic skills necessary to use a CLI shell!  You have completed an important step toward becoming an efficient computer user.