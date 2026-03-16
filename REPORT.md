#Lab 6 Caches

Name: Nathan Herrera 
Email: nherr044@ucr.edu 

In this lab we found how cache size and associativity affects miss rates.
We used this to be able to find a cost effective cache design. We were able to use Valgrind memory Traces from the given executables:
- hello_c
- hello_cpp
- matrix_row
- matrix_col

For each program I tested 
-full cache traces = LI
-data cache traces = L
-instruction cache traces = I 

We then varied cache size and associativity. We only used the LRU replacement policy for the main tests we ran.

#Steps 

We first built the executables using CMake and we then ran valgrind lackey to gather the memeory traces.
After this we then used the filter given with 'awk' into these:

-LI = full caches
-L = data caches 
-I = instruction cache

We then simulated the cache in verilog while switching the parameters:

-Cache size: varied from 1024, 2048, 4096, 16384
-Associativity: varied from 1,2,3,4,8
-Replacement policy: same throughout "LRU"

This allowed us to track misses, total accesses, and miss rate for each configuration used.
This was then submitted into google docs to allow for better understanding of the info gathered.

Graph of cache Performance: 
From this data we obtained a graph for the program matrix_col using the full cache LI and LRU replacement policy.


