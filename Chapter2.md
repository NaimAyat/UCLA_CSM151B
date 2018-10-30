# Chapter 2
## ISA: Instruction Set Architecture
* The mechanism by which software communicates with hardware
### Key ISA Decisions
* Operations
  * How many? Which ones? Size/length (number of bits to represent)?
* Operands
  * How many? Location? Types? How to specify?
* Instruction format
  * Size? How many formats?
### MIPS ISA
#### Arithmetic Operations
* Add and subtract, three operations: two sources and one destination
  * `add a, b, c   # a gets b+c`
  * All arithmetic operations have this form
  * Example: convert `f = (g + h) - (i + j);` from C to MIPS:
    ```
    add t0, g, h   # temp t0 = g + h
    add t1, i, j   # temp t1 = i + j
    sub f, t0, t1  # f = t0 - t1
    ```
#### Register Operands
* MIPS has a 32x32-bit register file numbered 0 to 31
  * 32-bit data called a "word"
* Assembler names
  * `$t0, $t1, ..., $t9` for temporary values
  * `$s0, $s1, ..., $s9` for saved variables
