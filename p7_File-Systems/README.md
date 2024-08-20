# ext2 Disk Image Scanner - Project 7

This repository contains the implementation of a disk image scanner for the ext2 file system, developed as part of the Operating Systems course (CS537) at the University of Wisconsin-Madison.

## Project Overview

The goal of this project was to develop a program named `runscan` that scans an ext2 disk image to recover JPEG files, including those that have been deleted. The recovered files are saved in a specified output directory along with their metadata. Objectives:
- Gain experience with file system structures.
- Learn to use file system utilities such as `mkfs` and `debugfs`.
- Implement a tool to recover JPEG files from an ext2 disk image, including deleted files.


## Features

### JPEG File Recovery

- **File Identification**: 
  - The program scans all inodes that represent regular files and checks if the first data block of the inode contains the JPEG magic numbers (`FF D8 FF E0`, `FF D8 FF E1`, `FF D8 FF E8`).
  - If an inode represents a JPEG file, the content of the file is copied to the output directory.

- **File Naming**:
  - Two copies of each recovered JPEG file are saved in the output directory:
    - One copy is named after the inode number (e.g., `file-18.jpg`).
    - The other copy is named after the actual file name found in the directory structure.

- **File Metadata**:
  - A third file is created for each recovered JPEG, containing metadata such as the number of links, file size, and the owner’s user ID. This file is named `file-<inode>-details.txt`.

### Directory Traversal

- The program traverses the directory structure within the disk image to locate and recover files, handling both undeleted and deleted files.
- If the disk image contains a directory named `top_secret`, only the JPEG files under this directory are recovered.

## Implementation Details

### Main Program Flow (`runscan`)

1. **Disk Image Initialization**:
   - The disk image is opened and the superblock and group descriptor are read.
   - The program calculates the number of groups in the disk image and begins scanning each inode.

2. **JPEG File Detection**:
   - For each inode, the program reads the first data block and checks for the JPEG magic numbers to identify JPEG files.
   - If a JPEG file is detected, the program proceeds to create copies in the output directory.

3. **File Recovery**:
   - **File with Inode Name**: The content of the file is copied to `file-<inode>.jpg`.
   - **File with Actual Name**: The program searches the directory entries to find the actual file name and creates a copy with this name.
   - **Metadata**: The program writes the file’s metadata to `file-<inode>-details.txt`.

### Helper Functions

- **`name_search()`**:
  - This function scans the directory entries to find the actual file name associated with a given inode number.
  
- **`data_copy()`**:
  - This function copies the data blocks of the file from the disk image to the output file.
  
- **`write_detail()`**:
  - This function writes the file's metadata (link count, file size, user ID) to a text file.

### Details:
1. Open an img file and get related information: 
 	- Read first the super-block and group-descriptor. 

		`read_super_block(fd, 0, &super);`
    
	 - Superblock and groupblock are the same for each group, so only need to read it once and store it in an adress

		`read_group_desc(fd, 0, &group);`

2. For each group, loop through its inode table

    Each group has its own inode table. For each group, loop through its inode table
    `read_inode()`'s 2nd parameter is offset to the inode table of the group, 3rd parameter is the inode number within that group's inode table, idx + 1 bc inode number starts at 1, but idx starts at 0, the final offset will be `[(offset to the inode table of the group) + (idx + 1 - 1) * inode_size]`
    
	For example: offset to 2nd group's inode table is 2000;
    to reach the 1st inode of the group: `2000 + ((0+1)th - 1) * inode_size`
 
	- Create the jpg file with inode number (e.g. file-14.jpg)
	- Create the jpg file with actual name (e.g. indonesian-flag-extra-small.jpg)
	- Create the txt file with details (e.g. file-14-details.txt)

## Recreating the Environment

### Prerequisites

To compile and run the `runscan` program, you need the following tools installed on your Unix/Linux environment:

- GCC (GNU Compiler Collection)
- GNU Make

### Steps to Compile and Run

1. **Download and Extract the Project**:
   - Extract the project files and navigate to the `starter_code` directory.
  
2. **Compile the Program**:
   - Run the following command to compile the program:
     ```bash
     make
     ```
   - This command compiles `runscan.c` along with the necessary dependencies.

3. **Run the Program**:
   - To run the program and recover JPEG files from a disk image, use the following command:
     ```bash
     ./runscan <input_disk_image> <output_directory>
     ```
   - Example:
     ```bash
     ./runscan ../test_disk_images/image-02 output_dir
     ```

## Testing

### Running Test Cases

- To test the program with provided test cases, run:
  ```bash
  ./runscan ../test_disk_images/test_0/image-02 ./output_test_0
  ```

### Verifying Output Files
- python3 ../rcheck.py ./output0 /home/cs537-1/tests/P7/test_disk_images/test_0/output

