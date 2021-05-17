---
title: "(Option 1 - Python) Run your own code"
teaching: 15
exercises: 15
questions:
- How do I run Python programs?
- What options do I have to pass data to my programs?
objectives:
- Run Python programs from the command line
- Understand how a program can read what you type into the terminal
- Be able to pass arguments when you execute the program
- Make a flexible Python program that can except different arguments
keypoints:
- Your own Python programs can be run from the command line
- There are different option to pass data into your program
---

> ## Example code
>
> In this episode you will work with example code. During the
> workshop your instructor will have set up a user account
> that comes with the example code already provided.
> If you are not participating in the workshop, you can 
> [download an archive](../files/userbase.tar.gz)
> with all the files assumed to be present during this episode.
{: .prereq }

In this episode we will run our own code, written in Python,
from the command line. Let us suppose that we have written a
program that searches for integer number in a certain range
that have all the numbers in a list as their divisor.

For this we have written the following program a put it in a
file `divisors.py`. You can find this file in the directory
`~/examples/python`.

```
# These are the parameters
upper_bound = 100
divisors = [3, 5]

# Here we search for suitable numbers
result = []
for i in range(1,upper_bound):
   divisable = True
   for div in divisors:
      if i % div != 0:
         divisable = False
   if divisable:
      result.append(i)

print("The following numbers are divisible by all of the divisors")
print(result)
```
{: .language-python}

We will now consider how we can run this program from the command line.

## Running a Python program

Once we have a Python program in a file like this, it is actually not that
difficult to run it from the command line. We can start the Python interpreter
using the command `python3`.

```
$ python3
```
{: .language-bash}

which put us into an interactive mode. Here, we can evaluate Python code
by directly typing it into the interpreter. In fact, it works very similar
to the bash shell, except that it speaks a different language.

```
Python 3.6.9 (default, Jan 26 2021, 15:33:00)
[GCC 8.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> x = [1,2,3]
>>> x.append("hello")
>>> print(x)
[1, 2, 3, 'hello']
>>> exit()
```
{: .output }

While we could type the Python program line by line into this interactive mode,
this is not very convenient, so let's quit the interactive mode for now by
typing `exit()`.

If we type a filename that contains a Python script after the `python3` command,
it will execute that script directly, rather than use the interactive mode.
Let's try and do this for our program.

```
$ cd ~/examples/python
$ python3 divisors.py
```
{: .language-bash}

If all goes well, we should see the output of our program!

```
The following numbers are divisible by all of the divisors
[15, 30, 45, 60, 75, 90]

```
{: .output }

However, it might be nice if we can make the program a bit more flexible.
We will look at two ways to do so: using standard in, and via command line
arguments.

## Reading data typed into the terminal

While you learned programming, you may have written interactive programs
where the program would ask you for your name, and then printed it back 
to you. This works fine from the command line: the `input()` call in
Python can be used to read input typed by the user. Let us adjust the
code a bit to let the user enter the bound and the divisors.

```
upper_bound = input("What is the upper bound of numbers to be considered?\n")
upper_bound = int(upper_bound)

divisors = []
read_more = True
while read_more:
   next = input("Enter a divisor you want to consider (or -1 if you are done)\n")
   next = int(next)
   if next == -1:
      read_more = False
   else:
      divisors.append(next)

# The code with the computation goes here
```
{: .language-python}

This version of the program is stored in the file `divisors-stdin.py`. Let's run it:

```
$ python3 divisors-stdin.py
```
{: .language-bash}

When we run this, we get interactive prompts where we can specify the
bound and the divisors, as follows:

```
What is the upper bound of numbers to be considered?
100
Enter a divisor you want to consider (or -1 if you are done)
3
Enter a divisor you want to consider (or -1 if you are done)
5
Enter a divisor you want to consider (or -1 if you are done)
-1
The following numbers are divisible by all of the divisors
[15, 30, 45, 60, 75, 90]
```
{: .output }

Bash actually has a handy feature we can use if we want to avoid
manually typing in all the input ourselves. Rather than sending
data from the keyboard to the *standard input* of the Python program,
we can send the data in a text file instead. We can do so by with the
`<` operator on the command line. There is already a text file we
can try this with: `~/examples/data1.txt` that has the following
contents:

```
100
3
5
-1

```

Let us send this as input to our Python program using the following
command:

```
$ python3 divisors-stdin.py < ~/examples/data1.txt
```
{: .language-bash }

This gives us the following output:

```
What is the upper bound of numbers to be considered?
Enter a divisor you want to consider (or -1 if you are done)
Enter a divisor you want to consider (or -1 if you are done)
Enter a divisor you want to consider (or -1 if you are done)
The following numbers are divisible by all of the divisors
[15, 30, 45, 60, 75, 90]
```
{: .output }

