---
title: "(Option 3 - Java) Run your own code"
teaching: 0
exercises: 0
questions:
- How do I run Java Programs?
- What options do I have to pass data to my programs?
objectives:
- Run Java programs from the command line
- Understand how a program can read what you type into the terminal
- Be able to pass arguments when you execute the program
- Make a flexible Java program that can except different arguments
keypoints:
- Your own Java programs can be run from the command line
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

**Note:** this episode is written for participants who prefer to work
with Java. There are also versions of this episode for participants
who prefer [Python](../06-python-run/index.html) or who prefer [R](../07-r-run/index.html). If you are finished with this lesson, you can also go to the [next episode](../09-tmux-cron/index.html).

In this episode we will run our own code, written in Java,
from the command line. Let us suppose that we have written a
program that searches for integer number in a certain range
that have all the numbers in a list as their divisor.

For this we have written the following program a put it in a
file `Divisors.java`. You can find this file in the directory
`~/examples/java`.

```
import java.util.List;
import java.util.ArrayList;

public class Divisors {
   public static void main(String [] args) {
      int upperBound = 100;
      List<Integer> divisors = List.of(3, 5);

      List<Integer> result = getDivisors(upperBound, divisors);
      System.out.println("The following numbers are divisible by all of the divisors");
      System.out.println(result);
   }

   public static List<Integer> getDivisors(int upperBound, List<Integer> divisors) {
      List<Integer> result = new ArrayList<>();
      for (int i=1; i < upperBound; i++) {
         boolean divisable = true;
         for (int div : divisors) {
            if (i%div != 0) {
               divisable = false;
            }
         }
         if (divisable) {
            result.add(i);
         }
      }
      return result;
   }
}
```
{: .language-java}

> ## Portable Code
>
> Since Java compiles to bytecode that is ran on the JVM, you can easily
> write and compile Java code on a Windows or Mac computer,
> and then run it on a Linux computer without modification.
> Making sure code runs on multiple operating
> system is called writing *portable* code. For most Java code, 
> things will work without adaption, but you may have to
> pay attention with things that can differ between systems, such as:
>
> * Don't hard code absolute file paths that only exist on your local computer, such as `C:\Users\Jane McDoe\mydata.csv`, but always use relative paths, e.g. `mydata.csv`.
> * Avoid hard coding file separator symbols, i.e. `/` and `\` as they differ between Windows and Mac/Linux. Use the property `File.separator` if you need this, as it will take value depending on the operating system your program is ran on, or look for a more stable way to construct file paths in the documentation.
> * Avoid calling system specific commands, such as `System.exec('ls')`, as it will not work on a Windows system.
> * If your program uses compiled or native libraries, be sure that the native libraries are available on all operating systems you want to run your program on.
{: .callout }

We will now consider how we can run this program from the command line.

## Running a Java program

Since Java is a compiled language, the first step is to compile the program.
This can be done with the command for the Java compiler, `javac`. Note that
we only need to compile a program once. If it is compiled, we can run it as
many times as we want. We need to pass the Java compiler the files we want
to compile as arguments. Let's do so for our program:

```
$ cd ~/examples/java
$ javac Divisors.java
```
{: .language-bash }

If we do not get any error, we can use `ls` to check if a `Divisors.class` 
was generated. If yes, this indicates the program was compiled succesfully.

After it is compiled, we can actually run it. 

```
$ java Divisors
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
to you. This works fine from the command line: if we use a `Scanner` on
`System.in` we can read input typed by the user. Let us adjust the
code a bit to let the user enter the bound and the divisors, by adding
the following to the `main` method.

```
try (Scanner scan = new Scanner(System.in)) {
   System.out.println("What is the upper bound of numbers to be considered?");
   int upperBound = scan.nextInt();
   List<Integer> divisors = new ArrayList<>();

   boolean readMore = true;
   while (readMore) {
      System.out.println("Enter a divisor you want to consider (or -1 if you are done)");
      int div = scan.nextInt();
      if (div == -1) {
         readMore = false;
      }
      else {
         divisors.add(div);
      }
   }

   // Perform the computation here
} 
```
{: .language-java}

