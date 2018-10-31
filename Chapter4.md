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
## R-Type `add` Datapath Walkthrough
1. Load 32-bit instruction from memory, from the address stored in the program counter (PC)
   * This address is also used to calculate the address of the next instruction
2. The opcode goes to the Control unit, which decodes the bits and chooses which path will be used
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
## J-Type `j` Datapath Walkthrough
1. Load 32-bit instruction from memory, from the address stored in the program counter (PC)
   * This address is also used to calculate the address of the next instruction
2. The opcode goes to the control unit
   1. Jump is given a high signal, which toggles the MUX which is to be utilized by the jump instruction
   2. All other Control signals get a low signal, since we don't need them
3. The remaining 26 bits of the instruction get converted to an address, and the 4 MSBs are reused from the previous instruction. The two LSBs get 00 to form a full 32 bit address.
4. The MUX that got a high signal from the control unit allows the resulting address to be loaded back into the program counter.
## I-Type `beq` Datapath Walkthrough
1. Load 32-bit instruction from memory, from the address stored in the program counter (PC)
   * This address is also used to calculate the address of the next instruction
2. The opcode goes to the Control unit
   1. `Branch` gets high signal
   2. `ALUOp` gets high signal because there needs to be a conditional check
   3. All other signals get low
3. `rs` and `rt` are sent as input to the ALU
   * If the condition does not pass, then the current address is simply incremented by 4 and we continue to the next instruction
4. If the condition passes: the ALU sends a high signal to the AND gate to activate the MUX signifying that the condition passed
5. The 16-bit `immediate` field is sign extended to 32 bits and shifted left twice to convert distance to bytes
6. This number is then added to the incremented address of the current instruction, and the final result is sent back to the program counter
## I-Type `sw` Datapath Walkthrough
1. Load 32-bit instruction from memory, from the address stored in the program counter (PC)
   * This address is also used to calculate the address of the next instruction
2. The opcode goes to the Control unit
   1. ALUOp gets high signal, since we need to add offset 
   2. MemWrite gets high signal since we are doing store word
   3. ALUSrc gets high to activate MUX into ALU
   4. All else gets low
3. `rs` (the address in memory where we want to store data) gets read and passed as input to ALU
4. `rt` contains data that needs to be written to memory, so it goes to Write Data in Data Memory
5. The 16-bit `immediate` immediate field (the offset) is extended to 32 bits
6. Since the control unit activated the ALUSrc MUX, the offset is fed as input to the ALU
7. The ALU adds the destination address with the offset
8. The result goes to Address in Data Memory, and Data Memory stores Write Data to the Address
9. The current address is incremented by 4 and the next instruction gets loaded by program counter
## Misc
### ALUOp
* 00 = addition
* 01 = subtraction
* 10 = TBD by funct field of R-type
