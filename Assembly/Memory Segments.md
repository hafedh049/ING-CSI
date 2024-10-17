# Memory Segments in Assembly Programs

We have already discussed the three sections of an assembly program. These sections also represent various memory segments.

Interestingly, if you replace the `section` keyword with `segment`, you will get the same result. Try the following code:

```c
segment .text      ; code segment
   global _start   ; must be declared for linker

_start:            ; tell linker entry point
   mov edx, len    ; message length
   mov ecx, msg    ; message to write
   mov ebx, 1      ; file descriptor (stdout)
   mov eax, 4      ; system call number (sys_write)
   int 0x80        ; call kernel

   mov eax, 1      ; system call number (sys_exit)
   int 0x80        ; call kernel

segment .data      ; data segment
msg db 'Hello, world!', 0xa  ; our dear string
len equ $ - msg              ; length of our dear string
```

When the above code is compiled and executed, it produces the following result:

```
Hello, world!
```

## Memory Segments

A segmented memory model divides the system memory into groups of independent segments, which are referenced by pointers located in the segment registers. Each segment is used to contain a specific type of data. One segment contains instruction codes, another stores data elements, and a third keeps the program stack.

Based on the above discussion, we can specify various memory segments as follows:

### Data Segment

The data segment is represented by the `.data` section and the `.bss` section.

- **.data Section**: Used to declare the memory region where data elements are stored for the program. This section cannot be expanded after the data elements are declared and remains static throughout the program.

- **.bss Section**: This is also a static memory section that contains buffers for data to be declared later in the program. This buffer memory is zero-filled.
### Code Segment

The code segment is represented by the `.text` section. It defines an area in memory that stores the instruction codes. This is also a fixed area.

### Stack Segment

The stack segment contains data values passed to functions and procedures within the program.