> ## Input redirection hides the input
>
> When we redirect input from a file to our Python program,
> the data from the file is not shown in the printed. If we
> type the data into the terminal ourselves, the text is printed
> only because we type it. This is why we do not see the numbers
> from `~/examples/data1.txt` in our output.
{: .callout }

In some cases, this can be more convenient than typing the input
directly into the program. See what happens if you run the
program with a different data file, `~/examples/data2.txt`.
Try editing the file and see if you can get a different output!

## Reading command line arguments from our program

While reading input from a file passed via the command line
does help to make our program more flexible, it can also be
inconvenient that we must prepare a file with the things
to send to the program.

When we worked with other terminal commands, such as `ls`,
or it was possible to pass specific arguments such as `-l`
that adjusted the behavior of the program. We can do something
similar with our own program.

If we import the Python module `sys`, the property `sys.argv`
contains a list with the arguments passed to our program.
The first element, `sys.argv[0]` is the name of the Python
script and is often not that interesting, but the other
arguments, i.e. `sys.argv[1:]` are! To test this, the
following two line Python program can be found under 
`~/examples/python/printargs.py`:

```
import sys
print(sys.argv[1:])
```
{: .language-python }

If we run `python3 printargs.py 100 3 5` we get the following output:

```
['100', '3', '5']
```
{: .output }

That seems to work! Let's rewrite our program
so that it works with these command line arguments rather than
the standard input. This would give the following code:

```
import sys

# Converts the first command line argument to an int
upper_bound = int(sys.argv[1])
# Takes the remaining command line arguments sys.argv[2:] and converts them to ints using a list comprehension
divisors = [int(arg) for arg in sys.argv[2:]]

# The code with the computation goes here
```
{: .language-python }

This Python script is available as `divisors-args.py`. Let us run it using

```
$ python3 divisors-args.py
```
{: .language-bash }

Unfortunately, this gives an error:

```
Traceback (most recent call last):
  File "divisors-args.py", line 4, in <module>
    upper_bound = int(sys.argv[1])
IndexError: list index out of range
```
{: .error }

The reason for this error is that Python expects
there to be some arguments in `sys.argv`, but we
forgot to pass them!

