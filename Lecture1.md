# Lecture 1: October 1, 2018
* Applications, compiler, OS sits on ISA (instruction set architecture) layer
* Instruction set architecture
  * Instructions: add, multiply accumulator
  * Registers
* Micro architecture sits under ISA
  * Area: silicon real estate increasing over time
  * Power / heat
  * Wires: as feature size decreases, wires must be made thinner and taller; capacitance becomes an issue
  * Performance
  * Security
* Physicial design under micro architecture
* Chip
  * Memory
  * Functional units
* Feature size: distance of components from one another
  * Lower is faster; bottleneck is heat, solution is heatsink
## Quarter Overview
* Performance
* ISA
* ALU - arithmetic logic unit
* Datapath + control
* Pipelining
* Caches
## Components of a Computer
* Same across all types of computers - desktop, server, embedded
* Input / output includes
  * UI devices: display, keyboard, mouse
  * Storage devices: HDD, SSD, CD/DVD, flash
  * Network adapters: for communicating with other computers
## Computer Abstractions and Technology
* Spectrum from CPU to ASIC
  * CPU is more flexible; ASIC is designed to particular task
### Understanding Performance
* Algorithm
  * Determines number of operations executed
* Programming language, compiler, architecture
  * Determine number of machine instructions executed per operation
* Processor and memory system
  * Determine how fast instructions are executed
* I/O system (including OS)
  * Determines how fast I/O operations are executed
