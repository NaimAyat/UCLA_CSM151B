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
## Load-Use Data Hazard
* Can't always avoid stalls by forwarding, might need to stall as well
### Load-Use Data Hazard Detection
* Check when using instruction is decoded in ID stage
* ALU operand register numbers in ID stage are given by
  * IF/ID.REgisterRs, IF/ID.RegisterRt
* Load-use hazard when
  * ID/EX.MemRead and ((ID/EX.RegisterRt = IF/ID.RegisterRs) or (ID/EX.RegisterRT = IF/ID.RegisterRt))
* If detected, stall and insert bubble
## How to Stall Pipeline
* Force control values in ID/EX reg to 0
  * EX, MEM, and WB do nop
* Prevent update of PC and IF/ID reg
# Data Hazards Recap
* Stalls reduce performance but are required to get correct results
* Compiler can arrange code to avoid hazards and stalls (requires knowledge of pipeline structure)
# Control Hazards
* Branch determines flow of control
  * Fetching next instruction depends on branch outcome
  * Pipeline can't always fetch correct instruction, still working on ID stage of branch
* In MIPS pipeline
  * Need to compare registers and compute target early in pipeline
  * Add hardware to do it in ID stage
## Dealing with Branch Hazards
* Hardware solutions
  * Stall until you know which direction branch goes
  * Guess which direction, start executing chosen path
    * Static branch predition: base guess on instruciton type
    * Dynamic branch predicition: base guess on execution history
  * Reduce the branch delay
### Stall on Branch
* Wait until branch outcome determined before fetching next instruciton
### Branch Prediction
* Longer pipelines can't readily determine branch outcome early
  * Stall penalty becomes unacceptable
* Predict outcome of branch
  * Only stall if prediction is wrong
* In MIPS pipeline
  * Can predict branches not taken
  * Fetch instruction after branch, with no delay
* Static branch prediction
  * Based on typical branch behavior
  * Ex: loop and if-statement branches
    * Predict backward branches taken, predict forward branches not taken
* Dynamic branch prediction
  * Hardware measures actual branch behavior
    * Record history of each branch
  * Assume future behavior will continue the trend
    * When wrong, stall while re-fetching, and update history
### Reducing Branch Delay
* Move hardware to determine outcome to ID stage
  * Target address adder
  * Register comparator
## Data Hazards for Branches
* If a comparison register is a destination of 2nd or 3rd preceding ALU instruction
  * Can resolve using forwarding
* If a comparison register is a destination of preceding ALU instruciton or 2nd preceding load instruction 
  * Need 1 stall cycle
* If comparison register is destination of immediately preceding load instruction
  * Need 2 stall cycles
## Dynamic Branch Predicition
* In deeper and superscalar pipelines, branch penalty is more significant
* Use dynamic prediciton
  * Branch prediction buffer (aka branch history table)
  * Indexed by recent branch instruction addresses
  * Stores outcome taken/not taken
  * To execute branch:
    * Check table, expect same outcome
    * Start fetching from fall-through or target
    * If wrong, flush pipeline and flip prediciton
### 1-bit Predictor: Shortcoming
* Inner loop branches mispredicted twice
  * Mispredict as taken on last iteration of inner loop
  * Then mispredict as not taken on first iteration of inner loop next time around
### 2-bit Predictor
* Only change prediction on two successive mispredictions
### Calculating the Branch Target
* Even with predictor, still need to calculate target address
  * 1-cycle penalty for a taken branch
* Branch target buffer
  * Cache of target addresses
  * Indexed by PC when instruction fetched
    * If hit and instruction is branch predicted taken, can fetch target immediately
## Exceptions and Interrupts
* "Unexpected" events requiring change in flow of control
* Exception 
  * Arises within CPU (undefined opcode, overflow, syscall...)
* Interrupt
  * From an external I/O controller
* Dealing with them without sacrificing performance is hard
### Handling Exceptions
* In MIPS, exceptions managed by a System Control Coprocessor
* Save PC of offending or interrupted instruciton
* Save indication of the problem
* Jump to handler at 8000 00180
### An Alternate Mechanism
* Vectored interrupts
  * Handler address determined by the cause
* Instructuons either deal with interrupt or jump to real handler
### Handler Actions
* Read cause and transfer to relevant handler
* Determine action required
* If restartable
  * Take corrective action
  * Use EPC to return to program
* Otherwise
  * Terminate program
  * Report error using EPC
### Exceptions in a Pipeline
* Another form of control hazard
* Consider overflow on add in EX stage `add $1, $2, $1`
  * Prevent $1 from being clobbered
  * Complete prev instructions
  * Flush add and subsequent instructions
  * Set Cause and EPC reg values
  * Transfer control to handler
