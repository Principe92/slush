Processes are the fundamental abstraction in any operating system! The whole purpose of an OS is to make it possible to execute computer programs easily. The shell is not part of the operating system, but it is a natural extension of it: a program whose purpose is to make it easy to launch other programs!

We never want to take programs like bash or csh for granted, so we'll go through the dirty work of building or own shell from scratch.

In this lab, you will:

1. Spawn new processes with the fork() and execvp() system calls
2. Wait for child processes with the wait() system call
3. Perform complex input parsing in C
4. Pipe data between processes
5. Perform basic signal handling

#Description

slush - SLU shell

slush is a very simple command-line interpreter. It uses a different syntax than shells like bash, and has much less functionality. slush executes in a loop in which it displays a prompt, reads in a command line from standard input, and executes the command.

There are two types of commands: built in commands which are executed by slush itself, and program execution commands which are carried out by separate processes.

A built in command must appear on a line by itself. The only built in command is:

cd dir - change current directory to dir

Program execution commands have the form:

prog_n [args] ( ... prog_3 [args] ( prog_2 [args] ( prog_1 [args]

This command runs the programs prog_n, ... , prog_2, prog_1 (each of which may have zero or more arguments) as separate processes in a "pipeline". This means the standard output of each process is connected to the standard input of the next.

The syntax of slush is backwards from shells you're used to, and is intended to emphasize the functional nature of pipeline commands. As an example, the command line:

more ( sort ( ps aux

should produce a paginated, sorted list of processes.

slush should catch ^C typed from the keyboard. If a command is running, this should interrupt the command. If the user is entering a line of input, slush should respond with a new prompt and a clean input line.

slush will exit when it reads an end-of-file on input.


#Error Handling

If a program name does not exist or is not executable, slush should print an error message of the form:

prog1: Not found

(To do this automatically, you can use perror() after the exec() fails).

Syntax errors such as:

more ( ( ps au

more ( ps au (

should be handled with an appropriate message, such as:

Invalid null command
