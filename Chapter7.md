# Intro
* Goal: connect multiple computers to get higher performance
  * Multiprocessors
  * Scalability, availibility, power efficiency
* Job-level (process-level) paralellism
  * High throughput for independent jobs
* Parallel processing program
  * Single program run on multiple processors
* Multicore microprocessors
  * Chips with multiple processors (core)
## Parallel Programming
* Parallel software is the problem
* Need to get significant performance improvement
  * Otherwise, just use a faster uniprocessor, since it's easier
* Difficulties
  * Partitioning
  * Coordination
  * Communications overhead
## Amdahl's Law
* Sequential part can limit speedup
* Example: 100 processors, 90x speedup?
  * Tnew = Tparallelizable / 100 + Tsequential
  * Speedup = 1 / ((1 - Fparallelizable) + (Fparallelizable/100)) = 90
  * Solving: Fparallelizable = 0.999
* Need sequential part to be 0.1% of original time
## Scaling Example
* Workload: sum of 10 scalars, and 10x10 matrix sum
  * Speed up from 10 to 100 processors
* Single processor: Time = (10 + 100) * t_add
* 10 processors
  * Time = 10 * t_add + 100/10 * t_add = 20 * t_add
  * Speedup = 100/20 = 5.5 (55% of potential)
* 100 processors
  * Time = 10 * t_add + 100/100 * t_add = 11 * t_add
  * Speedup = 110/11 = 10 (10% of potential)
* Assumes load can be balanced across processors
## Scaling Example (continued)
* What if matrix size is 100x100?
* Single processor: Time = (10 + 10000) * t_add
* 10 processors
  * Time = 10 * t_add + 10000/10 * t_add = 1010 * t_add
  * Speedup = 10010/1010 = 9.9 (99% of potential)
* 100 processors
  * Time = 10 * t_add + 10000/100 * t_add = 110 * t_add
  * Speedup = 10010/110 = 91 (91% of potential)
* Assumes load can be balanced across processors
## Strong vs Weak Scaling
* Strong scaling: problem size fixed, as in previous example
* Weak scaling: problem size is proportional to number of processors
  * 10 processors, 10x10 matrix
    * Time = 20 * t_add
  * 100 processors, 32x32 matrix
    * Time = 10 * t_add + 1000/100 * t_add = 20 * t_add
  * Constant performance in this example
## Shared Memory
* SMP: shared memory multiprocessor
  * Hardware provides single physical address space for all processors
  * Synchronize shared variables using locks
  * Memory access time
    * UMA (uniform) vs NUMA (nonuniform)
## Example: Sum Reduction
* Sum 100,000 numbers on 100 processor UMA
  * Each processor has ID: 0 < Pn < 100
  * Partition 1000 numbers per processor
  * Initial summation on each processor
  ```
  sum[Pn] = 0;
    for (i = 1000*Pn; i < 1000*(Pn+1); i++) 
       sum[Pn] = sum[Pn] + A[i]
  ```
* Now need to add these partial sums
  * Reduction: divide and conquer
  * Half the processors add pairs, then quarters...
  * Need to synchronize between reduction steps
## Message Passing
* Each processor has private physical address space
* Hardware sends/receives messages between processors
## Loosely Coupled Clusters
* Network of independent computers
  * Each has private memory and OS
  * Connected using I/O system
    * Eg. ethernet/switch, internet
* Suitable for applications with independent tasks
  * Web servers, databases, simulations, ...
* High availability, scalable, affordable
* Problems
  * Administration cost (prefer virtual machines)
  * Low interconnect bandwidth
## Sum Reduction (again)
* Sum 100,000 on 100 processors
* First distribute 100 numbers to each
  * Then do partial sums
    ```
    sum = 0;
    for (i = 0; i<1000; i++)
      sum = sum + AN[i]
    ```
* Reduciton
  * Half the processors send, other half receive and add
  * The quarter send, quarter receive and add, ...
## Grid Computing
* Separate Computers interconnected by long-haul networks
  * Eg. internet connections
  * Work unites farmed out, results sent back
* Can make use of idle time on PCs
  * Eg. SETI@home, world community grid
## Multithreading
* Performing multiple threads of execution in parallel
  * Replicate registers, PC, etc
  * Fast switching between threads
* Fine-grain multithreading
  * Switch threads after each cycle
  * Interleave instruction executioin
  * If one thread stalls, others are executed
* Coarse-grain multithreading
  * Only siwtch on long stall (eg. L2 cache miss)
  * Simplifies hardware, but doesn't hide short stalls (eg. data hazards)
## Simultaneous Multithreading
* In multiple-issue dynamically scheduled processor
  * Schedule instructions from multiple threads
  * Instructions from independent threads execute when function units are available
  * Withn threads, dependencies handled by scheduling and register renaming
## Future of Multithreading
* Power considerations -> simplified microarchitectures
  * Simpler forms of multithreading
* Tolerating cache-miss latency
  * Thread switch may be most effective
* Multiple simple cores might share resources more effectively
## Instruction and Data Streams
![img](https://i.imgur.com/xNifSwz.png)
## SIMD
* Operate elementwise on vectors of data
* All processors execute the same instruction at the same time
  * Each with different data address, etc.
* Simplifies synchronization
* Reduced instruction control hardware
* Works best for highly data-parallel applications
## Vector Processors
* Highly pipelined function units
* Stream data from/to vector registers to units
  * Data collected from memory into registers
  * Results stored from registers to memory
* Example: vector extension to MIPS
  * 32 x 64-element registers (64bit elements)
  * Vector instructions
    * lv, sv: load/store vector
    * addv.d: add vectors of double
    * addvs.d: add scalar to each element of vector of double
* Significantly reduces instruction-fetch bandwidth
## Vector vs. Scalar
* Vector architectures and compilers
  * Simplify data-parallel programming
  * Explicit statement of absence of loop-carried dependences
    * Reduced checking in hardware
  * Regular access patterns benefit from interleaved and burst memory
  * Avoid control hazards by avoiding loops
* More general than ad-hoc media extensions (such as MMX, SSE)
  * Better match with compiler tech
