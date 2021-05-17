---
title: "(Option 2 - R) Run your own code"
teaching: 0
exercises: 0
questions:
- How do I run R programs?
- What options do I have to pass data to my programs?
objectives:
- Run R programs from the command line
- Understand how a program can read what you type into the terminal
- Be able to pass arguments when you execute the program
- Make a flexible R program that can except different arguments
keypoints:
- Your own R programs can be run from the command line
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

In this episode we will run our own code, written in R,
from the command line. Let us suppose that we have written a
program that searches for integer number in a certain range
that have all the numbers in a list as their divisor.

For this we have written the following program a put it in a
file `divisors.R`. You can find this file in the directory
`~/examples/R`.

```
upper_bound <- 100
divisors <- c(3, 5)

result <- c()
for (i in 1:upper_bound) {
   divisable <- TRUE
   for (div in divisors) {
      if (i %% div != 0) {
         divisable <- FALSE
      }
   }
   if (divisable) {
      result <- append(result, c(i))
   }
}

```
{: .language-r}

> ## Portable Code
>
> Since R scripts are interpreted, you can easily write them on
> a Windows or Mac computer, and then run them on a Linux computer
> without modification. Making sure code runs on multiple operating
> system is called writing *portable* code. For most R code, 
> things will work without adaption, but  you may have to
> pay attention with things that can differ between systems, such as:
>
> * Don't hard code absolute file paths that only exist on your local computer, such as `C:\Users\Jane McDoe\mydata.csv`, but always use relative paths, e.g. `mydata.csv`.
> * Avoid hard coding file separator symbols, i.e. `/` and `\` as they differ between Windows and Mac/Linux. Use the property `.Platform$file.sep` if you need this, as it will take value depending on the operating system your program is ran on, or look for a more stable way to construct file paths in the documentation.
> * Avoid calling system specific commands, such as `system('ls')`, as it will not work on a Windows system.
> * If your program uses compiled or native libraries, be sure that the native libraries are available on all operating systems you want to run your program on.
{: .callout }

We will now consider how we can run this program from the command line.

## Running an R program

Once we have a R program in a file like this, it is actually not that
difficult to run it from the command line. We can start the R interpreter
using the command `R`.

```
$ R
```
{: .language-bash}

which put us into an interactive mode. Here, we can evaluate R code
by directly typing it into the interpreter. In fact, it works very similar
to the bash shell, except that it speaks a different language.

```
R version 4.0.5 (2021-03-31) -- "Shake and Throw"
Copyright (C) 2021 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

> x <- c(1,2,3)
> append(x, 'hello')
[1] "1"     "2"     "3"     "hello"
> q()
Save workspace image? [y/n/c]: n
```
{: .output }

While we could type the R program line by line into this interactive mode,
this is not very convenient, so let's quit the interactive mode for now by
typing `q()` and .

The R software distribution comes with a second program we can use to 
execute R scripts, which is called `Rscript`. We have to provide a filename
that contains an R script after the `Rscript` command.
It will then just execute the script and not run in interactive mode.
Let's try and do this for our program.

```
$ cd ~/examples/R
$ Rscript divisors.R
```
{: .language-bash}

If all goes well, we should see the output of our program!

```
The following numbers are divisible by all of the divisors
15, 30, 45, 60, 75, 90
```
{: .output }

However, it might be nice if we can make the program a bit more flexible.
We will look at two ways to do so: using standard in, and via command line
arguments.

## Reading data typed into the terminal

While you learned programming, you may have written interactive programs
where the program would ask you for your name, and then printed it back 
to you. This works fine from the command line: the `scan()` call in
R can be used to read input typed by the user. If we want to let the user
enter a single numeric value from the command line, we can use the following:

```
scan(file=stdin, what=integer(0), n=1, quiet=TRUE)
```
{: .language-r }

Here `file=stdin` means we are reading from the command line, `what=integer(0)`
means we are reading an integer number, `n=1` means we are reading a single value
and `quiet=TRUE` supresses output generated by the `scan` function when it is
done reading. Now let's adjust the code a bit to let the user enter the bound and the divisors.

```
stdin <- file("stdin", "r")
read_more <- TRUE
cat("What is the upper bound of numbers to be considered?\n")
upper_bound <- scan(file=stdin, what=integer(0), n=1, quiet=TRUE)
divisors <- c()
while (read_more) {
   cat("Enter a divisor you want to consider (or -1 if you are done)\n")
   div <- scan(file=stdin, what=integer(0), n=1, quiet=TRUE)
   if (div == -1) {
      read_more <- FALSE
   }
   else {
      # The append function can be used to add an element to an existing vector
      divisors <- append(divisors, div)
   }
}

