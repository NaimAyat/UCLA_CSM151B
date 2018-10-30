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
  * Each word represented by a 5-bit address
* Assembler names
  * `$t0, $t1, ..., $t9` for temporary values
  * `$s0, $s1, ..., $s9` for saved variables
##### Example
* Example: convert `f = (g + h) - (i + j);` from C to compiled MIPS:
  * Assume `f ... j` are in `$s0, ..., $s4`
    ```
    add $t0, $s1, $s2
    add $t1, $s3, $s4
    sub $s0, $t0, $t1
    ```
#### Memory Operands
* Memory is byte addressed - each address identifies an 8-bit byte
* Words are aligned in memory - address must be a multiple of 4
* To apply arithmetic operations:
  * Load values from memory into registers
  * Store result from register to memory
##### Example
* Example: convert `g = h + A[8]` from C to compiled MIPS:
  * Assume `g` in `$s1`, `h` in `$s2`, base address of A in `$s3`. A is an `int` array and each element needs 4 bits
  ```
  lw $t0, 32($s3) # load word offset by 32 bits (4*8)
  add $s1, $s2, $t0
  ```
* Example: convert `A[12] = h + A[8]` from C to compiled MIPS:
  ```
  lw $t0, 32($s2)
  add $t0, $s1, $t0
  sw $t0, 48($s3) # store word
  ```
  
