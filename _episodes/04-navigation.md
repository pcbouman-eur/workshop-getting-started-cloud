---
title: "Moving around and looking at things"
teaching: 15 
exercises: 5
questions:
- How do I navigate and look around the system?
objectives:
- Learn how to navigate around directories and look at their contents
- Explain the difference between a file and a directory.
- Translate an absolute path into a relative path and vice versa.
- Identify the actual command, flags, and filenames in a command-line call.
- Demonstrate the use of tab completion, and explain its advantages.
keypoints:
- Your current directory is referred to as the working directory.
- To change directories, use `cd`.
- To view files, use `ls`.
- You can view help for a command with `man command` or `command --help`.
- Hit <kbd>Tab</kbd> to autocomplete whatever you're currently typing.
---

> ## Important Tips
> In this episode you start working with the bash shell. There are two
> very useful key combinations to know before you start.
>
> * You can go back to (and edit) the previous commands you typed with <kbd>&uarr;</kbd>. If you go too far in history, you can move forward in history with <kbd>&darr;</kdb>. Using these keys well will avoid a lot of type, in particular if you introduce typos, or want to make small adjustments to a command!
> * Commands and filenames can often be autocompleted by pressing <kbd>Tab</kbd>. If you have to type in the name of a file `this_is_a_super_long_filename_i_would_hate_to_have_to_type_it_all`, you can type a small part of it, e.g. `this` and press tab. Typically, `bash` will auto-complete the filename for you, if this is possible. If there is some ambiguity, it will only autocomplete up to the part where there is no ambiguity. Try pressing <kbd>Tab</kbd> when you can, and you'll get the hang of it!
> * In many terminal applications, pasting is done by doing a *right-click* with your mouse, so the standard shortcut <kbd>CTRL</kbd>+<kbd>V</kbd> is often not necessary. Similarly, copying text to the clipboard is often done by just selecting a piece of text in the terminal, and the standard shortcut <kbd>CTRL</kbd>+<kbd>C</kbd> is often not necessary.
>
{: .prereq }


At this point in the lesson, we've just logged into the system. Nothing has
happened yet, and we're not going to be able to do anything until we learn a
few basic commands. By the end of this lesson, you will know how to "move
around" the system and look at what's there.

Right now, all we see is something that looks like this (assuming `test01` 
is the username and `Cloud-Workshop-VM` is the hostname of the machine):

~~~
test01@Cloud-Workshop-VM:~$
~~~
{: .language-bash}

The dollar sign is a **prompt**, which shows us that the shell is waiting for
input; your shell may use a different character as a prompt and may add
information before the prompt. When typing commands, either from these lessons
or from other sources, do not type the prompt, only the commands that follow
it.

Type the command `whoami`, then press the Enter key <kbd>&crarr;</kbd> (sometimes marked Return)
to send the command to the shell. The command's output is the ID of the current
user, i.e., it shows us who the shell thinks we are:

~~~
$ whoami
~~~
{: .language-bash}
~~~
yourUsername
~~~
{: .output}

More specifically, when we type `whoami` the shell:

1.  finds a program called `whoami`,
2.  runs that program,
3.  displays that program's output, then
4.  displays a new prompt to tell us that it's ready for more commands.

Next, let's find out where we are by running a command called `pwd` (which
stands for "print working directory"). ("Directory" is another word for
"folder"). At any moment, our **current working directory** (where we are) is
the directory that the computer assumes we want to run commands in unless we
explicitly specify something else. Here, the computer's response is `/home/yourUsername`,
which is ``yourUsername`` **home directory**. Note that the location of your home directory may differ from
system to system.

~~~
$ pwd
~~~
{: .language-bash}
~~~
/home/yourUsername
~~~
{: .output}

So, we know where we are. How do we look and see what's in our current 
directory?
```
$ ls
```
{: .language-bash}

`ls` prints the names of the files and directories in the current directory in
alphabetical order, arranged neatly into columns.

```
examples  welcome.txt
```
{: output}

If nothing shows up when you run `ls`, it means that nothing's there. Let's
make a directory for us to play with.

`mkdir <new directory name>` makes a new directory with that name in your
current location. Notice that this command required two pieces of input: the
actual name of the command (`mkdir`) and an argument that specifies the name of
the directory you wish to create.

```
$ mkdir documents
```
{: .language-bash}

Let's use `ls` again. What do we see?

Our folder is there, awesome. What if we wanted to go inside it and do stuff
there? We will use the `cd` (change directory) command to move around. Let's
`cd` into our new documents folder.

```
$ cd documents
$ pwd
```
{: .language-bash}
```
~/documents
```
{: .output}

What is the `~` character? When using the shell, `~` is a shortcut that
represents `/home/yourUserName`.

Now that we know how to use `cd`, we can go anywhere. That's a lot of
responsibility. What happens if we get "lost" and want to get back to where we
started?

To go back to your home directory, the following three commands will work:

```
$ cd /home/yourUserName
$ cd ~
$ cd
```
{: .language-bash}

A quick note on the structure of a UNIX (Linux/Mac/Android/Solaris/etc)
filesystem. Directories and absolute paths (i.e. exact position in the system)
are always prefixed with a `/`. `/` by itself is the "root" or base directory.