# The code with the computation goes here
```
{: .language-r }

This version of the program is stored in the file `divisors-stdin.R`. Let's run it

```
$ Rscript divisors-stdin.r
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
15, 30, 45, 60, 75, 90
```
{: .output }

Bash actually has a handy feature we can use if we want to avoid
manually typing in all the input ourselves. Rather than sending
data from the keyboard to the *standard input* of the R program,
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

Let us send this as input to our R program using the following
command:

```
$ Rscript divisors-stdin.R < ~/examples/data1.txt
```
{: .language-bash }

This gives us the following output:

```
What is the upper bound of numbers to be considered?
Enter a divisor you want to consider (or -1 if you are done)
Enter a divisor you want to consider (or -1 if you are done)
Enter a divisor you want to consider (or -1 if you are done)
The following numbers are divisible by all of the divisors
15, 30, 45, 60, 75, 90
```
{: .output }

> ## Input redirection hides the input
>
> When we redirect input from a file to our R program,
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

We can use the R function `commandArgs()` to obtain a list
of arguments passed to our program. Since the clean call
returns a lot of additional information, such as the name
of the program, it is useful to call it using 
`commandArgs(trailingOnly=TRUE)` to make sure we only get
the arguments given on the command line after the name of the R
script.

To test this, we provided a two line `R` script in
`~/examples/R/printargs.R`:

```
args <- commandArgs(trailingOnly=TRUE)
print(args)
```
{: .language-r }

If we run `Rscript printargs.R 100 3 5` we get the following output:

```
[1] "100" "3"   "5"
```
{: .output }

That seems to work! Let's rewrite our program
so that it works with these command line arguments rather than
the standard input. This would give the following code:

```
# Read the command line arguments and convert them to numbers
args <- commandArgs(trailingOnly=TRUE)
args <- as.numeric(args)

# Extract the first element as the upper bound
upper_bound <- args[1]
# Extract the remaining elements as the divisor
divisors <- args[2:length(args)]

