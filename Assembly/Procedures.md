### Procedures in Assembly Language

Procedures (or subroutines) are essential in assembly language programming for structuring code and managing complexity. A procedure is defined by a name and contains a sequence of instructions to perform a specific task. Control is transferred to the procedure using the `CALL` instruction, and it returns control back to the calling program using the `RET` instruction.

#### Syntax

The syntax for defining a procedure is as follows:

```c
proc_name:
   ; Procedure body
   ...
   ret
```

The procedure can be called from another part of the program like this:

```c
CALL proc_name
```

### Example: Sum Procedure

Here is an example program that demonstrates the use of a simple procedure called `sum`, which adds two values stored in the ECX and EDX registers and returns the result in the EAX register:

```c
section .text
   global _start        ; Must be declared for using gcc
	
_start:	                ; Tell linker entry point
   mov	ecx, '4'      ; Load ASCII character '4' into ECX
   sub     ecx, '0'    ; Convert ASCII to integer
	
   mov 	edx, '5'      ; Load ASCII character '5' into EDX
   sub     edx, '0'    ; Convert ASCII to integer
	
   call    sum          ; Call sum procedure
   mov	[res], eax     ; Store result in 'res'
   mov	ecx, msg       ; Load message address into ECX
   mov	edx, len       ; Load message length into EDX
   mov	ebx, 1	        ; File descriptor (stdout)
   mov	eax, 4	        ; System call number (sys_write)
   int	0x80	        ; Call kernel
	
   mov	ecx, res       ; Load result address into ECX
   mov	edx, 1         ; Set length to 1 for output
   mov	ebx, 1	        ; File descriptor (stdout)
   mov	eax, 4	        ; System call number (sys_write)
   int	0x80	        ; Call kernel
	
   mov	eax, 1	        ; System call number (sys_exit)
   int	0x80	        ; Call kernel

sum:
   mov     eax, ecx     ; Move value from ECX to EAX
   add     eax, edx     ; Add value from EDX to EAX
   add     eax, '0'     ; Convert back to ASCII
   ret
	
section .data
msg db "The sum is:", 0xA, 0xD 
len equ $ - msg   

segment .bss
res resb 1
```

### Output

When the code is compiled and executed, the output will be:

```c
The sum is:
9
```

### Stack Data Structure

A stack is a data structure that follows the Last In First Out (LIFO) principle. Data is stored and retrieved from a location known as the "top" of the stack. The `PUSH` instruction is used to add data to the stack, while the `POP` instruction is used to remove data from it.

#### Stack Operations

- **PUSH Instruction**: Adds data to the stack.
    ```c
    PUSH operand
    ```

- **POP Instruction**: Removes data from the stack.
    ```c
    POP address/register
    ```

### Characteristics of the Stack

- Only words or doublewords can be saved in the stack, not bytes.
- The stack grows towards lower memory addresses.
- The top of the stack points to the last item inserted.

#### Example: Saving and Restoring Registers

Here’s how to save and restore registers using the stack:

```c
; Save the AX and BX registers in the stack
PUSH    AX
PUSH    BX

; Use the registers for other purposes
MOV	AX, VALUE1
MOV 	BX, VALUE2
...
MOV	VALUE1, AX
MOV	VALUE2, BX

; Restore the original values
POP	BX
POP	AX
```

### Example: Displaying the ASCII Character Set

The following program demonstrates the use of a stack to display the entire ASCII character set by calling a procedure named `display`.

```c
section .text
   global _start        ; Must be declared for using gcc
	
_start:	                ; Tell linker entry point
   call    display
   mov	eax, 1	        ; System call number (sys_exit)
   int	0x80	        ; Call kernel
	
display:
   mov    ecx, 256      ; Loop counter for ASCII characters
	
next:
   push    ecx          ; Save counter on the stack
   mov     eax, 4       ; System call number (sys_write)
   mov     ebx, 1       ; File descriptor (stdout)
   mov     ecx, achar    ; Address of character to print
   mov     edx, 1       ; Number of bytes to write
   int     80h          ; Call kernel
	
   pop     ecx          ; Restore counter from the stack
   mov	dx, [achar]   ; Move character to DX for comparison
   cmp	byte [achar], 0dh ; Check for carriage return
   inc	byte [achar]    ; Increment character
   loop    next         ; Loop until all characters are printed
   ret
	
section .data
achar db '0'           ; Starting character
```

### Output

When this code is compiled and executed, it produces the following output, which displays the ASCII character set:

```c
0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}
...
```