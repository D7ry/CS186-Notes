## Storage Media

Most database were designed for magnetic disks.

API for Reads and Writes  
- READ: transfer "page" from disk to RAM
- WRITE: transfer "page" from RAM to disk

Both operations are expensive.

The good side: API minimizes C pointer errors.

## Flash Drive
**NAND flash** allows very fine-grained reads but coarse-grained writes.  
Flash Drives age quickly where writes are performed.  
Sequencial reads and random reads are just as fast, unlike magnetic disks.  
Writes are just as slow as magnetic disks, with random writes being much slower. 

# Block Level Storage
**Block**: minimal unit for disk I/O. a.k.a. **page**.  
Goal: amortize seek and write delays

## predicting future behavior
Predicting future behavior helpes to reduce # of seeks and writes
- catching popular blocks
- pre-fetch likely-to-be-accessed blocks
- buffer writes to sequential blocks

# Disk Space Management
DSM is hte lowest layer of DBMS, manages space on disk.
Purpose:
- Map pages to locations on disk
- load pages from disk to memory
- Save pages back to disk

# File Organization

## Example: Unordered Heap Files
Collection of records with no particular order

Implementation:
- a header page with metadata
- each page contains:
  - 2 pointers
  - data
  - free space

Problem: inefficient linear traversal when looking up free space  
Fix: **page directory**

Directory entries stores the # of free bytes on the referenced page.  
A single load of the header page is sufficient to find a free page.

## Page Layout
### Header Page
- Number of records
- free space
- bitmaps
- ...