Let's go there now, look around, and then return to our home directory.

```
$ cd /
$ ls
$ cd ~
```
{: .language-bash}
```
bin   etc   lib64       mnt   root  snap  tmp  vmlinuz
boot  home  lost+found  opt   run   srv   usr  vmlinuz.old
dev   lib   media       proc  sbin  sys   var
```
{: .output}

The "home" directory is the one where we generally want to keep all of our
files. Other folders on a UNIX OS contain system files, and get modified and
changed as you install new software or upgrade your OS.

> ## Difference between Windows and UNIX
> #### No Drive Letters
> 
> The Folder structure on UNIX system is a bit different from the Windows
> structure. The are no drive letters such as `C:` or `D:` for different
> drives, but all paths start with `/`. If you use Linux on a desktop
> computer, a connected USB stick would typically be accessed via a path
> such as `/media/usb/` rather than via a drive letter.
>
> ---
>
> #### Upper and Lower Case names are different
> On UNIX based systems, file names are *case-sensitive*, which means that
> the upper-case version of a letter is considered different from the lower-case
> version. That means that on a UNIX system, a folder can contain two separate
> files name `myFile` and `MyFile`. On Windows, those two filenames would be
> considered equal, and could not co-exist in the same folder. 
{: .callout}

There are several other useful shortcuts you should be aware of.

- `.` represents your current directory
- `..` represents the "parent" directory of your current location
- While typing nearly *anything*, you can have bash try to autocomplete what
  you are typing by pressing the <kbd>Tab</kbd> key.

Let's try these out now:

```
$ cd ./documents
$ pwd
$ cd ..
$ pwd
```
{: .language-bash}

```
/home/yourUserName/documents
/home/yourUserName
```
{: .output}

Many commands also have multiple behaviours that you can invoke with command
line 'flags.' What is a flag? It's generally just your command followed by a
'-' and the name of the flag (sometimes it's '--' followed by the name of the
flag). You follow the flag(s) with any additional arguments you might need.

We're going to demonstrate a couple of these "flags" using `ls`.

Show hidden files with `-a`. Hidden files are files that begin with `.`, these
files will not appear otherwise, but that doesn't mean they aren't there!
"Hidden" files are not hidden for security purposes, they are usually just
config files and other tempfiles that the user doesn't necessarily need to see
all the time.

```
$ ls -a
```
{: .language-bash}
```
.  ..  .bash_logout  .bash_profile  .bashrc  documents  .emacs  .mozilla  .ssh
```
{: .output}

Notice how both `.` and `..` are visible as hidden files. Show files, their
size in bytes, date last modified, permissions, and other things with `-l`.

```
$ ls -l
```
{: .language-bash}
```
drwxr-xr-x 2 yourUsername tc001 4096 Jan 14 17:31 documents
```
{: .output}

This is a lot of information to take in at once, but we will explain this
later! `ls -l` is *extremely* useful, and tells you almost everything you need
to know about your files without actually looking at them.

We can also use multiple flags at the same time!

```
$ ls -l -a
```
{: .language-bash}

```
$ ls -la
total 36
drwx--S--- 5 yourUsername tc001 4096 Nov 28 09:58 .
drwxr-x--- 3 root         tc001 4096 Nov 28 09:40 ..
-rw-r--r-- 1 yourUsername tc001   18 Dec  6  2016 .bash_logout
-rw-r--r-- 1 yourUsername tc001  193 Dec  6  2016 .bash_profile
-rw-r--r-- 1 yourUsername tc001  231 Dec  6  2016 .bashrc
drwxr-sr-x 2 yourUsername tc001 4096 Nov 28 09:58 documents
-rw-r--r-- 1 yourUsername tc001  334 Mar  3  2017 .emacs
drwxr-xr-x 4 yourUsername tc001 4096 Aug  2  2016 .mozilla
drwx--S--- 2 yourUsername tc001 4096 Nov 28 09:58 .ssh
```
{: .output}

