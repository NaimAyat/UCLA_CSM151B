* LW followed by ADD: need one stall in between
### Loop unroll:
1. Copy loop contents
2. Increment each $t by 2, do not add offset
3. Keep each $s the same, but add an offset of 4 when available
4. Add offset of 4 to ADDI
### Cache Hit/Miss
* Go through each row in table
    * If index has never been used before:
      * Miss
    * If index has been used before:
      * Does the tag also match the tag for the last time this index was used?
        * If yes, hit
        * If no, miss and replace new tag

