### Defining Constants in NASM
NASM provides several directives to define constants. Among these, we will discuss three directives:

- `EQU`
- `%assign`
- `%define`

#### The EQU Directive
The `EQU` directive is used for defining constants. Its syntax is:

```c
CONSTANT_NAME EQU expression
```

For example:

```c
TOTAL_STUDENTS EQU 50
```

You can then use this constant value in your code:

```c
mov ecx, TOTAL_STUDENTS
cmp eax, TOTAL_STUDENTS
```

The operand of an `EQU` statement can also be an expression:

```c
LENGTH EQU 20
WIDTH  EQU 10
AREA   EQU LENGTH * WIDTH
```

In the above code segment, `AREA` would be defined as `200`.

##### Example
The following example illustrates the use of the `EQU` directive:

```c
SYS_EXIT  EQU 1
SYS_WRITE EQU 4
STDIN     EQU 0
STDOUT    EQU 1

section .text
   global _start    ; must be declared for using gcc
   
_start:             ; tell linker entry point
   mov eax, SYS_WRITE         
   mov ebx, STDOUT         
   mov ecx, msg1         
   mov edx, len1 
   int 0x80                
   
   mov eax, SYS_WRITE         
   mov ebx, STDOUT         
   mov ecx, msg2         
   mov edx, len2 
   int 0x80 
   
   mov eax, SYS_WRITE         
   mov ebx, STDOUT         
   mov ecx, msg3         
   mov edx, len3 
   int 0x80
   
   mov eax, SYS_EXIT    ; system call number (sys_exit)
   int 0x80             ; call kernel

section .data
msg1 db  'Hello, programmers!', 0xA, 0xD   
len1 EQU $ - msg1            

msg2 db  'Welcome to the world of,', 0xA, 0xD 
len2 EQU $ - msg2 

msg3 db  'Linux assembly programming! '
len3 EQU $ - msg3
```

When the above code is compiled and executed, it produces the following result:

```
Hello, programmers!
Welcome to the world of,
Linux assembly programming!
```

#### The %assign Directive
The `%assign` directive can also be used to define numeric constants like the `EQU` directive. Unlike `EQU`, `%assign` allows redefinition of the constant value.

For example:

```c
%assign TOTAL 10
```

Later in the code, you can redefine it as:

```c
%assign TOTAL 20
```

The `%assign` directive is case-sensitive, meaning that `TOTAL` and `total` would be treated as different identifiers.

#### The %define Directive
The `%define` directive allows you to define both numeric and string constants. This directive is similar to `#define` in C, and it allows redefinition.

For example, you may define a pointer as:

```c
%define PTR [EBP+4]
```

In the above code, every occurrence of `PTR` will be replaced by `[EBP+4]`.

The `%define` directive is also case-sensitive, meaning it distinguishes between uppercase and lowercase letters when defining and using constants.