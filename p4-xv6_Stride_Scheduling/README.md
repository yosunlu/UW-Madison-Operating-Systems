# xv6 Scheduler Extension

This repository contains the implementation of a stride-based scheduler for the xv6 operating system, developed as part of the Operating Systems course (CS537) at the University of Wisconsin-Madison. Objectives:
- Gain familiarity with the xv6 codebase.
- Learn to implement a stride scheduling algorithm.
- Visualize the scheduling behavior through graphing.

## Project Overview

The goal of this project was to modify the existing scheduler in the xv6 operating system to implement a stride scheduling algorithm. This scheduling algorithm allocates CPU time to processes based on the number of tickets assigned to each process. The more tickets a process has, the more CPU time it receives.

## Features

### Scheduling

- **New Attributes in `struct proc`**: Added the following attributes to `struct proc` in `proc.h`:
  - `int init_stride`: The initial stride, calculated by `MAXSTRIDE / tickets`.
  - `int tickets`: The number of tickets provided by the user; fixed once given.
  - `int passvalue`: The value used to determine which process to execute; the lower the better.
  - `int ticks`: The number of times this process has been executed.

- **Max Stride**: Added `MAXSTRIDE` in `param.h`, a constant that should be a random number used in stride calculations.

- **New System Calls**:
  - `int settickets(int number)`: Sets the initial number of tickets for a process.
  - `int sys_getpinfo(void)`: Returns information about the currently running processes.

- **New Header File**: Added `pstat.h` in the `include` folder to define the structure for process information.

- **Scheduler Modifications**:
  - **`scheduler()` in `proc.c`**: Modified to implement the stride scheduling algorithm.
  - **`allocproc()` in `proc.c`**: Modified to initialize the new attributes in `struct proc`.
  - **`fork()` in `proc.c`**: Modified so that the child process inherits the parent's `init_stride` and `tickets`. The `passvalue` is also set to the initial stride.

### Implementation Details

#### Where to Add New System Calls

- **`sysproc.c`**: Implement the core logic for the new system calls, especially if accessing the process table (`ptable`).
- **`syscall.c`**: Add entries for the new system calls in the array of function pointers to handlers.
- **`defs.h`**: Add function prototypes for the new system calls.
- **`sysfunc.h`**: Declare the new system call functions for internal kernel use.
- **`syscall.h`**: Expose the system calls to user programs.
- **`user.h`**: Add system call definitions for user space.
- **`usys.S`**: Add system call numbers in the `user` folder.

#### Where to Add Tester User Programs

- **`Makefile`**: Update the `Makefile` under the `user` folder to include the new test programs.
- **`test.c`**: Add the user-space test programs in the `user` folder.

### Graphical Representation

In addition to the code, a graph was created to illustrate the behavior of the stride scheduler. The graph shows the pass value of two processes over 10 time slots, where the processes have a 3:2 ratio of tickets (e.g., process A might have 3 tickets, and process B 2). The max_stride and tickets assigned to each process are specified in the graph. This graph demonstrates how the stride scheduler balances CPU time between processes based on their ticket allocations.

## Recreating the Environment

### Prerequisites
To work with the xv6 operating system and the modified scheduler, you need the following tools installed on your Unix/Linux environment:

- GCC (GNU Compiler Collection)
- GNU Make
- QEMU (for emulation)
- GDB (GNU Debugger)

### Steps to Recreate the Environment

1. **Download and Extract the Project**:
   - Download the `p4.tar.gz` file.
   - Extract the contents using the following command:
     ```bash
     tar -xzvf p4.tar.gz
     ```
   - This will create a directory containing the xv6 source code and your modified files.

2. **Navigate to the xv6 Directory**:
   ```bash
   cd path_to_extracted_folder
   ```
3. **Compile the xv6 Operating Syste**:
Run the following command to compile the xv6 OS:
```bash
make all
```
This command will generate the necessary binaries, including xv6.img and fs.img.  

4. Run xv6 Using QEMU:
To run the xv6 operating system using QEMU, use the following command:
``` bash
make qemu
```
This will start the xv6 OS in the QEMU emulator. You can interact with the system using the terminal.

## Testing the Scheduler
To test the stride scheduler, you can write user-level programs that set different ticket values using settickets(int number) and observe how the scheduler allocates CPU time by calling sys_getpinfo().



