# Processor Operations and Registers in Assembly

Processor operations mostly involve processing data. This data can be stored in memory and accessed from thereon. However, reading from and storing data into memory slows down the processor due to the complicated process of sending the data request across the control bus to the memory storage unit and retrieving the data through the same channel.

To speed up processor operations, the processor includes some internal memory storage locations called **registers**.

Registers store data elements for processing without accessing the memory. A limited number of registers are built into the processor chip.

## Processor Registers

There are ten 32-bit and six 16-bit processor registers in the IA-32 architecture. The registers are grouped into three categories:

1. **General registers**
2. **Control registers**
3. **Segment registers**

The general registers are further divided into the following groups:

- **Data registers**
- **Pointer registers**
- **Index registers**

### Data Registers

Four 32-bit data registers are used for arithmetic, logical, and other operations. These 32-bit registers can be used in three ways:

1. As complete 32-bit data registers: **EAX**, **EBX**, **ECX**, **EDX**.
2. Lower halves of the 32-bit registers can be used as four 16-bit data registers: **AX**, **BX**, **CX**, **DX**.
3. Lower and higher halves of the above-mentioned four 16-bit registers can be used as eight 8-bit data registers: **AH**, **AL**, **BH**, **BL**, **CH**, **CL**, **DH**, and **DL**.

**Data Registers:**
- **AX** (Accumulator): Used in input/output and most arithmetic instructions. In multiplication, one operand is stored in **EAX**, **AX**, or **AL** depending on the operand size.
- **BX** (Base Register): Used in indexed addressing.
- **CX** (Count Register): Stores loop counts in iterative operations.
- **DX** (Data Register): Used in input/output operations and multiplication/division involving large values with **AX**.

### Pointer Registers

The pointer registers are 32-bit **EIP**, **ESP**, and **EBP** and their corresponding 16-bit right portions: **IP**, **SP**, **BP**. There are three types of pointer registers:

- **Instruction Pointer (IP)**: Stores the offset address of the next instruction to be executed. **IP** and **CS** form **CS:IP** to give the full address of the current instruction.
- **Stack Pointer (SP)**: Provides the offset value within the program stack. **SS:SP** refers to the current position of data or addresses within the stack.
- **Base Pointer (BP)**: Helps in referencing parameters passed to subroutines. **BP** can also be combined with **DI** and **SI** for special addressing.

### Index Registers

The 32-bit index registers, **ESI** and **EDI**, and their 16-bit rightmost portions, **SI** and **DI**, are used for indexed addressing and sometimes in addition/subtraction. There are two sets of index pointers:

- **Source Index (SI)**: Used as the source index for string operations.
- **Destination Index (DI)**: Used as the destination index for string operations.

### Control Registers

The 32-bit **instruction pointer register** and **flags register** are considered control registers. Many instructions involve comparisons and mathematical calculations that change the status of flags. Conditional instructions use these flags to determine the control flow.

The common flag bits are:

- **Overflow Flag (OF)**: Indicates overflow of the high-order bit after signed arithmetic.
- **Direction Flag (DF)**: Determines the direction for string operations (left-to-right if 0, right-to-left if 1).
- **Interrupt Flag (IF)**: Determines whether external interrupts should be ignored (0) or processed (1).
- **Trap Flag (TF)**: Allows setting the processor in single-step mode for debugging.
- **Sign Flag (SF)**: Indicates the sign of the result after an arithmetic operation.
- **Zero Flag (ZF)**: Indicates whether the result of an arithmetic or comparison operation is zero.
- **Auxiliary Carry Flag (AF)**: Contains the carry from bit 3 to bit 4 after arithmetic operations.
- **Parity Flag (PF)**: Indicates whether the total number of 1-bits is even (0) or odd (1).
- **Carry Flag (CF)**: Contains the carry from the high-order bit after an arithmetic operation.

### Segment Registers

Segments are specific areas defined in a program for containing data, code, and stack. There are three main segments:

- **Code Segment (CS)**: Contains all instructions to be executed.
- **Data Segment (DS)**: Contains data, constants, and work areas.
- **Stack Segment (SS)**: Contains data and return addresses for procedures.

Additional segment registers (**ES**, **FS**, **GS**) provide extra segments for storing data. Segment addresses in the segment registers are combined with offset values to reference specific memory locations.

### Example Program

The following program demonstrates the use of registers in assembly programming by displaying nine stars on the screen along with a simple message:

```c
section .text
   global _start    ; must be declared for linker (gcc)

_start:             ; tell linker entry point
   mov edx, len     ; message length
   mov ecx, msg     ; message to write
   mov ebx, 1       ; file descriptor (stdout)
   mov eax, 4       ; system call number (sys_write)
   int 0x80         ; call kernel

   mov edx, 9       ; message length
   mov ecx, s2      ; message to write
   mov ebx, 1       ; file descriptor (stdout)
   mov eax, 4       ; system call number (sys_write)
   int 0x80         ; call kernel

   mov eax, 1       ; system call number (sys_exit)
   int 0x80         ; call kernel

section .data
msg db 'Displaying 9 stars', 0xa ; message
len equ $ - msg                  ; length of message
s2 times 9 db '*'                ; nine stars
```

When the above code is compiled and executed, it produces the following result:

```
Displaying 9 stars
*********
```