Flags generally precede any arguments passed to a UNIX command. `ls` actually
takes an extra argument that specifies a directory to look into. When you use
flags and arguments together, the syntax (how it's supposed to be typed)
generally looks something like this:

```
$ command <flags/options> <arguments>
```
{: .language-bash}

So using `ls -l -a` on a different directory than the one we're in would look
something like:

```
$ ls -l -a ~/documents
```
{: .language-bash}

```
drwxr-sr-x 2 yourUsername tc001 4096 Nov 28 09:58 .
drwx--S--- 5 yourUsername tc001 4096 Nov 28 09:58 ..
```
{: .output}

## Where to go for help?

How did I know about the `-l` and `-a` options? Is there a manual we can look
at for help when we need help? There is a very helpful manual for most UNIX
commands: `man` (if you've ever heard of a "man page" for something, this is
what it is).

```
$ man ls
```
{: .language-bash}

```
LS(1)                          User Commands                          LS(1)

NAME
     ls - list directory contents

SYNOPSIS
     ls [OPTION]... [FILE]...

DESCRIPTION
     List  information  about the FILEs (the current directory by default).
     Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

     Mandatory arguments to long options are mandatory for short options too.
```
{: .output}

To navigate through the `man` pages, you may use the <kbd>&uarr;</kbd> and <kbd>&darr;</kbd> keys to
move line-by-line, or try the <kbd>&#9251;</kbd>(spacebar) and <kbd>b</kbd> keys to skip up and down by full
page. Quit the `man` pages by pressing <kbd>q</kbd>.

Alternatively, most commands you run will have a `--help` option that displays
addition information For instance, with `ls`:

```
$ ls --help
```
{: .language-bash}

```
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~

# further output omitted for clarity
```
{: .output}

> ## Unsupported command-line options
>
> If you try to use an option that is not supported, `ls` and other programs
> will print an error message similar to this:
>
> ~~~
> [remote]$ ls -j
> ~~~
> {: .language-bash}
> 
> ~~~
> ls: invalid option -- 'j'
> Try 'ls --help' for more information.
> ~~~
> {: .error}
{: .callout}

> ## Looking at documentation
>
> Looking at the man page for `ls` or using `ls --help`, what does the `-h`
> (`--human-readable`) option do?
{: .challenge}

> ## Absolute vs Relative Paths
>
> Starting from `/Users/amanda/data/`, which of the following commands could
> Amanda use to navigate to her home directory, which is `/Users/amanda`?
>
> 1. `cd .`
> 2. `cd /`
> 3. `cd /home/amanda`
> 4. `cd ../..`
> 5. `cd ~`
> 6. `cd home`
> 7. `cd ~/data/..`
> 8. `cd`
> 9. `cd ..`
>
> > ## Solution
> >
> > 1. No: `.` stands for the current directory.
> > 2. No: `/` stands for the root directory.
> > 3. No: Amanda's home directory is `/Users/amanda`.
> > 4. No: this goes up two levels, i.e. ends in `/Users`.
> > 5. Yes: `~` stands for the user's home directory, in this case 
> >    `/Users/amanda`.
> > 6. No: this would navigate into a directory `home` in the current directory
> >    if it exists.
> > 7. Yes: unnecessarily complicated, but correct.
> > 8. Yes: shortcut to go back to the user's home directory.
> > 9. Yes: goes up one level.
> {: .solution}
{: .challenge}

> ## Relative Path Resolution
>
> Using the filesystem diagram below, if `pwd` displays `/Users/thing`, what
> will `ls -F ../backup` display?
>
> 1.  `../backup: No such file or directory`
> 2.  `2012-12-01 2013-01-08 2013-01-27`
> 3.  `2012-12-01/ 2013-01-08/ 2013-01-27/`
> 4.  `original/ pnas_final/ pnas_sub/`
>
> ![File System for Challenge Questions](../fig/filesystem-challenge.svg)
>
> > ## Solution
> > 1. No: there *is* a directory `backup` in `/Users`.
> > 2. No: this is the content of `Users/thing/backup`,
> >    but with `..` we asked for one level further up.
> > 3. No: see previous explanation.
> > 4. Yes: `../backup/` refers to `/Users/backup/`.
> {: .solution}
{: .challenge}

> ## `ls` Reading Comprehension
>
> Assuming a directory structure as in the above Figure (File System for
> Challenge Questions), if `pwd` displays `/Users/backup`, and `-r` tells `ls`
> to display things in reverse order, what command will display:
>
> ~~~
> pnas_sub/ pnas_final/ original/
> ~~~
> {: .output}
>
> 1.  `ls pwd`
> 2.  `ls -r -F`
> 3.  `ls -r -F /Users/backup`
> 4.  Either #2 or #3 above, but not #1.
>
> > ## Solution
> >  1. No: `pwd` is not the name of a directory.
> >  2. Yes: `ls` without directory argument lists files and directories
> >     in the current directory.
> >  3. Yes: uses the absolute path explicitly.
> >  4. Correct: see explanations above.
> {: .solution}
{: .challenge}

> ## Exploring More `ls` Arguments
>
> What does the command `ls` do when used with the `-l` and `-h` arguments?
>
> Some of its output is about properties that we do not cover in this lesson
> (such as file permissions and ownership), but the rest should be useful
> nevertheless.
>
> > ## Solution
> >
> > The `-l` arguments makes `ls` use a **l**ong listing format, showing not
> > only the file/directory names but also additional information such as the
> > file size and the time of its last modification. The `-h` argument makes
> > the file size "**h**uman readable", i.e. display something like `5.3K`
> > instead of `5369`.
> {: .solution}
{: .challenge}

> ## Listing Recursively and By Time
>
> The command `ls -R` lists the contents of directories recursively, i.e.,
> lists their sub-directories, sub-sub-directories, and so on in alphabetical
> order at each level. The command `ls -t` lists things by time of last change,
> with most recently changed files or directories first. In what order does `ls
> -R -t` display things? Hint: `ls -l` uses a long listing format to view
> timestamps.
>
> > ## Solution
> >
> > The directories are listed alphabetical at each level, the
> > files/directories in each directory are sorted by time of last change.
> {: .solution}
{: .challenge}
