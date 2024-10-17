Conditional execution in assembly language is achieved using both unconditional and conditional jump instructions, allowing programs to change their flow based on specific conditions.

### Conditional Execution Types
1. **Unconditional Jump**: 
   - Performed by the `JMP` instruction.
   - It transfers control directly to a specified label, regardless of conditions.
   - Syntax: `JMP label`
   
   Example:
   ```assembly
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
  ```assembly
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

### Example: Finding the Largest of Three Numbers
This example program finds the largest of three variables: `num1 = 47`, `num2 = 22`, `num3 = 31`.

```assembly
section .text
   global _start         ; must be declared for using gcc

_start:                  ; tell linker entry point
   mov ecx, [num1]       ; Move num1 to ECX
   cmp ecx, [num2]       ; Compare ECX with num2
   jg check_third_num    ; If ECX is greater, skip to next comparison
   mov ecx, [num2]       ; Otherwise, move num2 to ECX
   
check_third_num:
   cmp ecx, [num3]       ; Compare ECX with num3
   jg _exit              ; If ECX is greater, skip to _exit
   mov ecx, [num3]       ; Otherwise, move num3 to ECX

_exit:
   mov [largest], ecx    ; Store the largest value in 'largest'
   mov ecx, msg
   mov edx, len
   mov ebx, 1            ; File descriptor (stdout)
   mov eax, 4            ; System call number (sys_write)
   int 0x80              ; Call kernel

   mov ecx, largest
   mov edx, 2
   mov ebx, 1            ; File descriptor (stdout)
   mov eax, 4            ; System call number (sys_write)
   int 0x80              ; Call kernel
    
   mov eax, 1            ; System call number (sys_exit)
   int 0x80              ; Call kernel

section .data
   msg db "The largest digit is: ", 0xA, 0xD
   len equ $ - msg
   num1 dd '47'
   num2 dd '22'
   num3 dd '31'

segment .bss
   largest resb 2
```
**Output**:
```
The largest digit is: 
47
```

In this program:
- The values `num1`, `num2`, and `num3` are compared using the `CMP` and conditional jump instructions to find the largest.
- The result is printed to the console using system calls.