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
