# Linux Shell Implementation

This repository contains the implementation of *smash*, a simple Unix shell developed as part of the Operating Systems course (CS537) at the University of Wisconsin-Madison. Objectives:
- Familiarization with the Linux programming environment
- Understanding process creation and management
- Implementing essential shell functionality

## Project Overview

The goal of this project was to implement a basic command-line interpreter, or shell, that can execute commands in a manner similar to the Bash shell. The shell, named `smash`, operates in interactive mode, continuously prompting the user for input and executing commands until the `exit` command is received.

## Features

### Basic Functionality
- **Interactive Mode**: `smash` runs in a loop, prompting the user for commands and executing them.
- **Built-in Commands**:
  - `exit`: Exits the shell.
  - `cd <directory>`: Changes the current working directory.
  - `pwd`: Prints the current working directory.
  - `loop <n> <command>`: Executes the specified command `n` times.

### Advanced Features
- **Command Execution**: Supports the execution of both built-in and external commands using `fork`, `exec`, and `wait` system calls.
- **Redirection**: Output of commands can be redirected to files using the `>` operator.
- **Piping**: Supports piping between commands, allowing the output of one command to serve as the input to another.
- **Multiple Commands**: Allows chaining multiple commands in a single line using the `;` separator.

## Usage

To run the shell, simply compile the code and execute the resulting binary:

```bash
gcc -o smash smash.c
./smash
```

Once smash is running, you can enter commands as you would in any Unix shell. The shell will execute the commands and prompt for further input.

### Example Commands
```bash
bash
smash> pwd
/home/user

smash> cd /tmp
smash> pwd
/tmp

smash> /bin/ls -la /tmp > output.txt

smash> cat output.txt | grep "log"
```


### Built-in Commands
The shell supports a few built-in commands (exit, cd, pwd, and loop) that are executed directly by the shell process. These commands do not require the creation of a child process and are handled by specific functions in the code.

### Command Execution
For non-built-in commands, smash uses the fork and execv system calls to create a new process that executes the command. The parent process waits for the child process to complete using the wait system call.

### Redirection
The shell supports output redirection using the > operator. The implementation involves creating or truncating the specified file and redirecting the standard output to this file using dup2.

### Piping
Piping is implemented by creating multiple child processes connected by pipes. Each process reads from the previous process's output and writes to the next process's input, effectively chaining the commands together.

### Development Environment
- Language: C 
- Platform: Unix/Linux
- Tools: GCC, Makefile
- Project Specifications



