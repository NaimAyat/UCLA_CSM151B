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
#### Immediate Operands
* Constant data specified in an instruction, ex. `addi $s3, $s3, 4`
* No subtract immediate instruction, just use negative constant, ex. `addi $s2, $s1, -1`
#### The Constant Zero
* MIPS register 0 (`$zero`) is the constant 0; this cannot be overwritten
* Useful for common operations, like moving between registers
  * `add $t2, $s1, $zero`
### Registers vs. Memory
* Registers are faster to access
* Operating on memory requires loads and stores
* Compliler must use registers for variables as much as possible
### 2's Complement
* To negate, flip all the bits until the last `1`, leave that and all that follows as-is
* For sign extension: replicate the sign bit to the left. Ex: `0101 1111 -> 0000 0000 0101 1111`
## Representing Instructions
* Instructions are encoded in binary machine code
* MIPS instructions encoded as 32-bit instruction words
### MIPS R-format Instructions
* op: operation code (opcode) = 6 bits
* rs: first source register number = 5 bits
* rt: second source register number = 5 bits
* rd: destination register number = 5 bits
* shamt: shift amount = 5 bits
* funct: function code (extends opcode) = 6 bits
### MIPS I-format Instructions (for immediate arithmetic and load/store instructions)
* op: operation code (opcode) = 6 bits
* rs: first source register number = 5 bits
* rt: second source register number = 5 bits
* constant or address = 16 bits
### Logical Operators for Bitwise Manipulation
* Shift left = `sll`
* Shift right = `srl`
* Bitwise AND = `and, andi`
* Bitwise OR = `or, ori`
* Bitwise NOT = `nor`
### Shift Operations
* op: operation code (opcode) = 6 bits
* rs: first source register number = 5 bits
* rt: second source register number = 5 bits
* rd: destination register number = 5 bits
* shamt: shift amount = 5 bits
* funct: function code (extends opcode) = 6 bits
### AND Operations
* Useful to mask bits in a word (select some bits, clear others to 0) `and $t0, $t1, $t2`
### OR Operations
* Useful to include bits in a word `or $t0, $t1, $t2`
### NOT Operations
* Useful to invert bits in a word `nor $t0, $t1, $zero`
## Conditional (Branch) Operations
* `beq rs, rt, L1` means: `if (rs == rt)` branch to an instruction labeled `L1`
* `bne rs, rt, L1` means: `if (rs != rt)` branch to an instruction labeled `L1`
* `j L1` is an unconditional jump to instruction labeled `L1`
##### Example
* `f ... g` in `$s0 ... $s4`
```
if (i==j) f=g+h;
else f=g-h;
```
```
      bne $s3, $s4, Else
      add $s0, $s1, $s2
      j Exit
Else: sub $s0, $s1, $s2
```
```
while (save[i] == k) i += 1;
```
```
Loop: sll $t1, $s3, 2
      add $t1, $t`, $s6
      lw $t0, 0($t1)
      bne $t0, $s5, Exit
      addi $s3, $s3, 1
      j Loop
```
### Basic Block
* Sequence of instructions without embedded branches (except at the end) and no branch targets (except at the beginning)
### More Conditional Operations
* `slt` set result to 1 if a condition is true, otherwise set to 0
  * `slt rd, rs, rt` `if (rs<rt) rd = 1; else rd = 0;`
* `slti` set result to 1 if a condition is true, otherwise set to 0
  * `slt rd, rs, constant` `if (rs<constant) rd = 1; else rd = 0;`
* Use in combination with `beq`, `bne`
  ```
  slt $t0, $s1, $s2 # if ($s1 < $s2)
  bne $t0, $zero, L # branch to L
  ```
### Signed vs. Unsigned
* `slt`, `slti` for signed comparison
* `sltu`, `sltui` for unsigned comparison
