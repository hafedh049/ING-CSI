### System Calls in Assembly Language

System calls are APIs that provide an interface between the user space and the kernel space. We have already used system calls like `sys_write` and `sys_exit` to write to the screen and exit a program, respectively.

#### Linux System Calls

Linux system calls can be used in your assembly programs, providing a bridge to access kernel services. To make use of Linux system calls in assembly, you generally follow these steps:

1. Put the system call number in the `EAX` register.
2. Store the arguments for the system call in registers like `EBX`, `ECX`, and others.
3. Call the appropriate interrupt (`int 80h`).
4. The result is typically returned in the `EAX` register.

There are six registers used for storing the arguments of system calls: `EBX`, `ECX`, `EDX`, `ESI`, `EDI`, and `EBP`. These registers take the arguments in sequence, starting with `EBX`. If more than six arguments are required, the memory address of the first argument is stored in `EBX`.

The following example shows the use of the `sys_exit` system call:

```c
mov eax, 1       ; system call number (sys_exit)
int 0x80         ; call kernel
```

The following example shows the use of the `sys_write` system call:

```c
mov edx, 4       ; message length
mov ecx, msg     ; message to write
mov ebx, 1       ; file descriptor (stdout)
mov eax, 4       ; system call number (sys_write)
int 0x80         ; call kernel
```

All system calls are listed in `/usr/include/asm/unistd.h`, along with their numbers (the value to put in `EAX` before calling `int 80h`).

Below is a table showing some of the system calls used in this tutorial:

| %eax      | Name       | %ebx            | %ecx          | %edx         | %esx | %edi |
|-----------|------------|-----------------|---------------|--------------|------|------|
| 1         | sys_exit   | int             | -             | -            | -    | -    |
| 2         | sys_fork   | struct pt_regs  | -             | -            | -    | -    |
| 3         | sys_read   | unsigned int    | char *        | size_t       | -    | -    |
| 4         | sys_write  | unsigned int    | const char *  | size_t       | -    | -    |
| 5         | sys_open   | const char *    | int           | int          | -    | -    |
| 6         | sys_close  | unsigned int    | -             | -            | -    | -    |

#### Example: Reading and Displaying a Number

The following example reads a number from the keyboard and displays it on the screen:

```c
section .data
   userMsg db 'Please enter a number: ' ; Prompt message to the user
   lenUserMsg equ $ - userMsg           ; Length of the prompt message
   dispMsg db 'You have entered: '
   lenDispMsg equ $ - dispMsg           ; Length of the display message

section .bss
   num resb 5                           ; Reserve 5 bytes for user input

section .text
   global _start

_start:
   ; User prompt
   mov eax, 4
   mov ebx, 1
   mov ecx, userMsg
   mov edx, lenUserMsg
   int 80h

   ; Read and store the user input
   mov eax, 3
   mov ebx, 2
   mov ecx, num
   mov edx, 5                            ; Read up to 5 bytes (numeric, 1 for sign)
   int 80h

   ; Output the message 'You have entered: '
   mov eax, 4
   mov ebx, 1
   mov ecx, dispMsg
   mov edx, lenDispMsg
   int 80h

   ; Output the number entered
   mov eax, 4
   mov ebx, 1
   mov ecx, num
   mov edx, 5
   int 80h

   ; Exit code
   mov eax, 1
   mov ebx, 0
   int 80h
```

#### Output

When compiled and executed, the program produces the following result:

```c
Please enter a number:
1234
You have entered: 1234
```