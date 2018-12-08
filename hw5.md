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
```
    | C1 | C2 | C3 | C4  | C5  | C6 | C7  | C8  | C9  | C10 | C11 | C12 | C13 | C14 |
    |----+----+----+-----+-----+----+-----+-----+-----+-----+-----+-----+-----+-----|
    | IF | ID | EX | MEM | WB  |    |     |     |     |     |     |     |     |     |
    |    | IF | ID | NOP | NOP | EX | MEM | WB  |     |     |     |     |     |     |
    |    |    | IF | NOP | NOP | ID | EX  | MEM | WB  |     |     |     |     |     |
    |    |    |    |     |     | IF | ID  | NOP | EX  | MEM | WB  |     |     |     |
    |    |    |    |     |     |    | IF  | NOP | ID  | NOP | NOP | EX  | MEM | WB  |
```
*     Hence, the following code takes 14 cycles to complete.
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