This version of the program is stored in the file `DivisorsStdIn.java`.
Let's compile and run it.

```
$ javac DivisorsStdIn.java
$ java DivisorsStdIn
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
data from the keyboard to the *standard input* of the Java program,
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

Let us send this as input to our Java program using the following
command:

```
$ java DivisorsStdIn < ~/base/examples/data1.txt
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
> When we redirect input from a file to our Java program,
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

Every main method in Java always has an array declared, like
`String [] args`. You may have wondered what this array is
used for. In fact, it contains any command line arguments
passed to the Java program!

To test this, the following two line Java program can be found under 
`~/examples/java/PrintArgs.java`:

```
public class PrintArgs {
   public static void main(String [] args) {
      for (String arg: args) {
         System.out.println(arg);
      }
   }
}
```
{: .language-java }

If we first compile it with `javac PrintArgs.java` and then run
`java PrintArgs 100 3 5` we get the following output:

```
100
3
5
```
{: .output }

That seems to work! Let's rewrite our program
so that it works with these command line arguments rather than
the standard input. For this we would add the following code
to the `main` method:

```
int upperBound = Integer.parseInt(args[0]);
List<Integer> divisors = new ArrayList<>();
for (int i=1; i < args.length; i++) {
   divisors.add(Integer.parseInt(args[i]));
}

// The code with the computation goes here
```
{: .language-java }

This Java program is available as `DivisorsArgs.java`. Let us run it using

```
$ javac DivisorsArgs.java
$ java DivisorsArgs
```
{: .language-bash }

Unfortunately, this gives an error:

```
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 0 out of bounds for length 0
        at DivisorsArgs.main(DivisorsArgs.java:7)
```
{: .error }

The reason for this error is that our program expects
there to be some arguments in the `String [] args`, but we
forgot to pass them!

> ## A list of strings and arguments with spaces
> 
> The arguments passed to our Java program will be accessible within the `main` method of
> our Java program as the `String []` argument of this method. Since it is an array of `String`,
> we first need to convert them to other data types if that is desirable (in the example we
> convert them to `int` using `Integer.parseInt`).
> The arguments are separated by spaces. Thus if we run `java PringArgs hello there`, 
> we get two separate lines with `hello` and `there`. To avoid this, you can
> pass an argument that contains a space by surrounding it with quotation marks, i.e.
> `java PrintArgs "hello there"`, which will then print `hello there` on a single line.
>
> It can be handy to use strings in your programs. Such strings can be names of files from
> which your program should read data, write data to, an URL to retrieve data from or
> some non-numeric property.
{: .callout }

Let's fix this problem by adding some command line arguments

