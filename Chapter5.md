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
