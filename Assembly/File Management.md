### Key Concepts

#### File Streams
In the context of file handling, there are three standard file streams:
- **Standard Input (stdin)**: Used for input operations (File Descriptor: **0**).
- **Standard Output (stdout)**: Used for output operations (File Descriptor: **1**).
- **Standard Error (stderr)**: Used for error messages (File Descriptor: **2**).

#### File Descriptor
A file descriptor is a unique identifier (typically a 16-bit integer) assigned to an open file. The file descriptor is used to access the file during read and write operations. 

- File Descriptors:
  - **stdin**: 0
  - **stdout**: 1
  - **stderr**: 2

#### File Pointer
A file pointer indicates the current position for reading from or writing to the file, represented as an offset in bytes from the beginning of the file. When a file is opened, the pointer is initially set to zero.

### System Calls for File Handling
The following table summarizes the system calls related to file handling:

| %eax | Name           | %ebx                     | %ecx               | %edx               |
|------|----------------|--------------------------|---------------------|---------------------|
| 2    | sys_fork       | struct pt_regs           | -                   | -                   |
| 3    | sys_read       | unsigned int             | char *              | size_t              |
| 4    | sys_write      | unsigned int             | const char *        | size_t              |
| 5    | sys_open       | const char *             | int                 | int                 |
| 6    | sys_close      | unsigned int             | -                   | -                   |
| 8    | sys_creat      | const char *             | int                 | -                   |
| 19   | sys_lseek      | unsigned int             | off_t               | unsigned int        |

### Steps for Using System Calls
1. Place the system call number in the `EAX` register.
2. Store the required arguments in `EBX`, `ECX`, and `EDX`.
3. Call the relevant interrupt (`int 0x80`).
4. The result is returned in the `EAX` register.

### Example Program
The following NASM program demonstrates creating and opening a file named `myfile.txt`, writing a message to it, reading the message back, and displaying it.

```c
section .text
   global _start         ; Must be declared for using gcc
	
_start:                  ; Tell linker entry point
   ; Create the file
   mov  eax, 8           ; sys_creat
   mov  ebx, file_name   ; File name
   mov  ecx, 0777        ; Permissions (read, write, execute for all)
   int  0x80             ; Call kernel
	
   mov [fd_out], eax     ; Store file descriptor for writing
    
   ; Write into the file
   mov	edx, len         ; Number of bytes to write
   mov	ecx, msg         ; Message to write
   mov	ebx, [fd_out]    ; File descriptor 
   mov	eax, 4            ; sys_write
   int	0x80             ; Call kernel
	
   ; Close the file
   mov eax, 6            ; sys_close
   mov ebx, [fd_out]     ; File descriptor
   int  0x80             ; Call kernel
    
   ; Indicate end of file write
   mov eax, 4            ; sys_write
   mov ebx, 1            ; stdout
   mov ecx, msg_done     ; Message indicating end of file write
   mov edx, len_done     ; Length of message
   int  0x80             ; Call kernel
    
   ; Open the file for reading
   mov eax, 5            ; sys_open
   mov ebx, file_name    ; File name
   mov ecx, 0            ; Read only access
   mov edx, 0777         ; Permissions
   int  0x80             ; Call kernel
	
   mov  [fd_in], eax     ; Store file descriptor for reading
    
   ; Read from file
   mov eax, 3            ; sys_read
   mov ebx, [fd_in]      ; File descriptor
   mov ecx, info         ; Buffer to read into
   mov edx, 26           ; Number of bytes to read
   int 0x80              ; Call kernel
    
   ; Close the file
   mov eax, 6            ; sys_close
   mov ebx, [fd_in]      ; File descriptor
   int  0x80             ; Call kernel
	
   ; Print the info 
   mov eax, 4            ; sys_write
   mov ebx, 1            ; stdout
   mov ecx, info         ; Buffer to print
   mov edx, 26           ; Number of bytes to print
   int 0x80              ; Call kernel
       
   mov	eax, 1           ; sys_exit
   int	0x80             ; Call kernel

section .data
file_name db 'myfile.txt', 0
msg db 'Welcome to Tutorials Point', 0
len equ  $ - msg

msg_done db 'Written to file', 0xA
len_done equ $ - msg_done

section .bss
fd_out resb 1           ; File descriptor for writing
fd_in  resb 1           ; File descriptor for reading
info resb  26           ; Buffer to store read data
```

### Explanation of the Program

1. **File Creation**:
   - The program creates a file named `myfile.txt` using the `sys_creat` system call. The file permissions are set to read, write, and execute for all users.

2. **Writing to the File**:
   - It writes the message "Welcome to Tutorials Point" to the file using the `sys_write` system call.

3. **Closing the File**:
   - The program then closes the file.

4. **Indicating Completion**:
   - It writes a message to `stdout` indicating that the write operation is complete.

5. **Opening the File for Reading**:
   - The program opens the same file for reading using the `sys_open` system call.

6. **Reading from the File**:
   - It reads the contents of the file into a buffer named `info` using the `sys_read` system call.

7. **Closing the File Again**:
   - After reading, the program closes the file.

8. **Displaying the Read Data**:
   - Finally, it prints the contents of the `info` buffer to `stdout`.

### Output

When the above code is compiled and executed, it produces the following output:

```c
Written to file
Welcome to Tutorials Point
```

### Conclusion

This program demonstrates how to perform basic file operations in assembly language using NASM. It showcases the use of system calls for creating, writing, reading, and closing files, as well as how to handle standard output. If you have any questions or need further clarification, feel free to ask!