# The code with the computation goes here
```
{: .language-r }

This R script is available as `divisors-args.R`. Let us run it using

```
$ Rscript divisors-args.R
```
{: .language-bash }

Unfortunately, this gives an error:

```
Error in 1:upper_bound : NA/NaN argument
Execution halted
```
{: .error }

The reason for this error is that the R script expects
there are some arguments passed, but these values are missing.
The reason is of course that we forgot to pass them!

> ## A list of strings and arguments with spaces
> 
> The arguments passed to our R script will be returned within our R program by
> `commandArgs(trailingOnly=TRUE)`. Note that all arguments are a string, thus we 
> first need to convert them to other data types if that is desirable (in the example
> we convert them to numbers using the `as.numeric()` function).
> The arguments we pass are separated by spaces. Thus if we run `Rscript printargs.R hello there`, 
> we can see the `commandArgs` function gives us the vector `"hello" "there"`. To avoid this, you can
> pass an argument that contains a space by surrounding it with quotation marks, i.e.
>  `Rscript printargs.R "hello there"`, which gives the vector `"hello there"`.
>
> It can be handy to use strings in your programs. Such strings can be names of files from
> which your program should read data, write data to, an URL to retrieve data from or
> some non-numeric property.
{: .callout }

Let's fix this problem by adding some command line arguments

```
$ Rscript divisors-args.R 100 3 5
```
{: .language-bash}

Fortunately, this gives the correct output:

```
The following numbers are divisible by all of the divisors
15, 30, 45, 60, 75, 90
```
{: .output }

Feel free to play around with this! What happens if you pass different numbers?

## Nicer command line arguments with a parser

Now that we have learned how we can use arguments passed on the command line within
our R program, there are a few things to consider. First, the error message
we got when we forgot to pass arguments was not really helpful. Second, if we have
many arguments, it can become a hassle to remember a fixed order we should provide
them. Furthermore, we may not want to be forced to provide all arguments at the same
time. For the sake of usability, it is often a good idea to have *sensible defaults*
for the settings of your program, and let the user only write command line arguments
for settings he desired to override. Thus, if we compare our R program 
to the comprehensive help we get when we run `ls --help`, and it is clear that there
is still room for improvement.

There exists a very helpful R package that allows us to improve this rather easily,
the [`argparser` package](https://cran.r-project.org/web/packages/argparser/index.html).
This package gives us the option to easily construct a *argument parser*, which is a program that can
process command line arguments for us, and provide help and meaningful error messages in
case of trouble. After the argument parser is constructed, we let it consume the command line
arguments and if the parser completes without raising an exception it gives us easy access
to the arguments we are interested in.

Setting up the argparser library is not that difficult, and is done in the script
`divisors-argparser.R`. We use the following code to set up the argument parser:

```

# Load the argparser library and install it if it is missing
if (!require("argparser", quiet=TRUE)) {
   install.packages("argparser")
   library(argparser)
}

# First we build up the argument parser
parser <- arg_parser("Find numbers that are divisible by a list of divisors")
parser <- add_argument(parser, "--bound", short="-b", help="An upper bound on which numbers to check", default=100)
parser <- add_argument(parser, "--divs", short="-d", help="A list of divisors to check for", nargs=Inf)


# Read the command line arguments and parse the arguments
args <- commandArgs(trailingOnly=TRUE)
parsed_args <- parse_args(parser, args)

# Extract the parsed data
upper_bound <- as.numeric(parsed_args$bound)
divisors <- as.numeric(parsed_args$divs)

# Check if some divisors are given
if (any(is.na(divisors))) {
   cat("Please provide one or more divisors\n")
   quit()
}
```
{: .language-r }

As you can see, setting up the parser requires only three lines of code: one for the parser itself and one
line for each argument. For each argument we specify a number of properties: the names of the argument,
the type of the argument, and a help message to display. Furthermore, we can define the default value
of the argument if the user omits it, and we can also specify arguments that can have any number of
values, such as the `--divs` argument in the example does with the `nargs=Inf` attribute. This tells
the argument parser multiple numbers can be provided behind this argument, and the parser will then
give a list of values, rather than a singular value.

First, let's try our program with the `--help` argument:

```
$ Rscript divisors-argparser.R --help
```
{: .language-bash }

which prints
```
usage: divisors-argparser.R [--] [--help] [--opts OPTS] [--bound BOUND]
       [--divs DIVS]

Find numbers that are divisible by a list of divisors

flags:
  -h, --help   show this help message and exit

optional arguments:
  -x, --opts   RDS file containing argument values
  -b, --bound  An upper bound on which numbers to check [default: 100]
  -d, --divs   A list of divisors to check for
