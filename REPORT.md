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
-Associativity: varied from 1,2,4,8
-Replacement policy: same throughout "LRU"

This allowed us to track misses, total accesses, and miss rate for each configuration used.
This was then submitted into google docs to allow for better understanding of the info gathered.

Graph of cache Performance: 
From this data we obtained a graph for the program matrix_col using the full cache LI and LRU replacement policy.


Through the graph we were able to determine that the miss rate decreases as the chche size increases for all associativity leevels. We also find that increasing associativity also lowers the miss rate but the improvement becomes smaller at higher associativity especially when going from 4 way to 8 way.

#cost model

In order to compare the performance against the cost we used the model given inside the lab.

#Cache Size Cost 
As the cache size doubles the cost doubles 

-1024 = 1 
-2048 = 2
- 4096 = 4
- 8192 = 8
- 16384 = 16

### Associativity cost
when doubling associativivity we have an increase cost by 10%

- 1-way = 1.00
- 2-way = 1.10
- 4-way = 1.21
- 8-way = 1.331

###Total cost

for a single cache we have 
cost = size factor * associativity factor 

so for example if we have 4-way 8192 then 
8*1.21 = 9.68

For split caches the total cost is the sum of the data cache cost and instruction cache cost.

## Best Full Cache Configuration

When using LI traces we found the lowest miss rate full cache configuratiton for all foru programs was:

8 way, 16384, LRU

which resulted in 

-hello_c = 3.95%
- hello_cpp = 1.60%
- matrix_row = 1.47%
- matrix_col = 1.47%

We found through the data collected that this gave the best raw performance but was also the most expesnive full cache option.

## Best Cost-Effective Full Cache

The best strong cost effective full cache configuration was

4-way, 8192, LRU

Results:
- hello_c = 4.74%
- hello_cpp = 2.19%
- matrix_row = 1.95%
- matrix_col = 1.95%

Cost:
- 8 × 1.21 = 9.68

Using this configuration allowed slight higher miss rates than the largest 8-way cache but it cost less and still did very well.

## Split Cache Analysis
We then compared split caches with a full cache and we combined the data cache and instruction cache results:

combined miss rate = (L misses + I misses) / (L accesses + I accesses)

A strong split-cache configuration from my results was:

- Data cache:** 4-way, 8192, LRU
- Instruction cache:** 4-way, 8192, LRU

### Combined split-cache results

#### hello_c
- Data misses = 2881
- Instruction misses = 4470
- Total accesses = 32778 + 147766 = 180544
- Combined miss rate = (2881 + 4470) / 180544 = 4.07%

#### hello_cpp
- Data misses = 30901
- Instruction misses = 8392
- Total accesses = 491731 + 1873757 = 2365488
- Combined miss rate = (30901 + 8392) / 2365488 = 1.66%

#### matrix_row
- Data misses = 37282
- Instruction misses = 9533
- Total accesses = 727798 + 2366618 = 3094416
- Combined miss rate = (37282 + 9533) / 3094416 = 1.51%

#### matrix_col
- Data misses = 37237
- Instruction misses = 9518
- Total accesses = 727787 + 2366575 = 3094362
- Combined miss rate = (37237 + 9518) / 3094362 = 1.51%

### Split-cache cost
Each 4-way, 8192 cache costs:

8 × 1.21 = 9.68

So total split-cache cost is:

9.68 + 9.68 = 19.36

## Full Cache vs Split Cache

The best full-cache configuration we tested was:

- 8-way, 16384, LRU
- Cost = `21.296`

The split-cache configuration above was:

- 4-way, 8192 data + 4-way, 8192 instruction
- Cost = `19.36`

This split configuration was cheaper and had miss rates very close to the best full-cache configuration. In every program, the split cache stayed close to the best full-cache miss rate while using less total cost. This makes it a very good cost-performance tradeoff.

## Observations Across the Four Programs

A few consistent trends appeared in all four executables:

- Increasing cache size always reduced miss rate.
- Increasing associativity also reduced miss rate, but the improvement became smaller at higher associativity.
- The difference between 4-way and 8-way associativity was often small compared to the added cost.
- Instruction caches had much lower miss rates than data caches, especially at larger cache sizes.
- hello_cpp had a much larger trace than hello_c, but both showed the same overall cache trends.
- matrix_row and matrix_col had very similar miss-rate behavior in my results.
- The graph clearly shows diminishing returns when associativity increases from 4-way to 8-way.

## Best Overall Configuration

The best absolute performance came from:

- 8-way, 16384, LRU

However, the best overall cost-effective configuration** was:

- Split cache: 4-way, 8192 data cache + 4-way, 8192 instruction cache, both using LRU

I chose this configuration because it gave miss rates that were very close to the best full-cache results while costing less.

## Conclusion

This lab showed that larger caches and higher associativity reduce miss rate, but the lowest miss rate is not always the best design once cost is considered. Although an 8-way, 16384 full cache gave the best raw performance, a split-cache design with 4-way, 8192 instruction and data caches was the best balance between low cost and good performance. Overall, the results showed clear diminishing returns at very large cache sizes and high associativity, which is why cost-effective cache design matters.
