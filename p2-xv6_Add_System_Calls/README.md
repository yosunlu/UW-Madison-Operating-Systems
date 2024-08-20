# xv6 System Call Extensions - Project 2

This repository contains the implementation of two additional system calls for the xv6 operating system, developed as part of the Operating Systems course (CS537) at the University of Wisconsin-Madison. Objectives:
- Gain familiarity with the xv6 codebase.
- Learn to add new system calls to a Unix-like operating system.
- Utilize `gdb` for kernel debugging.

## Project Overview

The goal of this project was to introduce two new system calls into the xv6 operating system. The project serves as an introduction to kernel development by modifying a small, educational operating system. The system calls implemented are `getnextpid()` and `getprocstate()`.

## Features

### New System Calls
- **getnextpid()**: Returns the next available Process ID (PID) in the system.
- **getprocstate(int pid, char* state, int n)**: Retrieves the state of a process with the given PID and stores it in the provided buffer.

### Debugging and Testing
- Demonstrated the use of `gdb` for debugging the xv6 kernel, particularly in examining the value returned by the `fdalloc()` function during process initialization.

## Recreating the Environment

### Prerequisites
To work with the xv6 operating system, you need the following tools installed on your Unix/Linux environment:

- GCC (GNU Compiler Collection)
- GNU Make
- QEMU (for emulation)
- GDB (GNU Debugger)

### Steps to Recreate the Environment

1. **Download and Extract the Project**:
   - Download the `yul.tar.gz` file.
   - Extract the contents using the following command:
     ```bash
     tar -xzvf yul.tar.gz
     ```
   - This will create a directory containing the xv6 source code and your modified files.

2. **Navigate to the xv6 Directory**:
   ```bash
   cd path_to_extracted_folder
   ```
3. Run the following command to compile the xv6 OS:
``` bash
make all
```
- This command will generate the necessary binaries, including xv6.img and fs.img.

4. Run xv6 Using QEMU:
``` 
make qemu
```

## Implementation Details

### System Call: `getnextpid()`
- **Description**: This system call returns the next available PID in the system. It retrieves this information from the `nextpid` variable in `proc.c`.
- **Implementation**:
  - Added the system call handler in `sysproc.c`.
  - Updated the system call table in `syscall.c`.
  - Declared the function in `defs.h` and `sysfunc.h`.
  - Exposed the system call to user programs via `syscall.h` and `user.h`.

### System Call: `getprocstate(int pid, char* state, int n)`
- **Description**: This system call retrieves the state of the first process with the specified PID and copies the state string into the provided buffer. The state is returned as a string similar to the output of `procdump()`. If the buffer size is insufficient or the process is not found, the system call returns `-1`.
- **Implementation**:
  - Added the system call handler in `sysproc.c`.
  - Updated the system call table in `syscall.c`.
  - Declared the function in `defs.h` and `sysfunc.h`.
  - Exposed the system call to user programs via `syscall.h` and `user.h`.

### Files Modified

- **`sysproc.c`**: Added the core logic for the new system calls.
- **`syscall.c`**: Added entries for the new system calls in the system call function pointer array.
- **`defs.h`**: Added function declarations for the new system calls.
- **`sysfunc.h`**: Declared the new system call functions for use within the kernel.
- **`syscall.h`**: Exposed the system calls to user-space programs.
- **`user.h`**: Updated to allow user programs to call the new system calls.
- **`usys.S`**: Added the system call numbers for the new system calls in assembly.

### Debugging with `gdb`
- **Objective**: Demonstrated the use of `gdb` to debug the xv6 kernel by setting breakpoints and inspecting variables within the `fdalloc()` function.
- **Steps**:
  1. Compiled xv6 with the `-Og` flag for debugging.
  2. Set up `qemu` with `gdb` support using `make qemu-nox-gdb`.
  3. Set a breakpoint in the `fdalloc()` function and inspected the return value of `fd`.
  4. Captured a screenshot of the `gdb` session, showing the value of `fd` and the user’s login name.

## Adding New System Calls and User Programs

### Where to Add System Calls

- **`sysproc.c`**: Implement the system call logic, especially if accessing the process table (`ptable`).
- **`syscall.c`**: Add your system call to the array of function pointers to handlers.
- **`defs.h`**: Add the function prototype.
- **`sysfunc.h`**: Declare the system call functions.
- **`usys.S`**: Add the system call numbers in the `user` folder.
- **`syscall.h`**: Add the system call definitions in the `include` folder.
- **`user.h`**: Add system call definitions for user space.

### Where to Add Tester User Programs

- **Makefile**: Update the `Makefile` under the `user` folder to include the new test programs.
- **`test.c`**: Add the user-space test programs in the `user` folder.

## Development Environment

- **Language**: C
- **Platform**: xv6 (Unix-like operating system)
- **Tools**: GCC, GDB, QEMU
