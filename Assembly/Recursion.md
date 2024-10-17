### Recursive Procedures

A recursive procedure is one that calls itself to solve a problem. There are two types of recursion:

1. **Direct Recursion**: The procedure calls itself directly.
2. **Indirect Recursion**: One procedure calls another, which eventually calls the first procedure.

Recursion is commonly used in mathematical algorithms, such as calculating factorials. The factorial of a number \( n \) is defined as:

$$
\text{Fact}(n) = n \times \text{Fact}(n-1) \quad \text{for } n > 0
$$

### Ending Condition

Every recursive procedure must have a base case (ending condition) to avoid infinite recursion. For the factorial algorithm, the base case is:

$$ If ( n = 0 ), then (\text{Fact}(0) = 1) $$

### Example: Factorial Calculation in Assembly Language

The following assembly language program calculates the factorial of 3 using a recursive procedure.

#### Assembly Code

```c
section .text
   global _start         ; Must be declared for using gcc
	
_start:                  ; Tell linker entry point
   mov bx, 3             ; For calculating factorial of 3
   call proc_fact        ; Call the factorial procedure
   add ax, 30h           ; Convert result to ASCII
   mov [fact], ax        ; Store the result in 'fact'
    
   mov edx, len          ; Message length
   mov ecx, msg          ; Message to write
   mov ebx, 1            ; File descriptor (stdout)
   mov eax, 4            ; System call number (sys_write)
   int 0x80              ; Call kernel

   mov edx, 1            ; Message length for factorial result
   mov ecx, fact         ; Message to write
   mov ebx, 1            ; File descriptor (stdout)
   mov eax, 4            ; System call number (sys_write)
   int 0x80              ; Call kernel
    
   mov eax, 1            ; System call number (sys_exit)
   int 0x80              ; Call kernel
	
proc_fact:
   cmp bl, 1             ; Compare BL with 1
   jg do_calculation      ; If BL > 1, do calculation
   mov ax, 1             ; Base case: if n = 1, return 1
   ret
	
do_calculation:
   dec bl                ; Decrement BL (n - 1)
   call proc_fact        ; Recursive call
   inc bl                ; Increment BL (n)
   mul bl                ; Multiply AX by BL (ax = ax * bl)
   ret

section .data
msg db 'Factorial 3 is:', 0xa	
len equ $ - msg			 

section .bss
fact resb 1              ; Reserve space for the result
```

### How the Code Works

1. **Entry Point**: The program starts executing at the `_start` label. It initializes the `BX` register to 3, which is the value for which we want to calculate the factorial.
  
2. **Calling the Factorial Procedure**: The `call proc_fact` instruction invokes the `proc_fact` procedure.

3. **Recursive Procedure**:
   - The `proc_fact` checks if the value in `BL` is greater than 1.
   - If it is, the procedure decrements `BL` (representing \( n - 1 \)) and calls itself again. This continues until `BL` equals 1, at which point it returns 1 (the base case).
   - After returning from the recursive call, it increments `BL` back to its original value and multiplies the result by `BL` (the current \( n \)).
  
4. **Storing and Printing the Result**: Once the calculation is complete, the result is converted to ASCII by adding 30h (to display the number correctly) and stored in the `fact` variable. The program then writes a message and the result to the standard output.

### Output

When this program is compiled and executed, it produces the following output:

```c
Factorial 3 is:
6
```
