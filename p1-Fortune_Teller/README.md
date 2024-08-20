# Badger-Fortune

This repository contains the implementation of **Badger-Fortune**, a simple command-line fortune-telling utility developed as part of the Operating Systems course (CS537) at the University of Wisconsin-Madison. Objectives:
- Familiarize with a shell / terminal / command-line of UNIX
- Learn about how UNIX command line utilities are implemented

## Project Overview

The purpose of this project was to create a utility that reads a fortune file, which contains a series of quotes or fortunes, and outputs specific fortunes based on user input. The utility supports both single fortune retrieval and batch processing of multiple fortunes.

## Features

### Basic Functionality
- **Single Fortune Mode**: Retrieve a specific fortune from the fortune file using the `-n` flag.
- **Batch Mode**: Retrieve multiple fortunes specified in a batch file using the `-b` flag.
- **Output Redirection**: Optionally redirect the output to a file using the `-o` flag.

### Error Handling
The utility handles a variety of errors, printing appropriate messages to `stdout`:
- **Invalid Flags**: Detects and reports incorrect flag usage.
- **File Errors**: Reports errors when the fortune file or batch file cannot be opened.
- **Invalid Fortune Numbers**: Handles out-of-range fortune numbers and invalid input in both single and batch modes.

## Usage

To run the utility, compile the code and execute the resulting binary with the appropriate flags:

```bash
gcc -Wall -Werror -o badger-fortune badger_fortune.c
./badger-fortune -f <file> -n <number> (optionally: -o <output file>)
```
or

```bash
./badger-fortune -f <file> -b <batch file> (optionally: -o <output file>)
```

### Example Commands
Retrieve a single fortune:

```bash
./badger-fortune -f fortune.txt -n 1
```

Process a batch file of fortunes:
```bash
./badger-fortune -f fortune.txt -b batch.txt
```

Redirect output to a file:
```bash
./badger-fortune -f fortune.txt -n 1 -o output.txt
```

## Implementation Details

### File Format
- Fortune File: The file begins with two numbers indicating the number of fortunes and the maximum length of a fortune. Each fortune is separated by a % character.
- Batch File: A simple list of fortune numbers, each on its own line.
### Memory Management
- The utility dynamically allocates memory for storing the fortunes based on the size information provided in the fortune file.

### Output Redirection
- The -o flag allows the user to redirect the output of fortunes to a specified file instead of stdout.

### Development Environment
- Language: C
- Platform: Unix/Linux
- Tools: GCC, Makefile

