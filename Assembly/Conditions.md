Conditional execution in c language is achieved using both unconditional and conditional jump instructions, allowing programs to change their flow based on specific conditions.

### Conditional Execution Types
1. **Unconditional Jump**: 
   - Performed by the `JMP` instruction.
   - It transfers control directly to a specified label, regardless of conditions.
   - Syntax: `JMP label`
   
   Example:
   ```c
   MOV AX, 00    ; Initializing AX to 0
   MOV BX, 00    ; Initializing BX to 0
   MOV CX, 01    ; Initializing CX to 1
   L20:
   ADD AX, 01    ; Increment AX
   ADD BX, AX    ; Add AX to BX
   SHL CX, 1     ; Shift left CX (doubles the CX value)
   JMP L20       ; Repeats the statements
   ```

2. **Conditional Jump**: 
   - Performed by a set of conditional instructions (`J<condition>`).
   - Transfers control based on specific flag conditions, modifying the normal flow of execution.
   - These instructions are used for both signed and unsigned data.

### CMP Instruction
- **CMP** compares two operands, setting flags without changing either operand.
- Syntax: `CMP destination, source`
  
  Example:
  ```c
  CMP DX, 00      ; Compare the DX value with zero
  JE L7           ; Jump to label L7 if the zero flag is set (DX is zero)
  ```

### Conditional Jump Instructions
- **Signed Data (Arithmetic Operations)**:

| Instruction | Description                         | Flags Tested |
| ----------- | ----------------------------------- | ------------ |
| JE/JZ       | Jump Equal or Jump Zero             | ZF           |
| JNE/JNZ     | Jump Not Equal or Jump Not Zero     | ZF           |
| JG/JNLE     | Jump Greater or Jump Not Less/Equal | OF, SF, ZF   |
| JGE/JNL     | Jump Greater/Equal or Jump Not Less | OF, SF       |
| JL/JNGE     | Jump Less or Jump Not Greater/Equal | OF, SF       |
| JLE/JNG     | Jump Less/Equal or Jump Not Greater | OF, SF, ZF   |

- **Unsigned Data (Logical Operations)**:

| Instruction | Description                        | Flags Tested |
| ----------- | ---------------------------------- | ------------ |
| JE/JZ       | Jump Equal or Jump Zero            | ZF           |
| JNE/JNZ     | Jump Not Equal or Jump Not Zero    | ZF           |
| JA/JNBE     | Jump Above or Jump Not Below/Equal | CF, ZF       |
| JAE/JNB     | Jump Above/Equal or Jump Not Below | CF           |
| JB/JNAE     | Jump Below or Jump Not Above/Equal | CF           |
| JBE/JNA     | Jump Below/Equal or Jump Not Above | AF, CF       |

- **Special Conditional Jump Instructions**:

| Instruction | Description                          | Flags Tested |
|-------------|--------------------------------------|--------------|
| JXCZ        | Jump if CX is Zero                   | none         |
| JC          | Jump If Carry                        | CF           |
| JNC         | Jump If No Carry                     | CF           |
| JO          | Jump If Overflow                     | OF           |
| JNO         | Jump If No Overflow                  | OF           |
| JP/JPE      | Jump Parity or Jump Parity Even      | PF           |
| JNP/JPO     | Jump No Parity or Jump Parity Odd    | PF           |
| JS          | Jump Sign (negative value)           | SF           |
| JNS         | Jump No Sign (positive value)        | SF           |
