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
