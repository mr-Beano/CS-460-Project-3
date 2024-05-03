# CS 460 Operating Systems Project 3 - Improving the xv6 File System
Dean Feller 3/12/2024

## Inode Improvements
By default, xv6 uses an inode-based filesystem where each inode contains 12 direct data block pointers and 1 single-indirect block pointer. Each block “pointer” is a 4-byte disk block number (indexed from the first sector on the drive), and each block can hold 512 bytes of data. This means the indirect block can hold 128 block pointers, for a maximum file size of 12 + 128 = 140 blocks, or around 70 KB.

For this project, we had to change the inode structure to contain 11 direct block pointers, 1 single-indirect block pointer, and 1 double-indirect block pointer. This allows us to address 11 + 128 + 128² = 16523 blocks, or just over 8 MB.

## Modified Files:
- Makefile - added bigtest user program
- bigtest.c - increased amount of sectors to write
- file.h - increased amount of addrs in inode struct
- fs.c - added double-indirect address retrieval to bmap()
- fs.h - added macro for number of double-indirect blocks, updated MAXFILE to include it. Increased amount of addrs in dinode struct
- param.h - increased FSSIZE

## Run Instructions

### Running xv6
- Use `make qemu-nox` to start xv6
- In another terminal, use `pkill qemu` to stop xv6
- Use `make clean` to remove generated files

### Testing the Improvements
In xv6, run the `bigtest` program, which will attempt to write and then read the largest file it can. When it finishes, it should report 16523 sectors written.