```
$ java DivisorsArgs 100 3 5
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
our Java program, there are a few things to consider. First, the error message
we got when we forgot to pass arguments was not really helpful. Second, if we have
many arguments, it can become a hassle to remember a fixed order we should provide
them. Furthermore, we may not want to be forced to provide all arguments at the same
time. For the sake of usability, it is often a good idea to have *sensible defaults*
for the settings of your program, and let the user only write command line arguments
for settings he desired to override. Thus, if we compare our Java program 
to the comprehensive help we get when we run `ls --help`, and it is clear that there
is still room for improvement.

While Java does not come with a library for parsing command line arguments out of the
box, the [Picocli library](https://picocli.info) is a rather powerful library that
helps us improve this rather easily. This library gives us the option to easily
construct a *argument parser*, which is a program that can process command line arguments
for us, and provide help and meaningful error messages in case of trouble. After the
argument parser is constructed, we let it consume the commandline arguments and if the
parser completes without raising an exception it gives us easy access to the arguments
we are interested in.

Setting up a java library for your project can be a bit of a hassle, as both the
compiler `javac` as well as the JVM runtime `java` need to be able to find it.
A `jar` file containing version `4.6.1` of the picocli library can be found in 
`~/examples/java/picocli-4.6.1.jar`. We will pass the argument `-cp .:picocli-4.6.1jar`
to make sure they are able to.

> ## Alternative: use a maven project
>
> If you work with many libraries, an alternative approach to passing
> the libraries via the `-cp` argument to the compiler and JVM, is to
> use a build tool to package everything in one big `.jar` file, and
> then run that `.jar` file directly. Maven is one of the most famous
> build tools for Java.
>
> In the directory `~/examples/java/project` you can find a maven
> project that is configured to automatically download and package
> `picocli` with your own code. The code can be found in
> `~/examples/java/project/main/java/examples/DivisorsArgParse.java`
> and the maven project configuration file in `~/examples/java/project/pom.xml`.
>
> To compile the project using maven, you can do so using
> ```
> $ cd ~/examples/java/project
> $ mvn package
> ```
> {: .language-bash }
>
> This will create a single runnable `jar` file called `~/examples/java/project/target/divisors-argparse.jar`.
> You can run this file as follows:
> ```
> $ java -jar ~/examples/java/project/target/divisors-argparse.jar
> ```
> {: .language-bash }
>
> The main advantage of this is that you do not have to specify the
> library every time. You can even copy the `.jar` packaged file to 
> another computer and run it there. Similarly, if you export a
> runnable `.jar` file from Eclipse or IntelliJ on your local computer,
> you can use this command to run this file directly on Linux as well.
{: .callout }

Once we make sure the library can be found by Java, using it is not that difficult.
We do need to change the way our program is set up a little bit, as picocli will
use the arguments it finds to modify instance variables of an object automatically,
and will the call the `run()` method of the `Runnable` interface on that object.
We can use a `@Command` annotation on the class to define a general help message,
and `@Option` annotations on the instance variables to link the instance variables
to 
Therefore, we define the class and its (protected) instance variables as follows:

```
@Command(description="Find numbers that are divisible by a list of divisors")
public class DivisorsArgParse implements Runnable {

   @Option(names={"--bound","-b"}, defaultValue="100", description="An upper bound on which numbers to check")
   protected int upperBound;

   @Option(names={"--divs","-d"}, required=true, arity="1..*", description="A list of divisors to check for")
   protected List<Integer> divisors;

   // The methods go here
   // ...
}
```
{: .language-java }

We then add the `run` method that will be executed after picocli manages to parse
the command line arguments. This will run the old `getDivisors()` method we had,
which we will also adjust to work with the instance variables rather than with
arguments to the methods. This gives us:

```
@Override
public void run() {
   List<Integer> result = getDivisors();
   System.out.println("The following numbers are divisible by all of the divisors");
   System.out.println(result);
}

public List<Integer> getDivisors() {
   List<Integer> result = new ArrayList<>();
   for (int i=1; i < upperBound; i++) {
      boolean divisable = true;
      for (int div : divisors) {
         if (i%div != 0) {
            divisable = false;
         }
      }
      if (divisable) {
         result.add(i);
      }
   }
   return result;
}
```
{: .language-java}

Finally, we need to add a `main` method that uses the picocli class `CommandLine`
to parse the command line arguments, configure a `DivisorsArgParse` object and then
perform the computation. We can do so with the following code:

```
public static void main(String [] args) {
   // Create an empty DivisorsArgParse object
   DivisorsArgParse myObj = new DivisorsArgParse();
   // Create a picocli CommandLine object that will configure myObj and then pass it the arguments
   CommandLine cli = new CommandLine(myObj);
   cli.execute(args);
}
```
{: .language-java }

As you can see, setting up the parser requires only three annotations and three lines of code. 
Within the annotation we specify a number of properties of the argument: the names of the argument,
the type of the argument, and a help message to display. Furthermore, we can define the default value
of the argument if the user omits it, and we can also specify arguments that can have any number of
values, such as the `--divs` argument in the example does with the `arity="1..*"` attribute. This tells
the argument parser multiple numbers can be provided behind this argument, and the parser will then
give a list of values, rather than a singular value.

First, let's compile our program and then run it:

```
$ javac -cp .:picocli-4.6.1.jar DivisorsArgParse.java
$ java -cp .:picocli-4.6.1.jar DivisorsArgParse
```
{: .language-bash }

which prints
```
Missing required option: '--divs=<divisors>'
Usage: <main class> [-b=<upperBound>] -d=<divisors>... [-d=<divisors>...]...
Find numbers that are divisible by a list of divisors
  -b, --bound=<upperBound>   An upper bound on which numbers to check
  -d, --divs=<divisors>...   A list of divisors to check for
