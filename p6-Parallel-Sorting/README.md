# Parallel Sort (`psort`)

This project implements a parallel sorting application in C, designed to sort large datasets efficiently using multiple threads. The project is part of the CS537 Operating Systems course at the University of Wisconsin-Madison. Objectives:
- Gain familiarity with Linux pthreads.
- Learn to parallelize a program.
- Optimize the program for high performance.


## Project Overview

The objective of this project is to develop a high-performance parallel sorting program called `psort`. The program reads records from an input file, sorts them based on a key, and writes the sorted records to an output file. The sorting operation is parallelized across multiple threads to leverage multi-processor systems and improve performance.

## Features

- **Parallel Sorting**: The program divides the input data into segments, each of which is processed by a separate thread using the merge sort algorithm. After sorting, the segments are merged into a final sorted array.
- **High Performance**: The program is optimized for speed by using parallel processing, minimizing I/O overhead, and implementing an efficient sorting algorithm.
- **Configurable Threads**: The number of threads used for sorting is specified by the user as a command-line argument.

## Usage

### Command-Line Arguments

The `psort` program accepts three command-line arguments:

```bash
./psort <input_file> <output_file> <num_threads>
```
- `input_file`: The path to the input file containing the records to be sorted.
- `output_file`: The path to the output file where the sorted records will be written.
- `num_threads`: The number of threads to be used for sorting.

### Example
```bash
./psort input.txt output.txt 4
```
This command sorts the records in input.txt using 4 threads and writes the sorted records to output.txt.

## Implementation Details

### Step 1: Parsing Input into an Array
- The input file is read into memory as an array of records. Each record is 100 bytes in size, with the first 4 bytes representing the key used for sorting.
### Step 2: Segregating the Array for Multiple Threads
The array is divided into segments based on the number of threads specified by the user. If the number of threads exceeds the number of records, the program adjusts to use fewer threads.
### Step 3: Sorting the Segments
Each segment is sorted using the merge sort algorithm, which is implemented in a separate function called mergeSort. Multiple threads are created to sort the segments concurrently.
### Step 4: Merging the Segments
After sorting, the segments are merged into a single sorted array. The final merge is performed by the main thread after all sorting threads have completed.
### Step 5: Writing the Sorted Records to Output
The sorted records are written to the specified output file. The program ensures that the output is flushed to disk before completing.
### Performance Measurement
The program measures and prints the time taken to perform the sorting operation (excluding I/O). This helps in evaluating the performance benefits of parallelization.


## Performance Evaluation
To evaluate the performance of psort, measure the execution time of the sorting operation with varying numbers of threads (e.g., 1, 2, 4, 8, 12, 16) on a multi-processor machine. Plot the execution time against the number of threads to observe how parallelization affects performance.

## Compilation
To compile the psort program, use the following command:

```bash
gcc -Wall -Werror -pthread -O -o psort psort.c
```
- `-Wall`: Enables all compiler's warning messages.
- `-Werror`: Treats all warnings as errors.
- `-pthread`: Links the pthread library for multi-threading.
- `-O`: Enables optimization.

## Execution
Run the program using the following command:
```bash
./psort <input_file> <output_file> <num_threads>
```
