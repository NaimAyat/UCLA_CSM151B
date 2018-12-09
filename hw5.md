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
##### No Forwarding
```   
add  IF  ID  EX  MEM  WB
nop      IF  ID  EX   MEM WB
nop          IF  ID   EX  MEM  WB
lw               IF   ID  EX   MEM WB
lw                    IF  ID   EX  MEM  WB
nop                       IF   ID  EX   MEM  WB
or                             IF  ID   EX   MEM  WB
nop                                IF   ID   EX   MEM  WB
nop                                     IF   ID   EX   MEM  WB
sw                                           IF   ID   EX   MEM  WB
```
* Hence, it takes 14 cycles
##### Full Forwarding, Predict-Taken, Branches Resolved in EX
```
         lw r2,0(r1)
 label1: beq r2,r0,label2 # not taken once, then taken
         lw r3,0(r2)
         beq r3,r0,label1 # taken
         add r1,r3,r1
 label2: sw r1,0(r2)
```
```
lw  IF   ID   EX   MEM  WB
beq           IF   ID   EX   MEM  WB
lw                                IF   ID   EX   MEM  WB
beq                                         IF   ID   EX   MEM  WB
beq                                              IF   ID   EX   MEM  WB
sw                                                    IF   ID   EX   MEM  WB
```


# 14.16
This exercise examines the accuracy of various branch predictors for the following repeating pattern (e.g., in a loop) of branch outcomes: T, NT, T, T, NT
* What is the accuracy of always-taken and always-not-taken predictors for this sequence of branch outcomes?
  * Always taken: hit, miss, hit, hit, miss = 3/5 = 60%
  * Always not taken: miss, hit, miss, miss, hit = 2/5 = 40%
* Using FSM: 
![img](https://i.imgur.com/k6TCoyG.png)
  * NT, NT, NT, NT. Accuracy 1/4 = 25%
* Repeated forever: 
