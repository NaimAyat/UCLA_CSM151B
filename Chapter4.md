# Chapter 4
## Carry Look Ahead Adder
* Generate and propagate functions
  * `G_i = x_i * y_i`
  * `P_i = x_i + y_1`
* For any `C_(i+1) = G_i + P_i * C_i`
  * Ex: `C_3 = G_2 + P_1 * C_2`
  * Ex: `C_1 = G_0 + P_0 * C_0`
  * Ex: `C_2 = G_1 + P_1 * C_1`
    * But `C_1` is not yet available, so we replace it with its equation `C_2 = G_1 + P_1 * G_0 + P_0 * C_0`
    * Simplified, this means `C_2 = G_1 + P_1 G_0 + P_1 P_0 * C_0`
    * `C_3 = G_2 + G_1 P_2 + G_0 P_1 P_2 + P_0 P_1 P_3 C_0`
    * `C_4 = G3 + G2 P3 + G1 P2 P3 + G0 P1 P2 P3 + P0 P1 P2 P3 C0`
* For any `S_i = (x_i XOR y_i) XOR C_i`
## Datapath
* R-Type Instruction: Assembler turns instruction into 32-bit word
  1. 6 bits for opcode `op`: determines which processor components are utilized for the given instruction. Automatically generated.
  2. 5 bits for first source register `rs`
  3. 5 bits for second source register `rt`
  4. 5 bits for destination register `rd`
  5. 5 bits for shift amount `shamt`. If no shift necessary, this is 00000
  6. 6 bits for `funct`, this defines which function will be performed on the registers
* R-Type add Datapath walk through
  1. Load 32-bit instruction from memory, from the address stored in the program counter (PC)
     * This address is also used to calculate the address of the next instruction
  2. The opcode goes to the control unit, which decodes the bits and chooses which path will be used
     1. RegDest gets a high signal to tell the multiplexer that feeds the register file that 3 registers are being used
     2. ALUOp set to high to signify to the ALU control unit what data needs to be passed
     3. RegWrite set to high to signify that data must be written to a register
     4. Jump and Branch must both be low signal since neither are being used
     5. MemRead, MemToReg, and MemWrite are all set to low since there is no memory manipulation in this instruction
     6. ALUSrc will be set to low to tell the mux which feeds the second argument to the ALU which piece of data to use
  3. `rs` goes into Read Register 1 to be used as the first argument in the ALU
  4. `rt` goes into Read Register w to be used as the second argument in the ALU
  5. Since RegDest got a high signal, `rd` goes to Write Register to indicate which register should have results written to
  6. `fn` goes to the ALU control unit to signify which operation should be performed (ex. add)
  7. Since a low signal was sent to Data Memory, it will not be utilized
  8. ALU result is sent back to Write Data register
  9. In step 1, we saw that the current address was also sent to an adder. This adder adds 4 to give us an address that is 32 bits away, AKA the next instruction. This address is then loaded into the program counter, and the cycle restarts.
  
