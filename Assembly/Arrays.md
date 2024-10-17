### Data Definition Directives

Data definition directives are used in assembly language to allocate storage for variables. They can also initialize these variables with specific values, which can be represented in hexadecimal, decimal, or binary form.

#### Examples of Variable Initialization

You can define a word variable, for example, `MONTHS`, in the following ways:

```c
MONTHS DW 12      ; Decimal initialization
MONTHS DW 0CH     ; Hexadecimal initialization
MONTHS DW 0110B   ; Binary initialization
```

#### Defining One-Dimensional Arrays

One-dimensional arrays can be defined using data definition directives. For example:

```c
NUMBERS DW 34, 45, 56, 67, 75, 89
```

This line declares an array named `NUMBERS` that consists of six words initialized with the values 34, 45, 56, 67, 75, and 89, allocating a total of \( 2 \times 6 = 12 \) bytes of memory. The symbolic address of the first element is `NUMBERS`, and the subsequent elements can be accessed using offsets (e.g., `NUMBERS + 2`, `NUMBERS + 4`, etc.).

You can also define an array named `INVENTORY` of size 8 and initialize all values to zero:

```c
INVENTORY DW 0  ; Initializing each element separately
           DW 0
           DW 0
           DW 0
           DW 0
           DW 0
           DW 0
           DW 0
```

This can be abbreviated as follows:

```c
INVENTORY DW 0, 0, 0, 0, 0, 0, 0, 0
```

#### Using the `TIMES` Directive

The `TIMES` directive can be employed for multiple initializations to the same value. For example, you can define the `INVENTORY` array like this:

```c
INVENTORY TIMES 8 DW 0  ; Initializes an array of 8 words to zero
```

### Example Program: Summing Array Values

Here is an example program that demonstrates the above concepts by defining a 3-element array `x`, summing its values, and displaying the result:

```c
section .text
    global _start  ; Must be declared for the linker (ld)
	
_start:	
    mov eax, 3        ; Number of bytes to be summed 
    mov ebx, 0        ; EBX will store the sum
    mov ecx, x        ; ECX points to the current element to be summed

top:
    add ebx, [ecx]    ; Add the current element to EBX
    add ecx, 1        ; Move pointer to the next element
    dec eax           ; Decrement counter
    jnz top           ; If counter not 0, loop again

done: 
    add ebx, '0'      ; Convert sum to ASCII
    mov [sum], ebx    ; Store result in "sum"

display:
    mov edx, 1        ; Message length
    mov ecx, sum      ; Message to write
    mov ebx, 1        ; File descriptor (stdout)
    mov eax, 4        ; System call number (sys_write)
    int 0x80          ; Call kernel
	
    mov eax, 1        ; System call number (sys_exit)
    int 0x80          ; Call kernel

section .data
global x
x:    
    db 2              ; First element
    db 4              ; Second element
    db 3              ; Third element

sum: 
    db 0              ; Variable to store the result
```

### Program Execution

When the above code is compiled and executed, it produces the output:

```
9
```

This program illustrates how to define variables and arrays, perform arithmetic operations on array elements, and display the results using system calls.