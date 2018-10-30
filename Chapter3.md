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
## [Booth's Algorithm](https://www.youtube.com/watch?v=1ubyXuXxIWU)
