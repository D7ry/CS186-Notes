# Sorting
To analyze sorting performance in databases, we only cound the number of I/Os needed.

## Two Way External Merge Sort

A **Sorted Run** is a sequence of records that are sorted.

A **Pass** is an I/O access across all the datas need to be sorted. Thus for a database with N pages, there are 2*N I/O's needed: 1 for reading and 1 for writing.

The total number of I/O's needed in a two way external merge sort is:  
2N * (1 + log<sub>2</sub>(N))  
1 -> the initial pass to sort individual pages  
log<sub>2</sub>(N) -> the number of passes needed to merge the pages  
2N -> the number of I/O's done in each pass