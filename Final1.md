# 1 
* Problem: too many capacity misses in the data cache 
  * Types of cache misses
    * Compulsory (cold): The first reference to a block of memory, starting with an empty cache
    * Capacity: The cache is not big enough to hold every block you want to use
    * Conflict: Two blocks are mapped to the same location and there is not enough room to hold both.
#2
```
// Instructions:
lw $t0, 512($t0)
lw $t1, 64($t0)
bne $s0, $t1, Loop
/* false start sw */
lw $t0, 512($t0)
lw $t1, 64($t0)
bne $s0, $t1,
sw $s1, 128($t0)
```
```
lw   IF  ID  EX  MEM  WB
lw       IF           ID  EX  MEM  WB
bne                   IF           ID  EX  MEM  WB
sw                                 IF  BAD BAD
lw                                              IF  ID  EX  MEM  WB
lw                                                  IF           ID  EX  MEM  WB
bne                                                              IF           ID  EX  MEM  WB
sw                                                                            IF  ID  EX  MEM  WB
```
* Arithmetic operations compute results in EX stage
* Load instructions produce results at the end of MEM stage; we need NOP of we want lw followed by add, for ex
``` 
 LW  uses IF, ID, EX, MEM, WB
 add uses IF, ID, EX, ---, WB
 SW  uses IF, ID, EX, MEM, --
 R   uses IF, ID, EX, ---, WB
 beq uses IF, ID, EX, ---, --
```
```
lw   IF  ID  EX  MEM  WB
lw       IF      ID   EX  MEM  WB
bne              IF   ID       EX   MEM  WB
sw                        IF   BAD  BAD   
lw                                       IF  ID  EX  MEM WB
lw                                           IF      ID  EX  MEM  WB
bne                                                  IF      ID   EX  MEM  WB
sw                                                           IF   ID  EX   MEM  WB
```
#3
* Since we have 64-byte blocks and 2^6 = 64, the offset size is 6 bits.
* Since 1KB is 2^10 bytes, we do 2^10/2^6 to yield 2^4. Hence, the index is 4 bits.
* Miss (Compulsory)
* Miss & replace (Compulsory)
* Miss (Compulsory)
* Miss & replace (Conflict)
* Hit
* Miss & replace (Conflict)
* Hit
* Miss & replace (Conflict)

#4
* Physical memory / page size = 1GB / 32KB = 2^30 / 2^15 = 2^15 pages
* page table size = #entry * the size of an entry = 2^17 x 2B = 2^18 Bytes
  * #entry = 2^17 (size of VPN)
  * the size of an entry = the size of PPN + 1 (the extra bit)= 30-15+1 = 15+1 (bit) = 2 Bytes
* #entry in TLB / total #page table = 64 * 8 / 2^17 = 2^9 / 2^17 = 1 / 2^8 = 1 / 256
* TLB has 64 sets, so 6 bit needed for the index. The 15 right most bits are used for page offset. So, the six bits are: bit 20 ~ bit 15

#7
