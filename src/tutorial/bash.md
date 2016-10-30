# Bash Basics

> **Note:** I am building this page over time, so content will appear here slightly haphazzardly.
> So if there is content missing, I will add it later.

So you have heard of the terminal environment, and maybe have even typed some commands in it. Now wouldn't you like to learn how add some more programming type logic to it? or perhaps even save these commands for later use? In this basic tutorial I hope to convey some of this knowledge to the reader. I will also try to keep this page updated when I see new tricks that are wonderful to use.

## Things To Be Covered

If you know some of these things then skip to what you want to know.

* [Basics](#basics)
* [Quick Notes on Syntax](#quick_notes_on_syntax)
* [Printing](#printing)
* [echo](#echo)
* [printf](#printf)
* [Variables](#variables)
* [Basic Math](#basic_math)
* [Printing with Variables](#printing_with_variables)
* [Reading in from the user](#reading_in_from_the_user)
* [Process Substitution](#process_substitution)
* [Control Structures](#control_structures)
* [Globing](#globing)
* [Piping](#piping)
* [Input/Output Redirection](#input_output_redirection)
* [Exit Codes](#exit codes)
* [Functions](#functions)
* [Arrays](#arrays)
* [Shorthand Filtering](#shorthand_filtering)

## Start

First you are going to need two things, a text editor and a terminal. For each OS there are multiple terminal apps you can use. I only know of a few having only used a few OSs but there are many that you can use. For `OS X` (or `macOS`) there is the built in `Terminal.app` and some people prefer [`iTerm2` ](https://www.iterm2.com). If you have an `Ubuntu` based distrobution of Linux then there is the `Terminal` (sometimes called by different names). And in the latest versions of `Windows 10` there is also a bash shell.

And secondly you should get some form of text editor, my preference when writing bash scripts is `Vim` however [Sublime 3](https://www.sublimetext.com/3) is also very good.

## Basics

First and foremost: **be careful what you do in a bash script, bash is very powerful and can do much harm if you make a mistake**. I have made many mistakes while learning, most of them were very easily corrected but not all mistakes are easy to correct.

Now for some things to keep in mind:

* Anything that can be run in your terminal can be put into a script file.
* Your script will run in a different instance to your shell, most of the time.
* Arguments can be passed to your script, and a result can be returned, but variables are usually kept in context of the script or your shell session, there are exceptions to this which I will cover later.

The syntax I will show will look as follows.

When I am using the shell the output will be shown as such:

```bash
$ ls -l
total 4.0K
-rw-r--r-- 1 c0de 1.9K Aug  8 18:23 style.css
$
```

> Remember though that you should not be typing the `$` at the beginning of the line.

However a file will be shown as:

```bash
# filename.sh
ls -l
```

and then the output shown as:

```bash
$ bash filename.sh
total 4.0K
-rw-r--r-- 1 c0de 1.9K Aug  8 18:23 style.css
$
```

> Notice that whenever I use the shell I will always end my output on another `$` symbol to show where the next input starts.

## Quick Notes on Syntax

There are some things you need to be aware of with Bash:

* Bash is **NOT** forgiving when spaces are incorrect. Having one to few or to much will create a syntax error.
* Syntax needs to be exact, otherwise either your script will break, or you will get weird results.
* A comment starts with a `#` and ends when the line ends. There are no multi-line comments.

Bash scripts should always start with one of the following:

* `#!/bin/bash` - most common
* `#!/usr/bin/bash` - an alternative to the above
* `#!bash` - least common
* `#!env bash` - most widely supported (apparently)

If you are unsure which to use then use the first one (`#!/bin/bash`).

## Printing

The basics of printing can be done in one of two ways, using the `echo` command or using the `printf` command. Most of the time you will see the `echo` command being used, and very very rarely the `printf` command is used.

### echo

`echo` is a basic command with some nifty extra first you can echo plain text:

```bash
$ echo "Hello"
Hello
$
```

However is does have some noteworthy options that can be used: `-n` which removes the automatically inserted line ending and `-e` which allows for special symbols to be added.

```bash
$ echo -n "Notice the last character"
Notice the last character$
```

> The previous example was to show that no new line was added by default. Hence the `$` at the end of the line

```bash
$ echo -e "Adding\nNew lines\nIn My text"
Adding
New lines
In My text
$
```

### printf

Using the `printf` function however allows for formatted printing. The formatting works in the same way as in most languages where you have the following meanings:

* `%f` - for floating point number
* `%d` - for an integer
* `%s` - for a string

But the ones that will be seen the most will be:

* `%10s` - ensure a column of 10 characters, adding spaces to the string as needed
* `%03d` - ensure a 3 digit number prepended with zeros to fill the gap.

Notice however that `printf` does not automatically add a new line for you.

Examples are:

```bash
$ printf "%03d\n" 2
002
$ printf "+%10s - %d\n" "Test" 2
+      Test - 2
$
```

## Variables

Bash allows for variables which you can save one of 3 types of data. Numbers, string and array. Arrays however will be covered in a section on it's own much later.

> There are some things you need to be aware of when using variables in Bash:
>
> * When assigning values there must be no space around the assignment operator: `a="qwerty"` is correct but `a ="qwerty"` and `a= "qwerty"` will  result in syntax errors.
> * When using variables, they need to be prepended with a `$` most of the time (you will see later when not to use them).
> * Variables are blank when they have not been assigned yet so `echo "$a"` will print a blank line if `$a` has not been created yet.

Assigning and using variables:

```bash
$ a=99
$ b="some text"
$ c="$a$b" # will concatenate the two variables
$ echo "$c"
99some text
$
```

## Basic Math

Now as most languages have some way to do basic math operations so does Bash. However note that the math that Bash can do is very limited so use with care:

```bash
$ a=8
$ echo "$((a+1))"
9
$ echo "$((a-1))"
7
$ echo "$((a*2))"
16
$ echo "$((a/2))"
4
$ echo "$((a%2))"
0
$
```

> Notice that this is one of those times when there is no `$` with the usage of a variable.

And to reassign the value back to the orriginal variable we can just use the assignment operator again:

```bash
$ a=2
$ a=$((a+2))
```

## Printing with Variables

Now that we can use variables and save simple data into them we would probably need to be able to print out that data again.

There was an axample further up where we did print out the values.

```bash
$ s="Some String"
$ a=3
$ echo $s
Some String
$ echo $a
3
$ echo "then the string is '$s' and the number is '$a'"
then the string is 'Some String' and the number is '3'
$
```

## Reading in from the user

There are many times that instructions will be needed from the user in terms of input from the user. There are two ways of doing this. Either you use arguments into your file, or you request information from the user in your script. (The first way is preferred.)

### Command Line Argument

A command line argument is what you would add after the name of the program. An example to this is the `-l` argument one can add to the `ls` program. Arguments usually change the way the program or script operates. As for example passing `-l` to `ls` will cause the output to show a list of files instead of your files in one line.

An example of passing arguments into a bash program would look like one of these three ways (depending on how you run your script).

```bash
$ bash myscript.sh argument1 argument2
$ ./myscript.sh argument1 argument2
$ myscript.sh argument1 argument2
```

The first way explicitly calls the program `bash` to run your script. The second way uses that first line in your script (`#!/bin/bash`) to figure out what program to call your script with. And the third way only works if your script is in the executable path, which in our case will not be the case.

Now that we can give arguments to our scripts, our scripts will need a way of reading them out again.

There are some special variables that are used in this case.

* `$#` has the count of variables that are in the list of arguments.
* `$0` contains the name of the script being run.
* `$1` to `$9` has the first nine arguments to your program.
* `${10}` and onwards contains the rest, you can also use `${1}` to get the arguments at the places below 10, but it looks ugly to it this way.

So to use this in a simple example:

```bash
# myscript.sh
echo "Count of arguments provided: $#"
echo "This script name: $0"
echo "The first argument: $1"
echo "The second argument: $2"
```

```bash
$ bash myscript.sh fluffy bunny
Count of arguments provided: 2
This script name: myscript.sh
The first argument: fluffy
The second argument: bunny
$ bash myscript.sh fluffy
Count of arguments provided: 1
This script name: myscript.sh
The first argument: fluffy
The second argument:
$ bash myscript.sh
Count of arguments provided: 0
This script name: myscript.sh
The first argument:
The second argument:
$
```

> Notice that in the second and third run some of the lines didn't print anything (nothing was given) but the script also gave no error.

### Reading User Input

Now the other way is request for information from the user when needed. This is very good for information that you do not want the user to store in their terminal history, and there is even a special command to read passwords so that they do not display on the screen while the user is typing it.

The command we are going to use is `read` note that there are some special arguments that you can give it

* `-s` this is for when you want the text to be hidden while you type.
* `-p "<other text>"` this will allow you to prompt the user for something.
* `-N <number>` this is great for when you only want a certain amount of text read in. Very useful for yes/no questions.
* And anything given without a `-` infront will be treated as if that is the variable to read into.

> If no variable is given then the variable that the data is read into is `$REPLY`

```bash
$ read
hi
$ echo "$REPLY"
hi
$ read -s
$ echo "$REPLY"
password
$ read -p "Answer this question: "
Answer this question: sure
$ echo "$REPLY"
sure
$ read -N 1 -p "Yes or No: "
Ye^C
$ read -N 1 -p "Yes or No: "
Yes or No: Y$ #Notice I pressed enter here after typing which gave the weird printing
$ echo "$REPLY"
Y
$ read answer
hello
$ echo "$answer"
hello
$
```

## Process Substitution

## Control Structures

As in other languages there are control structures in bash. The 3 main ones are `if`, `while` and `for`. And they work in similiar ways to other languages.

### if

An `if` is used to make decisions as it is used in other languages, the simplest if would be as follows:

```bash
# test_if.bash
if [[ 1 < 3 ]]; then
  echo "Yay"
fi
```

> Remember spacing is very important here!!!

And there is also the `else` keyword as in other languages:

```bash
# test_if_else.bash
if [[ 1 > 3 ]];then
  echo "damn"
else
  echo "Yay"
fi
```

And there is also an else if, however the keyword to use would be `elif`:

```bash
# test_if_elseif.bash
if [[ 1 > 3 ]]; then
  echo "No"
elif [[ 2 > 1 ]]; then
  echo "Yay"
fi
```

### while

The `while` statement follows the following syntax:

```bash
# test_while.bash
while read a; do
  echo "$a"
done
```

> Replacing the `read a` with whatever command you would like to be as the test condition.

The `while` statement can use either the exit code of a command as above or a test condition as in the example with the `if` above. However most of the time that I use the `while` statement I am normally reading in input as in above.

### for

The `for` statement follows the following syntax:

```bash
# test_for.bash
for f in *.mp3; do
  echo "$f"
done
```

The `for` statement requires a variable (the `f`) and something to loop over (the `*.mp3`). Most of the time that I use a for loop I use one of the following examples:

```bash
# test_for_examples.bash

# print all the numbers from 1 to 10:
for i in {1..10}; do
  echo "$i"
done

# print all the odd numbers between 1 and 10:
for i in {1..10..2}; do
  echo "$i"
done

# print out the names of all the mp3 files in the current directory:
for f in *.mp3; do
  echo "$f"
done

# or print out all the parts of a command (the output is split by space):
for p in `date`; do
  print "$p"
done
```

## Comparisons

The comparisons one can make depends on the data type that is used in the comparison. Bash supports using either numeric types or strings:

### Numeric

Numerical types use the same symbols as most languages: `<`, `>`, `==`, `<=`, `>=` and `!=`.

### Strings

Strings on the other hand cannot use the above symboles, instead they use the following:

* `-eq` - to test for equality.
* `-ne` - to test for non equality.
* `-gt` - to test for greater than.
* `-lt` - to test for less than.
* `-ge` - To test for greater and equality.
* `-le` - to test for less than and equality.

## Globing

## Piping

## Input/Output Redirection

## Exit Codes

## Functions

## Arrays

## Shorthand Filtering

## Script Snippets

Here I will add short snippets that I have found useful in the past.