```
{: .output }

Note that the `-x` argument is added automatically, and can be safely ignored.
Let's try to run the script using these arguments. When we run

```
$ Rscript divisors-argparser.R --divs 3 5
```
{: .language-bash }

the output looks like we have come to expect. Notice that we did not
provide the `--bound` argument, and that the default value `100` is still
being used! If we want to want to know the divisors for a different bound,
we can add it (either before or after the `--divs` argument).

```
$ Rscript divisors-argparser.R --divs 3 5 --bound 250
```
{: .language-bash }

which finally prints

```
The following numbers are divisible by all of the divisors
15, 30, 45, 60, 75, 90, 105, 120, 135, 150, 165, 180, 195, 210, 225, 240
```
{: .output }


> ## Modify the program
> 
> Add a third argument `--forbid` (and short name `-f`) that
> accepts a list of divisors that should not be a divisor of a
> number. This argument should be optional. Copy `divisors-argparser.R`
> to a new file `divisors-argparse2.R` and adjust the code to include
> this new option correctly. You can consider to make the `--divs` argument
> optional as well, but this is not required.
> Make sure that running
> ```
> $ Rscript divisors-argparser2.R -d 3 5
> ```
> {: .language-bash }
> produces the output
> ```
> The following numbers adhere to the rules defined
> 15, 30, 45, 60, 75, 90
> ```
> {: .output }
> and that 
> ```
> $ Rscript divisors-argparser2.R -d 3 5 -f 20
> ```
> {: .language-bash }
> produces the output
> ```
> The following numbers adhere to the rules defined
> 15, 30, 45, 75, 90
> ```
> **Hint:** Be aware that you may want to add an if statement that checks if
> the argument turns out to `NA`. For example, you can use something
> like `if (!any(is.na(forbidden))) { ...` to guard against the case where
> no `--forbid` argument is passed.
> {: .output }
> > ## Solution
> > 
> > First, it is necessary to add the new command to the parser:
> >
> > ```
> > parser <- add_argument(parser, "--forbid", short="-f", help="A list of divisors that are not allowed", nargs=Inf)
> > ```
> > {: .language-r }
> > add a line that extracts the argument into a variable with a name such as `forbidden`:
> > ```
> > forbidden <- as.numeric(parsed_args$forbid)
> > ```
> > {: .language-r }
> > and finally add an extra loop that sets `divisable` to `FALSE` if `i` is divisable by a number in `forbidden`.
> > The complete solution looks as follows:
> > ```
> > # Load the argparser library and install it if it is missing
> > if (!require("argparser", quiet=TRUE)) {
> >    install.packages("argparser")
> >    library(argparser)
> > }
> > 
> > # First we build up the argument parser
> > parser <- arg_parser("Find numbers that are divisible by a list of divisors")
> > parser <- add_argument(parser, "--bound", short="-b", help="An upper bound on which numbers to check", default=100)
> > parser <- add_argument(parser, "--divs", short="-d", help="A list of divisors to check for", nargs=Inf)
> > parser <- add_argument(parser, "--forbid", short="-f", help="A list of divisors that are not allowed", nargs=Inf)
> > 
> > # Read the command line arguments and parse the arguments
> > args <- commandArgs(trailingOnly=TRUE)
> > parsed_args <- parse_args(parser, args)
> > 
> > # Extract the parsed data
> > upper_bound <- as.numeric(parsed_args$bound)
> > divisors <- as.numeric(parsed_args$divs)
> > forbidden <- as.numeric(parsed_args$forbid)
> > 
> > # Check if some divisors are given
> > if (any(is.na(divisors))) {
> >    cat("Please provide one or more divisors\n")
> >    quit()
> > }
> > 
> > result <- c()
> > for (i in 1:upper_bound) {
> >    divisable <- TRUE
> >    for (div in divisors) {
> >       if (i %% div != 0) {
> >          divisable <- FALSE
> >       }
> >    }
> >    if (!any(is.na(forbidden))) {
> >       for (div in forbidden) {
> >          if (i %% div == 0) {
> >             divisable <- FALSE
> >          }
> >       }
> >    }
> >    if (divisable) {
> >       result <- append(result, c(i))
> >    }
> > }
> > 
> > cat("The following numbers adhere to the rules defined\n")
> > cat(paste(result, collapse=", "), "\n")
> > ```
> > {: .language-r }
> {: .solution }
{: .challenge }
