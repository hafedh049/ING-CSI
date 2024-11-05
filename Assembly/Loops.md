### Using the JMP Instruction for Loops

The `JMP` instruction can be used for implementing loops. For example, the following code snippet can be used for executing the loop body 10 times:

```c
MOV CL, 10
L1:
    <LOOP-BODY>
    DEC CL
    JNZ L1
```

The processor instruction set, however, includes a group of loop instructions for implementing iteration. The basic `LOOP` instruction has the following syntax:

```c
LOOP label
```

Where `label` is the target label that identifies the target instruction, similar to the jump instructions. The `LOOP` instruction assumes that the `ECX` register contains the loop count. When the loop instruction is executed, the `ECX` register is decremented, and control jumps to the target label until the `ECX` register value (the counter) reaches zero.

The above code snippet could be written as:

```c
mov CX, 10
l1:
    <loop body>
    loop l1
```
