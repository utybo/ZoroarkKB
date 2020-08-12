---
articleid: gTERBA
category: general
title: Terminal Basics
authors:
  - Thomas 'Cobaltarrena' Crambert
  - Matthieu 'Zoroark' Stombellini
license: © 2020 Thomas Crambert & Matthieu Stombellini, tous droits réservés, all rights reserved
excerpt: Information for beginners in the terminal world
---

# Terminal Basics

This article explains the basics of using terminals and shells.

* toc
{:toc}

## Terminals? Shells?

Before diving into how to use terminals and shells, we need to clarify the difference between a terminal and a shell

A terminal is an application that "emulates" a console. The shell is the program that is ran inside the terminal. The shell only takes text input and outputs text: it does not care about keyboard presses, mouse movements, etc, it only takes text. The terminal is responsible for taking in what you type and what you're doing and sending that information as text to the shell, and also displays the text the shell sends out in a more organized way.

The shell is the "logical" side of things while the terminal is the "visual and interaction" side.

Note that you can change the shell used by your terminal. On the EPITA machines and in almost all Linux-based systems, the default will be `bash`.

Some shells include:

* sh
* bash
* zsh
* fish
* PowerShell
* cmd

Some terminals include:

* urxvt
* konsole
* GNOME Terminal
* iTerm
* Windows Terminal
* conhost (the window in which PowerShell and cmd are launched on Windows)

## Shells 101

Shells and terminals are a way of interacting with your computer. Instead of clicking around to navigate and launch applications, you write commands to change the folder you're in and launch programs. This is what we call the "Command Line Interface", or CLI for short.

### Pros and cons

You might be thinking "well, I can already do everything I want by clicking with my mouse on my desktop, why should I care about this whole terminal and shell story?"

There are a few reasons why using shell is a valid alternative. The main ones are: 

* You are faster and more productive when using a keyboard to do everything instead of moving your cursor around. 
* A CLI is more versatile: many tools do not have a graphical interface and can only be used using the command line. While third-party graphical tools exist, they don't usually cover 100% of the functionality offered by the command line version.
* You can use *scripting* to run a slew of commands automatically by launching a single file. You can think of scripts as tiny programs that do their magic by launching commands.

The Command Line Interface does have a few drawbacks compared to graphical interfaces you may be used to. The biggest problem is ease of use: it is harder to use a CLI than it is to use a graphical interface. Instead of having access to all of the available options by right-clicking or clicking in some menus, you have to memorize commands and flags, which can be daunting at first.

The CLI is also less visual, and it can be harder to determine whether what you just did worked or not and just *what* it did.

In the end, the CLI vs Graphical interface war is mostly a question of preference, but sometimes, you have no choice but to use a CLI, so it is important to know at least a little bit about it.

At EPITA, you will not be using the CLI a bit in your SUP year, especially for Git, OCaml and some file manipulations. You will be using it a lot more in SPE and ING1.

### The prompt

The first thing you'll be met with when launching a shell is what is called the "prompt". It will look similar to this:

```shell
matthieu@MY-LAPTOP:~/example$
```

This line gives you a lot of information, so let's break it down.

* `matthieu`: that is your username
* `MY-LAPTOP`: that is the name of the computer the shell is on. This information is important because shells can be accessed remotely (i.e. launching a shell on another computer or server and accessing it through your own computer, similar to a TeamViewer but for shells).
* `~/example`: that is your current working directory. We will see more about paths and files on Linux later on, for now, just remember that this is "where you currently are", similar to the current directory in a file explorer.

### Paths and files on Linux/UNIX

