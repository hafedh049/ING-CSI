### Operand Addressing in Assembly Language

In assembly language, instructions often require operands to be processed. The operand address indicates where the data to be processed is located. Some instructions may have no operands, while others may require one, two, or even three operands.

For instructions with two operands, the first operand is generally the destination (register or memory) where the data will be stored, while the second operand is the source. The source can either contain the data (in immediate addressing) or refer to the address where the data is stored. Usually, the source data remains unchanged after the operation.

The three main addressing modes are:

- Register addressing
- Immediate addressing
- Memory addressing

#### Register Addressing

In register addressing mode, the operand is stored in a register. Depending on the instruction, the register can be the first operand, the second operand, or both.

Examples:

```c
MOV DX, TAX_RATE   ; Register in first operand
MOV COUNT, CX      ; Register in second operand
MOV EAX, EBX       ; Both operands are registers
```

Since data is processed between registers without involving memory, this addressing mode provides the fastest processing of data.

#### Immediate Addressing

In immediate addressing mode, an operand has a constant value or expression. If an instruction has two operands, the first operand can be a register or a memory location, while the second operand is a constant.

Examples:

```c
BYTE_VALUE  DB  150    ; Define a byte value
WORD_VALUE  DW  300    ; Define a word value
ADD  BYTE_VALUE, 65    ; Add immediate value 65
MOV  AX, 45H           ; Load immediate value 45H into AX
```

#### Direct Memory Addressing

In direct memory addressing mode, direct access to the main memory is required, typically in the data segment. The assembler calculates an offset value, called the "effective address," to locate the data in memory. This offset is often indicated by the variable name.

Examples:

```assembly
ADD BYTE_VALUE, DL    ; Adds the register value in the memory location
MOV BX, WORD_VALUE    ; Move value from memory to register
```

#### Direct-Offset Addressing

Direct-offset addressing mode uses arithmetic operators to modify an address. You can use it to access elements of arrays or tables.

Examples:

```c
BYTE_TABLE DB 14, 15, 22, 45     ; Define a table of bytes
WORD_TABLE DW 134, 345, 564, 123 ; Define a table of words

MOV CL, BYTE_TABLE[2]    ; Access the 3rd element of BYTE_TABLE
MOV CX, WORD_TABLE[3]    ; Access the 4th element of WORD_TABLE
```

#### Indirect Memory Addressing

In indirect memory addressing mode, the base registers (EBX, EBP) or index registers (DI, SI) are used, coded within square brackets. This is typically used for arrays where the starting address is in a register.

Examples:

```c
MY_TABLE TIMES 10 DW 0 ; Allocate 10 words, each initialized to 0
MOV EBX, MY_TABLE      ; Load effective address of MY_TABLE into EBX
MOV [EBX], 110         ; Set MY_TABLE[0] to 110
ADD EBX, 2             ; Increment EBX by 2
MOV [EBX], 123         ; Set MY_TABLE[1] to 123
```

### The MOV Instruction

The `MOV` instruction is used to transfer data from one location to another. It takes two operands, and its syntax is:

```c
MOV destination, source
```

The `MOV` instruction can be used in the following forms:

- `MOV register, register`
- `MOV register, immediate`
- `MOV memory, immediate`
- `MOV register, memory`
- `MOV memory, register`

Note that both operands must be of the same size, and the source value remains unchanged.

If the `MOV` instruction causes ambiguity, such as when specifying the size of the value being moved, it is good to use a type specifier. The following table shows some type specifiers:

| Type Specifier | Bytes Addressed |
|----------------|-----------------|
| BYTE           | 1               |
| WORD           | 2               |
| DWORD          | 4               |
| QWORD          | 8               |
| TBYTE          | 10              |

### Example: Changing and Displaying a String

The following program demonstrates some of these concepts. It stores the name "Zara Ali" in memory and then changes it to "Nuha Ali" before displaying both names.

```c
section .text
   global _start        ; Must be declared for the linker (ld)

_start:
   ; Writing the name 'Zara Ali'
   mov edx, 9           ; Message length
   mov ecx, name        ; Message to write
   mov ebx, 1           ; File descriptor (stdout)
   mov eax, 4           ; System call number (sys_write)
   int 0x80             ; Call kernel

   ; Change the name to 'Nuha Ali'
   mov dword [name], 'Nuha'    ; Change the first four characters of name

   ; Writing the name 'Nuha Ali'
   mov edx, 8           ; Message length
   mov ecx, name        ; Message to write
   mov ebx, 1           ; File descriptor (stdout)
   mov eax, 4           ; System call number (sys_write)
   int 0x80             ; Call kernel

   ; Exit code
   mov eax, 1           ; System call number (sys_exit)
   int 0x80             ; Call kernel

section .data
name db 'Zara Ali '
```

### Output

When the code above is compiled and executed, it produces the following output:

```c
Zara Ali Nuha Ali
```