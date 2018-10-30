# Chapter 1
## Response Time and Throughput
* Response time = how long it takes to do a task
* Throughput = work done per unit time
### Relative Performance
* `Performance = 1 / Execution Time`
* "X is n times faster than Y" means:
  * `Performance_X / Performance_Y = ExecutionTime_X / ExecutionTime_Y = n`
### Measuring Execution Time
* Elapsed time = total response time, determines system performance
* CPU time - time spent processing on a given time
### CPU Clocking
* Clock period = duration of a clock cycle (in seconds)
* Clock rate = cycles per second (in Hertz)
#### CPU Time
* `CPU_Time = CPU_Clock_Cycles * Clock_Cycle_Time = CPU_Clock_Cycles / Clock_Rate`
  * Improved by:
    * Reducing number of clock cycles
    * Increasing clock rate
    * There is usually a trade off between clock rate and cucle count
