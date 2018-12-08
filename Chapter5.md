# Memory Technologies
* Static RAM (SRAM)
  * 0.5ns - 2.5ns, $2000 - $5000 per GB
* Dynamic RAM (DRAM)
  * 50ns - 70ns, $20 - $75 per GB
* Magnetic disk
  * 5ms - 20ms, $0.20 - $2 per GB
* Ideal memory
  * Access time of SRAM
  * Capacity and cost/GB of disk
## DRAM Technology
* Data stored as a charge in a capacitor (single capacitor per bit)
  * Single transistor used to access the charge
  * If power is lost, data is lost
  * Must be periodically refreshed
    * Read contents and write back
    * Performed on a DRAM "row"
## Flash Storage
* Nonvolatile simiconductor storage (does not lose data if power is lost)
  * 100x - 1000x faseter than disk
  * Smaller, lower power, more robust
## Disk Storage
* Nonvolatile, rotating magnetic storage
## Principle of Locality
* Programs access a small proportion of their address space at any time
* Temporal locality
  * Items accessed recently are likely to be accessed again soon
  * Eg. instructions in a loop, induction variables
* Spatial locality
  * Items near those accessed recently are likely to be acced soon
  * Eg. sequential instruction access, array data
## Taking Advantage of Locality
* Memory hierarchy
* Store everyting on disk
* Copy recently accessed (and nearby) items from disk to smaller DRAM memory
  * Main memory
* Copy more recently accessed (and nearby) items from DRAM to smaller SRAM memory
  * Cache memory attached to CPU
## Memory Hierarchy Levels
* Block (aka line): unit of copying
  * May be multiple words
* If accessed data is present in upper level
  * Hit: access satisfied by upper level
    * Hit ratio: hits/accesses
* If accessed data is absent
  * Miss: block copied from lower level
    * Time taken: miss penalty
    * Miss ratio: misses/accesses = 1 - hit ratio
  * Then accessed data supplied from upper level
