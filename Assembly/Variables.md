## Allocating Storage Space for Uninitialized Data
The reserve directives are used for reserving space for uninitialized data. These directives take a single operand that specifies the number of units of space to be reserved. Each define directive has a related reserve directive for uninitialized data:

- `RESB` – Reserve a Byte
- `RESW` – Reserve a Word (2 bytes)
- `RESD` – Reserve a Doubleword (4 bytes)
- `RESQ` – Reserve a Quadword (8 bytes)
- `REST` – Reserve Ten Bytes (10 bytes)

The reserve directives do not initialize the memory; they simply reserve it for future use.

For example:

```c
section .bss
buffer  RESB  64   ; Reserve 64 bytes for a buffer
count   RESW  1    ; Reserve 1 word (2 bytes) for a counter
``` 

In this example, `buffer` reserves 64 bytes of memory, while `count` reserves space for a word (2 bytes).

### Multiple Definitions
You can have multiple data definition statements in a program. For example:

```c
choice   DB  'Y'          ; ASCII of 'Y' = 79H
number1  DW  12345        ; 12345D = 3039H
number2  DD  123456789    ; 123456789D = 75BCD15H
```

The assembler allocates contiguous memory for these multiple variable definitions.

### Multiple Initializations
The `TIMES` directive allows you to initialize multiple elements to the same value. This is useful for defining arrays and tables.

For example, an array named `marks` of size 9 can be defined and initialized to zero as follows:

```c
marks  TIMES  9  DW  0
```

The `TIMES` directive is also useful for repeating characters or values. The following example program uses the `TIMES` directive to display 9 asterisks on the screen:

#### Example Program
```c
section .text
   global _start        ; must be declared for linker (ld)
   
_start:                 ; tell linker entry point
   mov   edx, 9         ; message length
   mov   ecx, stars     ; message to write
   mov   ebx, 1         ; file descriptor (stdout)
   mov   eax, 4         ; system call number (sys_write)
   int   0x80           ; call kernel

   mov   eax, 1         ; system call number (sys_exit)
   int   0x80           ; call kernel

section .data
stars   TIMES 9 DB '*'
```

When the above code is compiled and executed, it produces the following result:

```
*********
```

The `stars` variable is defined with the `TIMES` directive to repeat the asterisk character (`'*'`) 9 times, and the program writes these characters to the standard output.