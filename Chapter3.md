# Chapter 3
## MIPS Multiplication
* Two 32-bit registers for product
  * `HI`: most significant 32 bits
  * `LO`: least significant 32 bits
* Instructions
  * `mult rs, rt` and `multu rs, rt`
    * Signed/unsigned 64-bit product in `HI`/`LO`
  * `mfhi rd` and `mflo rd`
    * Move from `HI`/`LO` to `rd`
  * `mul rd, rs, rt`
    * Least significant 32 bits of product to `rd`
### [Booth's Algorithm](https://www.youtube.com/watch?v=1ubyXuXxIWU)
1. Create table headers: Iteration, Step, Product Register, Last Bit Shifted Out, Description
2. Iteration = 0, Step = 0, Product Register = double multiplier size - extend left, Last = 0, Description = Initialize
3. Iteration = 1, Step = 1.(last_bit_of_product+Last), Product Register = same, Last = Same, Description = ?
   1. If last_bit_of_product+Last is 00 or 11, do nothing
   2. If last_bit_of_product+Last is 10, subtract by adding inverse of multiplicand to left side of product
   3. If last_bit_of_product+Last is 01, add by adding the multiplicand to the left side of the product
4. Iteration = 1, Step = 2, shift (extend preserving sign bit) and create new iteration
5. Create new iteration

[!1](img/1.png)

[!2](img/2.png)
