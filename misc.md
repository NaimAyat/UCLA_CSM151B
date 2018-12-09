* LW followed by ADD: need one stall in between
* Loop unroll:
  1. Copy loop contents
  2. Increment each $t by 2, do not add offset
  3. Keep each $s the same, but add an offset of 4 when available
  4. Add offset of 4 to ADDI
  
  
```
addi $s0, $s0, 4    lw $t0, 0($s0)
nop                 nop
nop                 lw $t1, 0($t0)
bne $s0, $s2, Loop
add $t1, $s1, $t1   
nop                 sw $t1, 0($t0)
```
```
lw $t0, 0($s0)
lw $t1, 0($t0)
add $t1, $s1, $t1
sw $t1, 0($t0)

lw $t2, 4($s0)
lw $t3, 0($t2)
add $t3, $s1, $t3
sw $t3, 0($t2)

addi $s0, $s0, 8
bne $s0, $s2, Loop
```

```
addi $s0, $s0, 8   lw $t0, 0($s0)
                   lw $t2, 4($s0)
                   lw $t1, 0($t0)
                   lw $t3, 0($t2)
add $t1, $s1, $t1
bne $s0, $s2, Loop sw $t1, 0($t0)
add $t3, $s1, $t3 
                   sw $t3, 0($t2)
```