# Cache Memory
* Level of memory hierarchy closest to CPU
* How do we know if data is present in cache? Where do we look?
## Direct Mapped Cache
* Location determined by address
* Direct mapped: only one choice
  * (Block address) modulo (#blocks in cache)
* #Blocks is a power of 2
* Use low-order address bits
## Tags and Valid Bits
* How do we know which particular block is stored in a cache location?
  * Store block address as well as the data
  * Actually, only need the high-order bits
  * Called the tag
* What if there is no data in a location?
  * Valid bit: 1 = present, 0 = not present
  * Initially 0
## Example: Larger Block Size
* 64 blocks, 16 bytes/block
  * To what block number does address 1200 map?
* Block address = floor(1200/16) = 75
* Block number = 75 modulo 64 = 11
## Block Size Considerations
* Larger blocks should reduce miss rate
  * Due to spatial locality
* But in a fixed-size cache
  * Larger blocks -> fewer of them
    * More competition -> increased miss rate
  * Larger blocks -> pollution
* Larger miss penalty
  * Can override benefit of reduced miss rate
  * Early restart and critical-word-first can help
## Cache Misses
* On cache hit, CPU proceeds normally
* On Cache miss
  * Stall the CPU pipeline
  * Fetch block from next level of hierarchy
  * Instruction cache miss
    * Restart instruction fetch
  * Data cache miss
    * Complete data access
## Write-Through
* On data-write hit, could just update the block in the cache
  * But then cache and memory would be inconsistent
* Write through: also update memory
* But makes writes take longer
  * Eg. if base CPI = 1, 10% of instructions are stores, write to memory takes 100 cycles
    * Effective CPI = 1 + 0.1 * 100 = 11
* Solution: write buffer
  * Holds data waiting to be written to memory
  * CPU continues immediately
    * Only stalls if write buffer is already full
## Write-Back
* Alternative: on data-write hit, just update the block in cache
  * Keep track of whether each block is dirty
* When a dirty block is replaced
  * Write it back to memory
  * Can use a write buffer to allow replacing block to be read first
## Write Allocation
* What should happen on a write miss?
* Alternatives for write-through
  * Allocate on miss: fetch the block
  * Write around: don't fetch the block
    * Since programs often write a whole block before reading it (eg. initialization)
* For write-back
  * Usually fetch the block
## Measuring Cahce Performance
* Components of CPU time
  * Program execution cycles
    * Includes cache hit time
  * Memory stall cycles 
    * Mainly from cache misses
* With simplifying assumptions:
  * Memory stall cycles = (Memory accesses / program) * Miss rate * miss penalty
  * = (instructions / program) * (misses / instruction) * miss penalty
## Cache Performance Example
* Given
  * I-cache miss rate = 2%
  * D-cache miss rate = 4%
  * Miss penalty = 100 cycles
  * Base CPI (ideal cache) = 2
  * Load and stores are 36% of instructions
* Miss cycles per instructions
  * I-cache: 0.02 * 100 = 2
  * D-cache: 0.36 * 0.04 * 100 = 1.44
* Actual CPI = 2 + 2 + 1.44 = 5.44
  * Ideal CPU is 5.44 / 2 = 2.72 times faster
## Average Access Time (AMAT)
* Hit time is also important for performance
* AMAT = hit time + miss rate * miss penalty
### AMAT Example
* CPU with 1ns clock, hit time = 1 cycle, miss penalty = 20 cycles, I-cache miss rate = 5%
* AMAT = 1 + 0.05 * 20 = 2ns
  * 2 cycles per instruction
## Performance Summary
* When CPU performance increases
  * Miss penalty becomes more significant
* Decreasing base CPI
  * Greater proportion of time spent on memory stalls
* Increasing clock rate
  * Memory stalls account for more CPU cycles
* Can't neglect cache behavior when evaluating system performance
## Associative Caches
* Fully associative
  * Allow a given block to go in any cache entry
  * Requires all entries to be searched at once
  * Comparator per entry (expensive)
* N-way set associative
  * Each set contains n entries
  * Block number determines which set
    * (Block number) modulo (#sets in cache)
  * Search all entries in a given set at once
  * n comparators (less expensive)
## How Much Associativity
* Increased associativity decreases miss rate
  * But with diminishing returns
* Simulation of a system with 64KB, D-cache, 16-word blocks, SPEC2000
  * 1-way: 10.3%
  * 2-way: 8.6%
  * 4-way: 8.3%
  * 8-way: 8.1%
## Replacement Policy
* Direct mapped: no choice
* Set associative
  * Prefer non-valid entry, if there is one
  * Otherwise, choose among entries in the set
* Least-recently used (LRU)
  * Choose the one unused for the longest time
    * Simple for 2-way, manageable for 4-way, impractical beyond that
* Random gives approx. same performance as LRU for high associativity
## Multilevel Caches
* Primary cache attached to CPU
  * Small, but fast
* Level-2 cache services misses from primary cache
  * Larger, slower, but still faster than main memory
* Main memory services L-2 cache misses
* Some high-end systems include L-3 cache
## Multilevel Cache Example
* Given
  * CPU base CPI = 1, clock rate = 4GHz
  * Miss rate/ instruction = 2%
  * Main memory access time = 100ns
* With just primary cache
  * Miss penalty = 100ns / 0.25 ns = 400 cycles
  * Effective CPI = 1 + 0.02 * 400 = 9
* Now add L-2 cache
  * Access time = 5ns
  * Miss rate = 10%
* Primary miss with L-2 hit
  * Penalty = 5ns / 0.25 ns = 20 cycles
* Primary miss with L-2 miss
  * Extra penalty = 400 cycles
* CPI = 1 + 0.02 * (20 + 0.10 * 400) = 2.2
* Performance ratio = 9 / 2.2 = 4.1
## Multilevel Cache Considerations
* Primary cache
  * Focus on minimal hit time
* L-2 cache
  * Focus on low miss rate to avoid main memory access
  * Hit time has less overall impact
* Results
  * L-1 cache usually smaller than a single cache
  * L-1 block size smaller than L-2 block size
## Interactions with Advanced CPUs
* Out-of-order CPUs can execute instructions during cahce miss
  * Pending store stays in load/store unit
  * Depentend instructions wait in reservation stations
    * Independent instructions continue
* Effect of miss depends on program data flow
  * Much harder to analyze
  * Use system simulation
## Interactions with Software
* Misses depend on memory access patterns
  * Algorithm behavior
  * Compiler optimization for memory access
## Virtual Memory
* Use main memory as a cache for secondary (disk) storage
  * Managed jointly by CPU hardware and the OS
* Programs share main memory
  * Each gets a private virtual address space holding its frequently used code and data
  * Protected from other programs
* CPU and OS translate virtual addresses to physical ddresses
  * VM block is called a page
  * VM translation miss is called a page fault
## Page Fault Penalty
* On page fault, the page must be fetched from disk
  * Takes millions of clock cycles
  * Handled by OS code
* Try to minimize page fault rate
  * Fully associative placement
  * Smart replacement algortihms
## Page Tables
* Stores placement information
  * Array of page table entries, indexed by virtual page number
  * Page table register in CPU points to page table in physical memory
* If page is present in memory
  * PTE stores the physical page number
  * Plus other status bits
* If page is not present
  * PTE can refer to location in swap space on disk
## Replacement and Writes
* To reduce page fault rate, prefer least-recently used (LRU) replacement
  * Reference bit (aka use bit) in PTE set to 1 on access to page
  * Periodically cleared to 0 by OS
  * A page with reference bit = 0 has not been used recently
* Disk writes take millions of cycles
  * Block at once, not individual locations
  * Write through is impractical
  * Use write-back
  * Dirty bit in PTE set when page is written
## Fast Translation Using a TLB
* Address translation would appear to require extra memory references
  * One to access the PTE
  * Then the actual memory address
* But access to page tables has good locality
  * So use a fast cache of PTEs withing the cPU
  * Called a Translation Look-aside Buffer (TLB)
  * Typical: 16-512 PTEs, 0.5-1 cycle for hit, 10-100 cycles for miss, 0.01% - 1% miss rate
  * Misses could be handled by hardware or software
## TLB Misses
* If page is in memory
  * Load the PTE from memory and retry
  * Could be handled in hardware
    * Can get complex for more complicated page table structures
  * Or in software
    * Raise a special exception with optimized handler
* If page is not in memory (page fault)
  * OS handles fetching the page and updating the page table
  * Then restart the faulting instruction
## TLB Miss Handler
* TLB Miss indicates
  * Page present, but PTE not in TLB
  * Page not present
* Must recognize TLB miss before destination register overwritten
  * Raise exception
* Handler copies PTE from memory to TLB
  * Then restarts instruction
  * If page not present, page fault will occur
## TLB and Cache Interaction
* If cache tag uses physical address
  * Need to translate before cache lookup
* Alternative: use virtual address tag
  * Complications due to aliasing
    * Different virtual addresses for shared physical address
## Memory Hierarchy Recap
* Common principles apply at all levels of the memory hierarchy
  * Based on notions of caching
* At each level in the hierarchy
  * Block placement
  * Finding a block
  * Replacement on a miss
  * Write policy
## Block Placement
* Determined by associativity
  * Direct mapped (1-way associative)
    * One choice for placement
  * n-2ay set associative
    * n choices within a set
  * Fully associative
    * Any location
* Higher associativity reduces miss rate, but increases complexity, cost, and access time
## Finding a Block
![img](https://i.imgur.com/wshWNws.png)
