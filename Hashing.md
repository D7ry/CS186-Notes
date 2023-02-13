# Hashing

Directly hashing a database is just as impractical as sorting due to memory limitations. We can resort to divide-and-conquer strategy to solve this problem by splitting the database into smaller chunks and hashing each chunk separately. 

This comes with one problem: if the key is shared in multiple hash tables, we need to make sure that every occurence of the key is loaded in the currenrt memory and can be hashed into the same bucket.

# Algorithm

## Divide
Assume we have B buffer frames available; 1 frame is used for input buffer and B-1 frames are used for output. This allows us to create B-1 output buffers at a time.

# Conquer
Now look at the divided buckets. For buckets whose size <= B, we can directly load it into the memory and start vuilding hash table. For buckets whose size > B, we need to divide them into smaller buckets and repeat the process, using a different hashing function.

# Drawbacks of Hashing

# Data Skew

Data skew happens when the values being hashed are not uniformly distributed; this leads to varying size of buckets and is sub-optimal for hashing.

# Analyzing Hashing performance

To analyze hashing performance, we count the number of I/Os of each divide phase and the conquer phase.

## Divide Phase:
```math
I/O = \sum_{i=1}^{m} r_i + w_i
```
where m is the total number of dividing phase required, r<sub>i</sub> is the number of pages to read for pass i, and w<sub>i</sub> is the number of pages to write for pass i.

## Conquer Phase:
```math
I/O = 2X
```
Where X is the number of pages we have after partitioning that we need to build the hash table out of.