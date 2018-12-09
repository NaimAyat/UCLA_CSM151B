* LW followed by ADD: need one stall in between
* Loop unroll:
  1. Copy loop contents
  2. Increment each $t by 2, do not add offset
  3. Keep each $s the same, but add an offset of 4 when available
  4. Add offset of 4 to ADDI
  
  
