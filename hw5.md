# 4.13.1
```
add r5,r2,r1
lw  r3,4(r5)
lw  r2,0(r2)
or  r3,r5,r3
sw  r3,0(r5)
```
If there is no forwarding or hazard detection, insert nops to ensure correct execution.
#### Answer
* Add WB needs to align with first lw ID
* First lw WB needs to align with or ID
* or WB needs to align with sw ID
```
add  IF  ID  EX  MEM  WB
nop      IF  ID  EX   MEM  WB
nop          IF  ID   EX   MEM  WB
lw               IF   ID   EX   MEM  WB
lw                    IF   ID   EX   MEM  WB
nop                        IF   ID   EX   MEM  WB
or                              IF   ID   EX   MEM  WB
nop                                  IF   ID   EX   MEM  WB
nop                                       IF   ID   EX   MEM  WB
sw                                             IF   ID   EX   MEM  WB
```
* Hence, the following code takes 14 cycles to complete.

```
        ADD r5, r2, r1
        NOP
        NOP
        LW r3, 4(r5)
        LW r2, 0(r2)
        NOP
        OR r3, r5, r3
        NOP
        NOP
        SW r3, 0(r5)
```
# 14.16
This exercise examines the accuracy of various branch predictors for the following repeating pattern (e.g., in a loop) of branch outcomes: T, NT, T, T, NT
* What is the accuracy of always-taken and always-not-taken predictors for this sequence of branch outcomes?
  * Always taken: hit, miss, hit, hit, miss = 3/5 = 60%
  * Always not taken: miss, hit, miss, miss, hit = 2/5 = 40%
* Using FSM: 
  * NT, NT, NT, NT. Accuracy 1/4 = 25%
* Repeated forever: 