> ## A list of strings and arguments with spaces
> 
> The arguments passed to our Python script will be accessible within our Python program as
> `sys.argv`. Note that all arguments are a string, thus we first need to convert them to
> other data types if that is desirable (in the example we convert them to `int`'s).
> The arguments are separated by spaces. Thus if we run `python3 printargs.py hello there`, 
> we can see `sys.argv[1:]` is the list `['hello', 'there']`. To avoid this, you can
> pass an argument that contains a space by surrounding it with quotation marks, i.e.
>  `python3 printargs.py "hello there"`, which gives the list `['hello there']`.
>
> It can be handy to use strings in your programs. Such strings can be names of files from
> which your program should read data, write data to, an URL to retrieve data from or
> some non-numeric property.
{: .callout }

Let's fix this problem by adding some command line arguments

```
$ python3 divisors-args.py 100 3 5
```
{: .language-bash}

Fortunately, this gives the correct output:

```
The following numbers are divisible by all of the divisors
[15, 30, 45, 60, 75, 90]
```
{: .output }

Feel free to play around with this! What happens if you pass different numbers?

## Nicer command line arguments with a parser

Now that we have learned how we can use arguments passed on the command line within
our Python program, there are a few things to consider. First, the error message
we got when we forgot to pass arguments was not really helpful. Second, if we have
many arguments, it can become a hassle to remember a fixed order we should provide
them. Furthermore, we may not want to be forced to provide all arguments at the same
time. For the sake of usability, it is often a good idea to have *sensible defaults*
for the settings of your program, and let the user only write command line arguments
for settings he desired to override. Thus, if we compare our Python program 
to the comprehensive help we get when we run `ls --help`, and it is clear that there
is still room for improvement.

Python comes with a very helpful library that allows us to improve this rather easily,
the [`argparse` library](https://docs.python.org/3/library/argparse.html). This library
gives us the option to easily construct a *argument parser*, which is a program that can
process command line arguments for us, and provide help and meaningful error messages in
case of trouble. After the argument parser is constructed, we let it consume the commandline
arguments and if the parser completes without raising an exception it gives us easy access
to the arguments we are interested in.

Setting up the argparse library is not that difficult, and is done in the script
`divisors-argparse.py`. We use the following code to set up the argument parser:

```
import argparse

# Construct the command line parser
parser = argparse.ArgumentParser(description="Find numbers that are divisible by a list of divisors")
parser.add_argument('--bound', '-b', type=int, default=100, help="An upper bound on which numbers to check")
parser.add_argument('--divs', '-d', type=int, nargs='*', required=True, help="A list of divisors to check for")

# Try to parse the arguments using the parser
args = parser.parse_args()

# Converts the first command line argument to an int
upper_bound = args.bound
# Takes the remaining command line arguments sys.argv[2:] and converts them to ints using a list comprehension
divisors = args.divs
```
{: .language-python }

As you can see, setting up the parser requires only three lines of code: one for the parser itself and one
line for each argument. For each argument we specify a number of properties: the names of the argument,
the type of the argument, and a help message to display. Furthermore, we can define the default value
of the argument if the user omits it, and we can also specify arguments that can have any number of
values, such as the `--divs` argument in the example does with the `nargs='*'` attribute. This tells
the argument parser multiple numbers can be provided behind this argument, and the parser will then
give a list of values, rather than a singular value.

First, let's try our program with the `--help` argument:

```
$ python3 divisors-argparse.py --help
```
{: .language-bash }

which prints
```
usage: divisors-argparse.py [-h] [--bound BOUND] --divs [DIVS [DIVS ...]]

Find numbers that are divisible by a list of divisors

optional arguments:
  -h, --help            show this help message and exit
  --bound BOUND, -b BOUND
                        An upper bound on which numbers to check
  --divs [DIVS [DIVS ...]], -d [DIVS [DIVS ...]]
                        A list of divisors to check for
```
{: .output }

If we call the script without any arguments, i.e. `python3 divisors-argparse.py`,
we get a much more comprehensible error message as well.

```
usage: divisors-argparse.py [-h] [--bound BOUND] --divs [DIVS [DIVS ...]]
divisors-argparse.py: error: the following arguments are required: --divs/-d
```
{: .error }

This can easily be fixed by adding the proper argument. When we run

```
$ python3 divisors-argparse.py --divs 3 5
```
{: .language-bash }

the output looks like we have come to expect. Notice that we did not
provide the `--bound` argument, and that the default value `100` is still
being used! If we want to want to know the divisors for a different bound,
we can add it (either before or after the `--divs` argument).

```
$ python3 divisors-argparse.py --divs 3 5 --bound 250
```
{: .language-bash }

which finally prints

```
The following numbers are divisible by all of the divisors
[15, 30, 45, 60, 75, 90, 105, 120, 135, 150, 165, 180, 195, 210, 225, 240]
```
{: .output }


> ## Modify the program
> 
> Add a third argument `--forbid` (and short name `-f`) that
> accepts a list of divisors that should not be a divisor of a
> number. This argument should be optional. Copy `divisors-argparse.py`
> to a new file `divisors-argparse2.py` and adjust the code to include
> this new option correctly. You can consider to make the `--divs` argument
> optional as well, but this is not required.
> Make sure that running
> ```
> $ python3 divisors-argparse2.py -d 3 5
> ```
> {: .language-bash }
> produces the output
> ```
> The following numbers adhere to the rules defined
> [15, 30, 45, 60, 75, 90]
> ```
> {: .output }
> and that 
> ```
> $ python3 divisors-argparse2.py -d 3 5 -f 20
> ```
> {: .language-bash }
> produces the output
> ```
> The following numbers adhere to the rules defined
> [15, 30, 45, 75, 90]
> ```
> {: .output }
> > ## Solution
> > 
> > First, it is necessary to add the new command to the parser:
> >
> > ```
> > parser.add_argument('--forbid', '-f', type=int, nargs='*', default=[], help="A list of divisors that are not allowed")
> > ```
> > {: .language-python }
> > add a line that extracts the argument into a variable with a name such as `forbidden`:
> > ```
> > forbidden = args.forbid
> > ```
> > {: .language-python }
> > and finally add an extra loop that sets `divisable` to `False` if `i` is divisable by a number in `forbidden`.
> > The complete solution looks as follows:
> > ```
> > import argparse
> > 
> > # Construct the command line parser
> > parser = argparse.ArgumentParser(description="Find numbers that are divisible by a list of divisors")
> > parser.add_argument('--bound', '-b', type=int, default=100, help="An upper bound on which numbers to check")
> > parser.add_argument('--divs', '-d', type=int, nargs='*', required=True, help="A list of divisors to check for")
> > parser.add_argument('--forbid', '-f', type=int, nargs='*', default=[], help="A list of divisors that are not allowed")
> > 
> > # Try to parse the arguments using the parser
> > args = parser.parse_args()
> > 
> > # Extracts the upper bound and list of divisors from the parsed result
> > upper_bound = args.bound
> > divisors = args.divs
> > forbidden = args.forbid
> > 
> > result = []
> > for i in range(1,upper_bound):
> >    divisable = True
> >    for div in divisors:
> >       if i % div != 0:
> >          divisable = False
> >    for div in forbidden:
> >       if i % div == 0:
> >          divisable = False
> >    if divisable:
> >       result.append(i)
> > 
> > print("The following numbers adhere to the rules defined")
> > print(result)
> > ```
> > {: .language-python }
> {: .solution }
{: .challenge }
