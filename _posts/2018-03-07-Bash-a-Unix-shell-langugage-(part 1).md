---
layout: post
title: Bash a Unix shell language (part 1)
---

## Introduction
There are several good reasons why you should learn Bash. First of all, if you are using Linux it is mandatory to learn some Bash in order to become a super user and you don’t want to be anything else. If you don’t use Linux, you should start.

## Automate boring stuff: Bash versus Python?
Bash is good when it comes to piping, redirection, stdin, stdout, stderr, etc. Since Bash is design to talk to the OS there are several fantastic features that makes your life easier. I recently had to automate some scripting on a server. Data was pushed to the server and needed resampling before entering a database. I had a Python script that resampled the data, but I wanted it to happen not once but continuously as data streamed in. Bash provided me with Crontab which is a job scheduler that solved this issue.

Bash is clearly good at automating things that has to do with the OS. But Python has modules such as glob and os that can handle much of the same issues. So when to use Bash and when to use e.g. Python? I guess it boils down to taste and the task at hand. Here are some points that I’ve understood through discussing this with people:

* Handling data in Bash is not optimal.

* Bash is excellent at small scripts that chain existing shell command together.

* Python is a lot easier to maintain than Bash, especially large Bash scripts can get really nasty.

## Here are some quotes:

* “As soon as I’m doing anything more complex than a for loop I switch to Python.”

* “My ~/bin folder is full of little useful bash scripts I’ve written. Some are very old and continue to work, upgrade after upgrade. (How much 15-year-old Python code still ‘just works’?)”

* “I find that it’s easier to pre-process data using Bash + built-ins like sed, awk, tr and cut, rather than trying to do the same work in Python. Suppose that you have a 2GB text file with multiple columns and you want just a few of them that have certain values in them. It’s pretty simple and fast do that extraction before loading the whole thing into Python (or worse, parsing line by line).”

* “It really boils down to what is the best tool for the job.”

* “Bash starts to suck after about 100 lines of code, and after 500 it starts to become virtually unmaintainable. Small ad hoc scripts have a habit of growing into bigger beasts as time goes on.”

* “The Bash type system is basically ‘it’s a string, deal with it somehow’. This leads to all sorts of weird and unpredictable behavior where instead in Python you’d get a TypeError or ValueError that’s easy enough to track down.”

* “Python has an ecosystem of tens of thousands of packages that do all sorts of things that you may want (e.g. reading CSV files or XML or whatever). Bash can just run programs that input files or text and output text.”

* “My rule is if it takes under 5 minutes to write and I’m probably going to throw it away I do it in Bash. Anything else I use Python.”

* “Bash’s single advantage is longevity. There are Bash scripts that are older than Python itself that are still running just fine.”

## New to Bash?
If you are new to Bash I’d recommend [LinuxCommand.org]( http://linuxcommand.org/index.php).

## Summary of useful common commands

Command | Description
--- | ---
pwd | print working directory
cd | change directory
ls | list files in directory
less | view a text file
file | determine what type of file
cp | copy file and directory
mv | move or rename file and directory
rm | remove file or directory
mkdir | create directory
type | info about command type
which | locate command
help | reference page for shell builtin
man | on-line command reference