While other systems (especially Windows) have a separate "root directory" for each storage device (e.g. `C:\` and `D:\` on Windows), Linux and all UNIX-like systems have a single root directory, designated by a single forward slash `/`. 

From this root directory, you can access pretty much everything you want, including things that are not "files" in the traditional sense of the word: one of the core aspects of UNIX systems is that "everything is a file".

For example, on Windows, to view your CPU information, you would open some sort of software. Under the hood, the program you open will call a specific API or function within Windows to get that information.

On Linux and UNIX-like systems all of this information can be accessed using a file-like representation! You can get information about your CPU by simply checking the "file" `/proc/cpuinfo`.

> Try this in your terminal! Launch `cat /proc/cpuinfo` in your terminal. `cat` simply prints out the contents of the file in your terminal.

This file representation is very useful, because anything we can do on files like searching, replacing, moving... can also be done on a LOT of other things that are not really "files" but have a file-like representation.

#### Absolute and relative paths

There are multiple kinds of paths: *absolute* and *relative* paths are the main types you should keep in mind.

* Absolute paths are paths that start from the root directory. You can think of these as the "full" paths. For example, your home directory, which is where your personal files are stored, is `/home/username`, where `username` is your account's name. This means that, from the root directory `/`, you go to the directory named `home`, then in the directory named `username`. This path can be used no matter where you are, and will always point in the same directory.

* Relative paths are paths that start from the current working directory -- that is, the directory you are currently in. For example, say you are currently in your home directory `/home/username` and you want to go to the folder `/home/username/documents/texts`. Instead of writing the full, absolute path, you can just write `documents/texts`. You can think of relative paths as the combination of the current working directory + the path (e.g. the current working directory `/home/username` + the relative path you entered `documents/texts` = `/home/userame/documents/texts`). Moreover, if you want to point to a directory or file that is in the current working directory, you can just write its name (e.g. `myfile.txt`).

Whenever a command is expecting a file or directory, you can use an absolute path or a relative path.

#### Special path characters

There are also special shortcuts you can use in paths:

* `..` denotes the parent folder of the current folder. For example, `/home/username/..` is the same as `/home`
* `.` denotes the current working directory. It is not usually directly used in path. However, you need to use it for launching executable files that are located in the current working directory, such as scripts -- you will run something like `./myscript.sh`. You cannot run `myscript.sh` directly, as `bash` would understand it as a command name instead of a file. `.` is also useful when using commands where you want to point to the current working directory.
* `~` denotes your home directory. You can use it only at the beginning of a path. Note that this is not an actual path feature: it is a shortcut offered by your shell called "tilde expansion", and can actually do quite a bit more. [You can read more about tilde expansions in `bash` here.](http://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html)

```shell
~/example/test$ cd ..
~/example$ cd ~/work/my-tp
~/work/my-tp$ cd ../other-things
~/work/other-things$
```

#### Filename expansion

Some characters can be used to point to multiple files at the same time, and can be considered as "wildcards":

* `?` means "any single character"
* `*` means "0 or more character", where the characters can be anything

This feature is called "filename expansion", and you can read more about it [here](https://www.gnu.org/software/bash/manual/bash.html#Filename-Expansion).

```shell
~/example$ ls
bread.txt  brett.txt  cat.txt  cathode.txt  catty.txt
~/example$ echo bre??.txt
bread.txt brett.txt
~/example$ echo cat*.txt
cat.txt cathode.txt catty.txt
```

In these examples, the shell "expands" the patterns. The command that is *actually* ran is respectively `echo bread.txt brett.txt` and `echo cat.txt cathode.txt catty.txt`.

**BE CAREFUL! Because file expansions simply adds all the files one after the other in the command, if one of the files begins with a `-`, it may be understood as a flag and may have disastrous consequences!**

#### Hidden files

Hidden files and hidden directories on UNIX systems start with a dot `.`. You will need to enable a special flag in some commands to be able to see them (for example, you need to use `ls -A` or `ls -a` instead of `ls` to see hidden files).

For example, a file is named `.gitignore` and a folder named `.config` are considered hidden directories because they start with a `.`.

#### Permissions

Linux and UNIX-like systems have a permissions system. A system has *users* and *user groups* (or just *groups* for short). Each user can be part of multiple groups.

Each file and directory is owned by a user and a group, and has three "levels" of permissions:

* The user owner's permissions (`u`)
* The group owner's permissions (`g`)
* Permissions for others (`o`)

There are three kinds of permissions:

* Read permissions `r`, needed to open and read files and directories
* Write permissions `w`, needed to change and create files and directories
* Execution permissions `x`, needed to run a program, file, etc.

You can check the groups you are part of by using `groups username`, replacing `username` by your account's username.

### Commands and running stuff

At its core, interacting with the shell is done by simply entering a text command and pressing Enter.

For example, open a terminal, and type `echo "Hello World!"`, then press enter. You should have something similar to this:

```shell
~/myfolder$ echo -E "Hello World!"
Hello World!
~/myfolder$
```

Congratulations, you just ran your first command!

Let's break down the command's structure.

* `echo`: that is the command name
* `-E`: this is a command [flag](#flags) that alters the effect of the command. In this case `-E` does not actually do anything (it is usually used for altering how `echo` treats input that contains backslash `\`)
* `"Hello World!"`: this is a command argument. In this case, it tells `echo` what to print out.

#### Flags

Flags can have two forms, a short form `-a` that starts with a single `-` and is a single character or a long form `--always-do-this` that is more explicit and starts with two `-`. The long form can be made of multiple words, in which cases `-` is used instead of a space (always do this --> `--always-do-this`).

If you are using multiple short flags together, like `-a -b -c`, you can combine all of them in a single short flag, where each character denotes a short flag `-abc`.

Note that these rules do not always apply depending on the exact command. When in doubt, use the manual page to check this (`man command`, e.g. `man echo`).

### Basic shortcuts

These shortcuts can be used in your shell.

| Shortcut | Description |
|:--------:| -------- |
| `Up/down arrows` | Move through the input command history
| `Tab` | Autocompletion. Automatically fills in the command name, file name, argument name, etc.
| `Ctrl+A` |Move to the beginning of the input command
| `Ctrl+E` | Move to the end of the input command
| `Ctrl+C` | Kill the current process
| `Middle click` | Paste the selected text. This works for all applications on Linux, not just the terminal.
| `Ctrl+L` | Clear the screen from all the previous command and their results. Keeps the current line.
| `Ctrl+U` | Cut the part of the line before the cursor 
| `Ctrl+K` | Cut the part of the line after the cursor 
| `Ctrl+Y` | Paste the content in the clipboard
| `Ctrl+W` | Remove the word before the cursor
{:.kb-table}


<!-- List format
* `Up/down arrow key`: Move through the input command history
* `Tab`: autocomplete to the input command the common part to all the possible cases
* `Ctrl+A`: Move to the beginning of the input command
* `Ctrl+E`: Move to the end of the input command
* `Ctrl+C`: Kill the current process
* `Middle click`: Paste the selected text. This works for all applications on Linux, not just the terminal.
* `Ctrl+L`: Clear the screen from all the previous command and their results. Keeps the current line.
* `Ctrl+U`: Cut the part of the line before the cursor 
* `Ctrl+K`: Cut the part of the line after the cursor 
* `Ctrl+Y`: Paste the content in the clipboard
* `Ctrl+W`: Remove the word before the cursor
-->

## File management commands

These commands can be used to move around, list files, change files, etc.

### pwd

The `pwd` command (for "**p**rint **w**orking **d**irectory") prints the [absolute path](#paths) to the current working directory (i.e. the directory you are currently in).

```shell
~/example$ pwd
/home/matthieu/example
```

### ls

The `ls` (for "**l**i**s**t") command displays every element contained in the current  working directory.

```shell
~/example$ ls
 a_file.txt an_executable some_directory
```

The following flags can also be used:
* `-a`, `--all` : display all elements, including hidden ones. 
* `-A`: display all elements, including hidden ones, but exclude `.` and `..`
* `-1`: display one element per line.
* `-l`: display a more detailed list.

You can also run `ls` on a specific folder by giving it a path to the folder. For example:

```shell
~/example$ ls some_directory
contents.txt of_some_directory.png
```

This prints the contents of `~/example/some_directory`.

Using the flag `l` will give you more details:

```shell
~/example$ ls -l
total 184
drwxr-xr-x 2 matthieu matthieu   4096 Aug 12 16:43 a_directory
-rw-r--r-- 1 matthieu matthieu     85 Aug 12 16:44 contents.txt
-rw-r--r-- 1 matthieu matthieu 176784 Aug 12 16:45 of_some_directory.png
```

The first `total` line represents the number of blocks this directory is taking on the storage device.

The list contains the following information:

* The file type as a single character. This will typically be `d` for a directory or `-` for a regular file.
* The "mode bits" (that is, the permissions settings for this directory or file). This is a sequence of `rwx` (read, write, execution[^ls1]) repeated three times, first for the owning user, then the owning group, then for others. If the permission is *not* granted, a `-` is shown instead. See [here](#permissions) for explanations on permissions.
* The number of hard links to the file or directory ([see here for details](https://unix.stackexchange.com/a/43047))
* The name of the user owner
* The name of the group owner
* The size on disk of the file or directory (in bytes)
* The last modification date
* The name of the file or directory

[^ls1]: This is not only an execution character and can instead be something else: the values "s", "S", "t", "T" are also possible under certain scenarios. Just know that if it is a lowercase character, it means `x` + something else. [See here for more information.](https://www.gnu.org/software/coreutils/manual/html_node/What-information-is-listed.html#index-permissions_002c-output-by-ls:~:text=The%20file%20mode%20bits%20listed%20are,each%20set%20of%20permissions%20as%20follows%3A)

### cd

The `cd` (for "**c**hange **d**irectory") command replaces the current working directory by the one given as argument.

```shell
~/example$ cd some_directory
~/example/some_directory$ 
```

To move up from one directory, [use `..` as the argument](#special-path-characters).

```shell
~/example/some_directory$ cd ..
~/example$ 
```

It is also possible to run `cd` with absolute paths. For example:

```shell
~/example$ cd /home/itsame/example/some_directory
~/example/some_directory$
```

Finally, running the `cd` command with no argument is the same as running `cd ~`

### touch

The `touch` command modifies the last access date of the file passed as an argument (the "Last accessed" date). It can also be used to create files in a directory: in case you try to `touch` something that does not exist, a file with that name is created instead.

```shell
~/example$ ls
a_file.txt directory_yes
~/example$ touch another_file.txt another_file_again.txt
~/example$ ls
a_file.txt another_file.txt another_file_again.txt directory_yes
```

Here, the files `another_file.txt` and `another_file_again.txt` were created.

The files created are empty.

### mkdir

The `mkdir` command (for "**m**a**k**e **dir**ectory") creates a new directory in the current working directory.

```shell
~/example$ ls
a_directory a_file.txt 
~/example$ mkdir another_directory
~/example$ ls
a_directory a_file.txt another_directory
```

### rm

The `rm` command (for **r**e**m**ove) removes the file(s) given as an argument. Multiple files can be removed at once.

**Removing files with `rm` cannot be undone.**

```shell
~/example$ ls
a_directory a_file.txt another_directory
~/example$ rm a_file.txt
~/example$ ls
a_directory another_directory
```

To remove a directory, the flag -r or --recursive has to be used.

```shell
~/example$ ls
a_directory another_directory
~/example$ rm -r another_directory
~/example$ ls
a_directory 
```

To ensure the deletion of every element, the flag -f or --force as to be used

### mv

The `mv` command (for **m**o**v**e) moves the file or the directory passed as the first argument to the path given by the second argument.

```shell
~/example$ ls
a_directory a_file.txt
~/example$ mv a_file.txt a_directory
~/example$ ls
a_directory 
~/example$ ls a_directory
a_file.txt some_already_existing_directory
```

It is possible to move multiple files at once.

```shell
~/example$ ls
a_directory 1.txt 2.txt 3.txt
~/example$ mv 1.txt 2.txt 3.txt a_directory
~/example$ ls
a_directory 
~/example$ ls a_directory
1.txt 2.txt 3.txt some_already_existing_directory
```

This command can also be used to rename files and directories.

```shell
~/example$ ls
a_directory file_to_rename.txt
~/example$ mv file_to_rename.txt renamed.txt
~/example$ ls
a_directory  renamed.txt
```

### cp

The `cp` command (for **c**o**p**y) copies the content of the file given by the first argument and paste it to the directory passed as the first argument. The basic syntax is identical to `mv`, but files are copied instead of being moved.

### chmod

The `chmod` command (for **ch**ange **mod**e) modifies the permissions (or "mode") of the files given as arguments.

Read [this part](#permissions) for details on permissions.

The `chmod` command has multiple kinds of ways to determine the exact permissions that should be set.

#### Script execution

The number 1 reason to use `chmod` at EPITA is to make a script executable. On Linux and UNIX-like systems, you must have an execution permission to run a file (you will otherwise get a "Permission denied" message).

The `chmod +x file.txt` command marks the file `file.txt` as executable.

```shell
# Creates a file "my_script.sh" with "echo Success" in it
~/example$ echo "echo Success" > my_script.sh
~/example$ ls
my_script.sh
~/example$ ./my_script.sh
bash: ./my_script.sh: Permission denied
~/example$ chmod +x my_script.sh
~/example$ ./my_script.sh
Success
```

This is only an example of the "symbolic mode" syntax. Read on for more details.

#### Symbolic mode

This part will detail the syntax of the `chmod` command.

The `chmod` command takes in first a "symbolic mode" (a human-friendly representation of the permissions) and the file(s) and directory(ies) the permissions should be applied to.

The syntax of the symbolic mode is

* a set of who the mode should be applied to (zero or more of `u` for the user owner, `g` for the group owner, `o` for all others, `a` for everyone). If none is specified, `a` is taken by default
* `-` to remove the permissions, `+` to grant the permissions, `=` to set the permissions to exactly what is given
* the kind of permissions, which is one or more of `r`, `w` or `x`[^chmod1] OR another target (`u`, `g`, `o`) to use their permissions.

[^chmod1]: Other kinds of permissions are available, see [this](https://www.gnu.org/software/coreutils/manual/coreutils.html#Symbolic-Modes) for more details on symbolic modes.

You can specify multiple symbolic modes by putting a `,` between them.

For example `chmod u=rwx,go-w myfile.txt` sets the permissions for `myfile.txt` as follows:

* Set `=` the user owner `u` permissions allowing them to read from `r`, write to `w` and execute `x` the file
* Remove `-` the group owner `g` and others `o` permission to write to `w` the file.

Another example: `chmod u+wx,g+w,o=r`

* Add `+` the user owner `u` permissions allowing them to write to `w` and execute `x` the file
* Add `+` the group owner `g` permissions allowing them to write to `w` the file
* Set `=` others' `o` permissions allowing them to read from `r` the file

Another one: `chmod g=u`

* Set (`=`) the group owner (`g`) permissions to the permissions of the user owner (`u`)

#### Octal mode

The symbolic mode can also be expressed using a number in base 8. See [this](https://www.gnu.org/software/coreutils/manual/html_node/Numeric-Modes.html) for more details.

For example, the number `777` is equivalent to `a=rwx`.

#### Take the mode of another file/directory

You can also take the permissions of another file and apply them to other files. Instead of the mode, use `--reference=reference_file.txt`, replacing `reference_file.txt` by a path to the reference file.

### locate

The `locate` can be used to search for a file. It prints the path of the files that match the pattern given as an argument. 

```shell
~/example$ ls
a_directory file_to_rename.txt
~/example$ ls a_directory
a_file_to_locate.txt
~/example$ locate a_file_to_locate.txt
~/example/a_directory/a_file_to_locate.txt
```

### zip/unzip

The `zip` command is used to compress files into an archive to make them lighter, i.e. to make ZIP files. The first argument is the name of the archive and the others are the file(s) and folder(s) to compress.

To extract the content of an archive, use the `unzip` command  with the name of the archive as the argument. 

```shell
~/example$ ls
1st_file_to_compress.txt 2nd_file_to_compress.txt
~/example$ zip archive_example.zip 1st_file_to_compress.txt 2nd_file_to_compress.txt
~/example$ ls
1st_file_to_compress.txt 2nd_file_to_compress.txt archive_example.zip
~/example$ rm *.txt
~/example$ ls
archive_example.zip
~/example$ unzip archive.example
~/example$ ls
1st_file_to_compress.txt 2nd_file_to_compress.txt archive_example.zip
```

To unzip the content of an archive in a specific directory, use the `-d` flag followed by the path to said directory.

```shell
~/example$ ls
archive_example.zip
~/example$ unzip archive.example -d archive_content
~/example$ ls
archive_content archive_example.zip
~/example$ ls archive_content
1st_file_to_compress.txt 2nd_file_to_compress.txt
```

### tar

The `tar` command is another way to archive and compress files.

`tar` is generally preferred to `zip` on UNIX-like systems. While the ZIP format both archives (i.e. put multiples files in a single file) and compresses (i.e. make the resulting file smaller) automatically, the `tar` format by itself only archives. The resulting `tar` file is then compressed using another program (e.g. the `gzip` program), which results in an archived and compressed file.

You can generally guess the compression algorithm by just looking at the file's name. For example, a `.tar.gz` file was archived with `tar` and compressed with `gzip`.

The `tar` command can both archive and compress files with a single command. It internally just calls the relevant compression program after the archival is done.

#### Archiving and compressing

You can archive files using the following command:

```shell
~/example$ tar -cvf output.tar file1.txt file2.txt my_folder
```

The `-cvf` flag is a [shorter notation](#flags) for `-c -v -f`, where:

* `-c` (**c**reate) indicates that we want to create an archive
* `-v` (**v**erbose) indicates that `tar` should output more text. By default, the `tar` command is mostly silent.
* `-f` (**f**ile) indicates that we want to create the archive in a specific file. `-f` must be immediately followed by said file.

Note that this command does not compress the resulting file. To archive *and* compress the files, use:

```shell
~/example$ tar -cavf output.tar.gz file1.txt file2.txt my_folder
```

The additional flag `a` enables "auto compression". The compression program that will be used is determined from the output file's suffix. For example, if your output ends in `.gz`, the gzip compression program will be used.

See [here](https://www.gnu.org/software/tar/manual/html_node/gzip.html#auto_002dcompress) for a full list of supported suffixes.

#### Decompressing and extracting

The opposite operation is simply done with

```shell
~/example$ tar -xvf my_archive.tar.gz
```

This command works for any uncompressed and compressed archives, no matter which compression program you used.

The `x` option (e**x**tract) tells `tar` that we want to do an extraction operation.


## Tools and utility commands

### echo

The `echo` command prints the arguments you give it in the terminal.

```shell
~/example$ echo "Hello World!" "This is great!"
Hello World! This is great!
```

It can also be used to add some content to a file. If the given file does not exist, it is created.

```shell
~/example$ echo "Hello World! This is great!" > a_file.txt
~/example$ cat a_file.txt
Hello World! This is great!
```

(This syntax can actually be used for any command, not just `echo`)

You can also use the `-e` flag to enable some backslash notations. For example:

```shell
~/example$ echo -e "Hello World\n\nMultiple lines! Wow!"
Hello World

Multiple lines! Wow!
```

### man

The man command (for **man**ual) displays the corresponding page in the UNIX manual pager of a given command. It also works for functions from the C language.

```shell
~/example$ man ls
# full description on what exactly the ls command is and what its flags can do
```

### grep

The grep command (for "**g**lobally search for a **r**egular ***e***xpression and **p**rint matching lines") searches all the occurences of the given argument and print them. The given argument is either a string or a regular expression. 

```shell
~/example$ echo -e "one\ntwo\nthree"
one
two
three
~/example$ echo -e "one\ntwo\nthree" | grep 'two'
two
```

This command is **very useful** to search for something that is contained in huge log files or in another command.

### clear

The clear command clears the screen of all the previous commands that have been entered. It is similar to the `Ctrl+L` shortcut.

### cat 

The cat command (for **cat**enate, a synonym for concatenate) is mainly used to print the content of a file on the terminal

```shell
~/example$ echo "Hello World! This is great!" > a_file.txt
~/example$ cat a_file.txt
Hello World! This is great!
```

### top

The `top` (**t**able **o**f **p**rocess) command is a small "task manager" program that runs in your terminal. Type `q` to exit it.

Here's an example of running `top`:

```
top - 16:24:44 up 59 min,  0 users,  load average: 0.00, 0.00, 0.00
Tasks:   8 total,   1 running,   7 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  6406176 total,  6267292 free,    62512 used,    76372 buff/cache
KiB Swap:  2097152 total,  2097152 free,        0 used.  6179928 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
    1 root      20   0     892    548    480 S   0.0  0.0   0:00.02 init
    7 root      20   0     892     84     16 S   0.0  0.0   0:00.00 init
    8 root      20   0     892     84     16 S   0.0  0.0   0:00.18 init
    9 matthieu  20   0   66476   8336   4528 S   0.0  0.1   0:00.29 zsh
   33 matthieu  25   5   27320   3472   3224 S   0.0  0.1   0:00.00 zsh
   34 matthieu  25   5   74544   2884   2672 S   0.0  0.0   0:00.11 gitstatusd-linu
  102 matthieu  20   0   23008   4844   3320 S   0.0  0.1   0:00.18 bash
  179 matthieu  20   0   42104   3524   3060 R   0.0  0.1   0:00.00 top
```

Your `top` output will usually be way longer than this: this one is fairly short because it was run in WSL on Windows instead of a full Linux OS.

### sudo

The `sudo` command (for "**s**ubstitute **u**ser **do**") gives the command given as an argument the permission of the root user. Even if this command is disabled on the computers of the school, it is important to know it exists, especially for installing stuff on your own computer.

**Anything you type after this command will be ran as an administrator (i.e. as the root account), and can have disastrous consequences if you do not know what you are doing. Use it with care and only when absolutely necessary!**

```shell
~/example$ pacman -S some_package
error: you cannot perform this operation unless you are root 
~/example$ sudo pacman -S some_package
# working fine
```

## Going further

The following resources can be useful if you want to learn more about shells:

* [**Redirection**](https://linuxconfig.org/introduction-to-bash-shell-redirections): `|`, `>`, putting command outputs in files.
* [**Scripting**](https://en.wikibooks.org/wiki/Bash_Shell_Scripting): Automate all the things and make mini-programs using shell commands.

## Footnotes
