# Joining
Below is a list of different methods of joining and their I/O costs.   

*the IO costs don't include write-back, as it will be discussed in later chapters*

## Nested Loop Join(NLJ)
NLJ functions like the naive double-for loop we write in our first years of programming.

### Simple Nested Loop Join(SNLJ)
Algorithm:
```
for each record r in R:
	for each record s in S:
		if condition(r, s):
			output(r, s)
```
I/O cost: # of pages in R + (# of records in R * # of pages in S)

### Page Nested Loop Join(PNLJ)
Algorithm:
```
for each Page p in R:
	for each Page q in S:
		for each record i in p:
			for each record j in q:
				if condition(i, j):
					output(i, j)
``` 
I/O cost: R.numPages * (1 + S.numPages)

### Block Nested Loop Join(BNLJ)
Assuming we have B buffer pages in memory, SNLJ and PNLJ only use 3 pages at a time: for R, S and output.   
In BNLJ, we use B-2 pages to store R as a **block**, and match S with all records in the buffer block, which cuts I/O significantly.

Algorithm:
```
block_pages = B - 2
for each block_pages b in R:
	for each page s in S:
		for each record i in b:
			for each record j in s:
				if condition(i, j):
					output(i, j)
```

for BNLJ, we scan all of S for each block of R, so the I/O cost is:  

R.numPages + (R.numPages / block_pages) * S.numPages

### Index Nested Loop Join(INLJ)
If we have an index on R and S on the join attribute, we can use the index to reduce the I/O cost of BNLJ.

Algorithm:
```
for each r in R:
	for each s in S:
		if r.join_attr == s.join_attr:
			output(r, s)
```

I/O cost: R.numPages + R.numRecords * (time to look up matching records in S)

## Hash Join

Hash table allow us to look for matching records super fast!

### Naive Hash Join

Assumine we have unlimited buffer pages and unlimited memory, the I/O cost will simply be R.numPages + S.numPages.

### Grace Hash Join

Instead of loading everything into the buffer(which is impractical), we repeatedly hash r in R and s in S into buffers until either r or s is <= than B-2 pages. Then we do a naive hash join on the buffer.

I/O cost: cost of hashing + cost of naive hash join

## Sort-Merge Join

Sort-merge join is helpful for joins on numeric attributes. The process is as follows:
1. Sort both R and S on the join attribute
2. Have 2 pointers for each R and S, and compare the values they point to. If they are equal, output the pair and the following values that are also equal. If one is smaller, move the pointer forward. If both are smaller, move both pointers forward.
3. Repeat step 2 until one of the pointers reaches the end of the table.

#### I/O cost: 
Average case:  
cost of sortingR and S + R.numpages + S.numPages  
Worst case:  
cost of sorting R and S + R.numpages + R.numRecords * S.numPages
The worst case happens when every r matches every s.