---
title: "Installing Software"
teaching: 5
exercises: 5
questions:
- How do I manage a Virtual Machine
- How I can I install missing software on a Linux Virtual Machine
objectives:
- Know the difference between a regular user and a *super user*
- Understand the concept of a package manager
- Search for some packages with `apt search`
keypoints:
- Have a general idea how additional software can be installed on a Linux machine
---

## Super Users and the `sudo` command

If you use Mac OS or Windows, you may have noticed that sometimes when you install software  or change something on your computer, you have to confirm that you want to do so with an additional security prompt (on Mac OS you have to type in your password).
The `sudo` command is the same idea. If you are trying to make some changes to your system and get the error that your account does not have enough privileges, you should try rerunning the command with
`sudo` in front of it to run it with administrator privileges.

It is a very bad idea to work with administrator privileges by default: if you make a mistake, such as typing in an incorrect `rm` or `mv` command, you may break the system entirely. 
It is therefore a good idea to only use the administrator privileges when you really need them.
Furthermore, if you create files as an administrator, you may not be able to access them as a
regular user without change the ownership and/or permissions on that file, which can be
cumbersome.

## Installing packages


Even as a regular user, you can use the package manager to search for packages that you may want to install. For example, suppose you are 
interested in using Octave, which is an Open Source package that is
very similar to MATLAB and even able to run many basic MATLAB scripts.

If we try to type 

```
$ octave
```
{: .language-bash }

We will get something like the following error:

```
Command 'octave' not found, but can be installed with:

sudo apt install octave
```
{: .error }

As we can see, Ubuntu recognized that there is a package that would provide the `octave` software, but that it is currently not installed.
It suggest to do so with the `apt install` command, preceded by `sudo`
as installing software requires elevated system permissions.

The command `apt` is a package manager for Debian and Ubuntu based Linux
distributions. A *package manager* is a somewhat similar to an app store application you may know from you smart phone. It allows you to easily install software you want.

Even if you do not have administrator rights on a system, you can still use `apt` to search which packages are there. Let's try and search for the `octave` package:

```
$ apt search octave
```

Since this shows a lot of packages that all contain the word `octave`, you can consider running it with `less` to be
able to browser the output easier:

```
$ apt search octave | less
```

You can close the `less` tool by pressing the <kbd>Q</kbd> button.

As you can see, the *main* package for Octave is called `octave`. If you would have
super-user rights on the virtual machine, which you **do not have during the workshop**,
but would have if you create your own Debian or Ubuntu virtual machine, you could install
the package by running 

```
$ sudo apt install octave
```
{: .language-bash }

Sometimes the software you want is not available in the standard package repository, or
the software in the standard repository is too old for your needs. In some cases, the
developers of software have additional repositories available which you can use. This
is for example the case for R. To be able to install it, you can first tell the `apt`
package manager to add an additional repository and corresponding security certificate
(**this is not possible during the workshop**):

```
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
$ sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/" 
```
{: .language-bash }

After that, you should be able to install a (modern) version of R using the package manger

```
$ sudo apt install r-base
```
{: .language-bash }

The general advice is too look on the website of the programming environment you want
to use, to see how it can be installed in a Linux context. Often multiple options are
given, but the option that works with a package manager is often easier than alternatives,
such as compiling and installing the software from source code.



