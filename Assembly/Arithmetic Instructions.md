Here is the revised text:

---

**The INC Instruction**

The `INC` instruction increments an operand by **one**. This operand can be either in a register or in memory.

**Syntax**:
```c
INC destination
```
The operand `destination` can be an 8-bit, 16-bit, or 32-bit value.

**Examples**:
```c
INC EBX     ; Increments 32-bit register
INC AX     ; Increments 16-bit register
INC DL      ; Increments 8-bit register
INC [count] ; Increments the count variable
```

---

**The DEC Instruction**

The `DEC` instruction decrements an operand by one. It can be used with a single operand, which may reside in a register or in memory.

**Syntax**:
```c
DEC destination
```
The operand `destination` can also be an 8-bit, 16-bit, or 32-bit value.

**Examples**:
```c
segment .data
   count dw  0
   value db  15

segment .text
   inc [count]
   dec [value]

   mov ebx, count
   inc word [ebx]

   mov esi, value
   dec byte [esi]
```

---

**The ADD and SUB Instructions**

The `ADD` and `SUB` instructions perform simple addition or subtraction operations with binary data in byte, word, or doubleword sizes, supporting 8-bit, 16-bit, or 32-bit operands.

**Syntax**:
```c
ADD/SUB destination, source
```
`ADD` and `SUB` operations can be performed between:

1. Register to register
2. Memory to register
3. Register to memory
4. Register to constant data
5. Memory to constant data

**Note**: Memory-to-memory operations are not possible with these instructions. Additionally, these operations modify the overflow and carry flags.

---

**The MUL/IMUL Instruction**

The `MUL` instruction is used for unsigned multiplication, while `IMUL` handles signed multiplication. Both affect the carry and overflow flags.

**Syntax**:
```c
MUL/IMUL multiplier
```

In both instructions, the multiplicand is in an accumulator register, with the result stored in different registers depending on operand sizes:

1. For byte multiplication, the result is in `AX`.
2. For word multiplication, the result is in `DX:AX`.
3. For doubleword multiplication, the result is in `EDX:EAX`.

**Examples**:
```c
MOV AL, 10
MOV DL, 25
MUL DL

MOV DL, 0FFH ; DL = -1
MOV AL, 0BEH ; AL = -66
IMUL DL
```

---

**The DIV/IDIV Instructions**

`DIV` is used for unsigned division and `IDIV` for signed division. Division generates a quotient and a remainder, with the dividend typically residing in the `AX` or `DX:AX` registers.

**Syntax**:
```c
DIV/IDIV divisor
```

The result and remainder are stored based on operand sizes:

1. For byte divisors, the quotient is in `AL` and the remainder in `AH`.
2. For word divisors, the quotient is in `AX` and the remainder in `DX`.
3. For doubleword divisors, the quotient is in `EAX` and the remainder in `EDX`.