* Similar to mispredicted branch
  * Use much of same hardware
### Exception Properties
* Restatable exceptions
  * Pipeline can flush the instruction
  * Handler executes, then returns to the instruction
    * Refetched and executed from scratch
* PC saved in EPC register
  * Identifies causing instruction
  * Actually PC+4 is saved
    * Handler must adjust
### Multiple Exceptions
* Pipelining overlaps multiple instructions
  * Could have multiple exceptions at once
* Simple approach: deal with exception from earliest instruction
  * Flush subsequent instructions
  * Precise exceptions
* In complex pipelines
  * Multiple instructions issued per cycle
  * Out-of-order completion
### Imprecise Exceptions
* Just stop pipeline and save state
  * Including exception cause(s)
* Let the handler work out
  * Which instruction(s) had exceptions
  * Which to complete or flush
    * May require manual completion
* Simplifies hardware, but more complex handler software
* Not feasible for complex multi-issue out-of-order pipelines
# Advanced Pipelining
## Instruction-Level Parallelism (ILP)
* Pipelining: executing multiple instructions in parallel
* To increase ILP
  * Deeper pipeline
    * Less work per stage -> shorter clock cycle
  * Multiple issue
    * Replicate pipeline stages -> multiple pipelines
    * Start multiple instructions per clock cycle
    * CPI < 1, so use instructions per cycle (IPC)
    * Eg. 4GHz 4-way multiple-issue
      * 16 BIPS, peak CPI = .25, peak IPC = 4
    * But dependencies reduce this in practice
## Multiple Issue
* Static multiple issue
  * Compile groups instructions to be issued together
  * Packages them into "issue slots"
  * Compiler detects and avoids hazards
* Dynamic multiple issue
  * CPU examines instruction stream and chooses instructions to issue each cycle
  * Compiler can help by reordering instructions
  * CPU resolves hazards using advanced techniques at runtime
## Speculation
* "Guess" what do do with an instruction
  * Start operation as soon as possible
  * Check whether guess was right
    * If so, complete the op
    * If not, rollback and do right thing
* Common to static and dynamic multiple issue
### Compiler/Hardware Speculation
* Compiler can reorder instructions
  * eg. move load before branch
  * Can include "fix-up" instructions to recover from incorrect guesses
* Hardware can look ahead for instructions to execute
  * Buffer results until it determines they are actually needed
  * Flush buffers on incorrect speculation
### Static Multiple ISsue
* Compiler groups instructions into "issue packets" 
  * Group of instructions that can be issues on a single cycle
  * Determined by pipeline resources required
* Think of an issue packet as a very long instruction
  * Specifies multiple concurrent operations
  * Very long instruction word (VLIW)
### Scheduling Static Multiple Issue
* Compiler must remove some/all hazards
  * Reorder instructions into issue packets
  * No dependencies with a packet
  * Possible some dependencies between packets
    * Varies between ISAs, compiler must know
  * Pad with nop if necessary
### MIPS with Static Dual ISsue
* Two-issue packets
  * One ALU/branch instruction
  * One load/store instruction
  * 64-bit aligned
    * ALU/branch, then load/store
    * Pad an unused instruction with nop
### Hazards in the Dual-Issue MIPS
* More instructions executing in parallel
* EX data hazard
  * Forwarding avoided stalls with single-issue
  * Now can't use ALU result in load/store in same packet
* Load-use hazard
  * Still one cycle use latency, but now two instructions
* More aggressive scheduling required
### Dylamic Multiple Issue
* "Superscalar" processors
* CPU decides whether to issue 0, 1, 2, ... each cycles
  * Avoiding structural and data hazards
* Avoids the need for compiler scheduling
### Dynamic Pipeline Scheduling
* Allow CPU to execute instructions out of order to avoid stalls
  * But commit result to registers in order
### Register Renaming
* Reservation stations and reorder buffer effectively provide register renaming
* On instruction issue to reservation station
  * If operand is available in register file or recorder buffer
    * Copied to reservation station
    * No longer required in the register, can be overwritten
  * If operand is not yet available
    * It will be provided to the reservation station by a function unit
    * Register update may not be required
### Why do dynamic scheduling?
* Why not just let the compiler schedule code?
* Not all stalls are predictable (eg. cache misses)
* Can't always schedule around branches
  * Branch outcome is dynamically determined
* Different implementations of an ISA have different latencies and hazards
### Does Multiple Issue Work?
* Yes, but not as much as we'd like
* Programs have real dependencies that limit ILP
* Some dependencies are hard to eliminate (eg. pointer aliasing)
* Some parallelism is hard to expose
  * Limited window size during instruction issue
* Memory delays and limited bandwidth
  * Hard to keep pipelines full
* Speculation can help if done well
