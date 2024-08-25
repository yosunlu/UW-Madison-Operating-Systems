# xv6 Copy-on-Write Fork - Project 5

This repository contains the implementation of a copy-on-write (CoW) fork for the xv6 operating system, developed as part of the Operating Systems course (CS537) at the University of Wisconsin-Madison. Objectives:
- Gain familiarity with xv6 memory management.
- Implement an efficient copy-on-write fork.
- Utilize debugging techniques to verify implementation correctness.

## Project Overview

The goal of this project was to modify the existing fork() system call in xv6 to implement copy-on-write (CoW) memory management. The default fork() in xv6 performs a complete copy of the parent process's memory, which is inefficient, especially when the child process immediately calls exec(). CoW defers copying memory pages until they are actually written to, optimizing performance.

## Features

### Copy-on-Write (CoW) Implementation

- **CoW Memory Management**: 

  - Implemented a new function called `cowuvm()` in `vm.c`, which is responsible for setting up the page table entries after a `fork()` system call. Instead of duplicating the memory pages, `cowuvm()` marks the pages as read-only and shared between the parent and child processes. This setup defers the actual memory copying until either process attempts to write to a shared page.
  - The function modifies the page table entries of both the parent and the child process to point to the same physical pages. However, these pages are marked as read-only. This ensures that a write operation on these pages triggers a page fault, which is handled by the CoW mechanism.

- **Page Fault Handler**:
  - Modified the `trap()` function in `trap.c` to handle page faults specifically related to CoW. A new handler, `cowHandler()`, was implemented to manage these page faults.
  - When a process attempts to write to a read-only page (shared with its parent or child), the CoW page fault handler is triggered. The handler checks if the page is shared by multiple processes (by examining the reference count). If so, it allocates a new physical page, copies the contents from the old page, and updates the page table to point to the new writable page. If the page is not shared, it simply updates the page table entry to make the page writable again.

- **Page Reference Counting**:
    - Added a reference count to each physical page in `kalloc.c` to track how many processes share the same page. This count is incremented whenever a new process shares the page (e.g., after a `fork()`) and decremented when a process no longer needs the page (e.g., after creating a private copy due to a page fault).
    - The reference count mechanism ensures that physical memory pages are only freed and returned to the free list when they are no longer in use by any process.

### System Calls

- **`getFreePagesCount()`**: 
  - Added a system call that returns the total number of free pages in the system. This is useful for debugging and verifying the CoW implementation.

## Implementation Details

### cowuvm()
The `cowuvm()` function is central to the CoW implementation. It is responsible for setting up the page tables during a `fork()`, ensuring that both the parent and the child process share the same physical pages in a read-only mode. The function iterates over the parent process's memory and marks each page as read-only, updating the page tables accordingly.

After setting up the shared pages, `cowuvm()` updates the TLB (Translation Lookaside Buffer) to ensure that the changes are recognized by the CPU.

### trap() and cowHandler()
The `trap()` function in `trap.c` was extended to handle page faults that occur due to write attempts on read-only pages. The new `cowHandler()` function is invoked when a `T_PGFLT` (page fault) trap is triggered.

`cowHandler()` checks the cause of the page fault and determines whether the faulting page is a shared read-only page. If it is, the handler either duplicates the page (if it is shared by multiple processes) or makes it writable (if it is no longer shared).

### Reference Counting in kalloc.c
To manage shared pages effectively, a reference count was added to each physical page in `kalloc.c`. This count tracks the number of processes using the page. The reference count is incremented whenever a new process shares the page and decremented when a process releases it.

Pages are only freed when their reference count drops to zero, ensuring that no process is using the page.

### TLB Flushing
Whenever the page table entries are modified (e.g., when a page is made writable after a CoW page fault), the TLB is flushed to ensure that the CPU's cached page table entries are invalidated. This prevents the CPU from using outdated or incorrect page mappings.

## Recreating the Environment

### Prerequisites
To work with the xv6 operating system and the modified `fork()` implementation, you need the following tools installed on your Unix/Linux environment:

- GCC (GNU Compiler Collection)
- GNU Make
- QEMU (for emulation)
- GDB (GNU Debugger)

### Steps to Recreate the Environment

1. **Download and Extract the Project:**
   - Download the `P5.tar.gz` file.
   - Extract the contents using the following command:
     ```bash
     tar -xzvf P5.tar.gz
     ```
   - This will create a directory containing the xv6 source code and your modified files.

2. **Navigate to the xv6 Directory:**
   ```bash
   cd path_to_extracted_folder
   ```


3. Compile the xv6 Operating System:
    - Run the following command to compile the xv6 OS:
```bash
make all
```
    - This command will generate the necessary binaries, including xv6.img and fs.img.
    - Run xv6 Using QEMU:
4. To run the xv6 operating system using QEMU, use the following command:
``` bash
make qemu
```
    - This will start the xv6 OS in the QEMU emulator. You can interact with the system using the terminal.

## Testing

### Default Tests:
    - The default forktest provided in xv6 should pass after implementing CoW.


## Makefile Usage

The provided Makefile automates the process of building and running the xv6 operating system. Here’s a brief overview of how to use it:

### Build xv6:
This is the default target and can be invoked by running make or make all. It compiles the kernel and user programs and creates the xv6.img and fs.img files.
```bash
make all
```
### Clean the Build:
Use this target to remove all generated files, including object files, binaries, and the filesystem image.
``` bash
make clean
```

### Run xv6 in QEMU:
To run xv6 in the QEMU emulator with a graphical interface (if available):
```bash
make qemu
```
- To run xv6 in QEMU without a graphical interface (serial mode only):
```bash
make qemu-nox
```
- To run xv6 in QEMU with GDB support for debugging:
``` bash
make qemu-gdb
```
- To run xv6 in QEMU without a graphical interface and with GDB support:
```bash
make qemu-nox-gdb
```

