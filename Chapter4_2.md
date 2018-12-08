# Pipelining
* Analogy: overlapping execution. Parallelism improves performance
## MIPS Pipeline
* Five stages, one step per stage
  1. IF: instruction fetch from memory
  2. ID: instruction decode and register read
  3. EX: execute operation or calculate address
  4. MEM: access memory operand
  5. WB: write result back to register
## Pipeline Performance
### Pipeline Speedup
* If all stages take the same time:
  * `Time between instructions (pipelined) = time between instructions (nonpipelined) / number of stages`
## Pipeline Principles
* All instructions that share a pipeline must have the same stages in the same order
  * Therefore, add does nothing during MEM stage; sw does nothing during WB stage
* All intermediate values must be latched each cycle
* There is no functional block reuse
## Pipeline Registers
* Need registers between stages to hold information produced in previous cycle
# Pipeline Hazards
* Structure hazards: a required resource is busy
* Data hazard: need to wait for previous instruction to complete its data read/write
* Control hazard: decidiing on control action depends on previous instruction
### Structure Hazards
* Conflict for use of a resource
* In MIPS pipeline with a single memory
  * Load/store requires data access
  * Instruction fetch would have to stall for that cycle
    * Would cause a pipeline "bubble"
* Hence pipelined datapaths require separate instruciton/data memories
### Data Hazards
* An instruction depends on completion of data access by a previous instruction
```
add $s0, $t0, $t1
sub $t2, $s0, $t3
```
# Dealing with Data Hazards
* In software: insert independent operations (no-ops)
  ![img](https://i.imgur.com/LA6AP48.png)
* In hardware: insert bubbles (stall the pipeline) and forward data