```
{: .output }

This is a nice and clear help message, that picocli automatically generated
based on the annotations in the class. It detected that the required argument
`--divs` is missing, and warns us about this.
Now let's try to run our class with a proper `--divs` argument.

```
$ java -cp .:picocli-4.6.1.jar DivisorsArgParse --divs 3 5
```
{: .language-bash }

the output looks like we have come to expect. Notice that we did not
provide the `--bound` argument, and that the default value `100` is still
being used! If we want to want to know the divisors for a different bound,
we can add it (either before or after the `--divs` argument).

```
$ java -cp .:picocli-4.6.1.jar DivisorsArgParse --divs 3 5 --bound 250
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
> number. This argument should be optional. We provided a copy of
> `DivisorsArgParse.java` called `DivisorsArgParse2.java` which
> updates the class name and the object creation in the `main` method. 
> The goal is to adjust the code to include this new option correctly.
> You can consider to make the `--divs` argument optional as well, but
> this is not required. Make sure that running
> ```
> $ javac -cp .:picocli-4.6.1.jar DivisorsArgParse2.java
> $ java -cp .:picocli-4.6.1.jar DivisorsArgParse2 --divs 3 5
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
> $ java -cp .:picocli-4.6.1.jar DivisorsArgParse2 --divs 3 5 --forbid 20
> ```
> {: .language-bash }
> produces the output
> ```
The following numbers adhere to the rules defined
[15, 30, 45, 75, 90]
> ```
> {: .output }
> > ## Solution
> > 
> > First, it is necessary to add the new command to the parser:
> >
> > ```
> > @Option(names={"--forbid","-f"}, arity="1..*", description="A list of divisors that are forbidden")
> > protected List<Integer> forbidden;
> > ```
> > {: .language-java }
> > This is enough to make sure picocli sets this instance variable if the `--forbid` argument is passed.
> > We also add an extra loop that sets `divisable` to `false` if `i` is divisable by a number in `forbidden`.
> > The complete solution looks as follows:
> > ```
> > import picocli.CommandLine;
> > import picocli.CommandLine.Command;
> > import picocli.CommandLine.Option;
> > 
> > import java.util.List;
> > import java.util.ArrayList;
> > 
> > @Command(description="Find numbers that are divisible by a list of divisors")
> > public class DivisorsArgParse2 implements Runnable {
> > 
> >    @Option(names={"--bound","-b"}, defaultValue="100", description="An upper bound on which numbers to check")
> >    protected int upperBound;
> > 
> >    @Option(names={"--divs","-d"}, required=true, arity="1..*", description="A list of divisors to check for")
> >    protected List<Integer> divisors;
> > 
> >    @Option(names={"--forbid","-f"}, arity="0..*", description="A list of divisors that are forbidden")
> >    protected List<Integer> forbidden;
> > 
> >    @Override
> >    public void run() {
> >       List<Integer> result = getDivisors();
> >       System.out.println("The following numbers adhere to the rules defined");
> >       System.out.println(result);
> >    }
> > 
> >    public List<Integer> getDivisors() {
> >       List<Integer> result = new ArrayList<>();
> >       for (int i=1; i < upperBound; i++) {
> >          boolean divisable = true;
> >          for (int div : divisors) {
> >             if (i%div != 0) {
> >                divisable = false;
> >             }
> >          }
> >          for (int div : forbidden) {
> >             if (i%div == 0) {
> >                divisable = false;
> >             }
> >          }
> >          if (divisable) {
> >             result.add(i);
> >          }
> >       }
> >       return result;
> >    }
> > 
> >    public static void main(String [] args) {
> >       // Create an empty DivisorsArgParse object
> >       DivisorsArgParse2 myObj = new DivisorsArgParse2();
> >       // Create a picocli CommandLine object that will configure myObj and then pass it the arguments
> >       CommandLine cli = new CommandLine(myObj);
> >       cli.execute(args);
> >    }
> > }
> > ```
> > {: .language-java }
> {: .solution }
{: .challenge }


