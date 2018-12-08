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
  * Cycle time will not be impacted, but number of instructions increases
  * Solution: code scheduling to avoid stalls
    * Reorder code to avoid use of load result in next instruction
* In hardware: insert bubbles (stall the pipeline) and forward data
  ![img](https://i.imgur.com/lN11h2W.png)
## Pipeline Stalls
* To ensure pipeline execution in light of register dependencies, we must:
  * Detect the hazard
  * Stall the pipeline
    * Prevent the IF and ID stage from making progress
      * ID Stage because we can't go on until the dependent instruction completes
      * IF stage because we do not want to lose instructions
## Forwarding (aka Bypassing)
* Use result when it is computed
  * Don't wait for it to be stored in a register
  * Requires extra connections in the datapath
### Detecting the Need to Forward
* Pass register numbers along the pipeline
  * e.g. ID/EX register numbers for Rs sitting in ID/EX pipeline register
* ALU operand register numbers in EX stage are given by 
  * ID/EX.REgisterRs, ID/EX.RegisterRT
* Data hazards when
  * EX/MEM.RegisterRd = ID/EX.RegisterRs
  * EX/MEM.RegisterRd = ID/EX.RegisterRt
  * MEM/WB.RegisterRd = ID/EX.RegisterRs
  * MEM/WB.RegisterRd = ID/EX.RegisterRt
* But only if forwarding instruction will write to a register
* And only if Rd for that instruction is not $zero
### Forwarding Conditions
  ![img](https://i.imgur.com/KaCSlEu.jpg)
  ![img](https://i.imgur.com/DYl7q6C.png)
