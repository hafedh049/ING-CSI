Here’s the **Interrupt 21h table** with examples for each function to help you understand how they are implemented in 8086 assembly:

### **Interrupt 21h Functions in Assembly 8086**

| **Function (AH)** | **Functionality**                               | **Input Registers**                      | **Output Registers / Behavior**                           | **Example**                                            |
|-------------------|--------------------------------------------------|------------------------------------------|----------------------------------------------------------|--------------------------------------------------------|
| **01h**           | Read a character from input (with echo)          | None                                     | AL = Character read                                      | ```mov ah, 01h \n int 21h```                           |
| **02h**           | Display a character on output                    | DL = Character to display                | None                                                     | ```mov ah, 02h \n mov dl, 'A' \n int 21h```            |
| **06h**           | Direct console I/O (read/write)                  | DL = 0xFF for read; DL = char for write | AL = Input char (if read), or ZF set if no input         | ```mov ah, 06h \n mov dl, 0FFh \n int 21h```           |
| **07h**           | Read character from input (no echo)              | None                                     | AL = Character read                                      | ```mov ah, 07h \n int 21h```                           |
| **08h**           | Read character (no echo, no wait)                | None                                     | AL = Character or 0 if unavailable                       | ```mov ah, 08h \n int 21h```                           |
| **09h**           | Display a string                                | DX = Offset of string (terminated by '$') | None                                                    | ```mov ah, 09h \n lea dx, msg \n int 21h``` (msg: 'Hello$') |
| **0Ah**           | Buffered input                                   | DX = Pointer to input buffer             | Input stored in buffer                                   | ```mov ah, 0Ah \n lea dx, buffer \n int 21h```         |
| **0Bh**           | Check for character availability                 | None                                     | AL = 0 if no character, AL = 0xFF if available           | ```mov ah, 0Bh \n int 21h```                           |
| **0Ch**           | Clear keyboard buffer and read input             | None                                     | AL = Character read                                      | ```mov ah, 0Ch \n int 21h```                           |
| **0Dh**           | Disk reset                                       | None                                     | None                                                     | ```mov ah, 0Dh \n int 21h```                           |
| **19h**           | Get current drive                                | None                                     | AL = Current drive number (0 = A:, 1 = B:, etc.)         | ```mov ah, 19h \n int 21h```                           |
```python

section .data
    msg db 'Enter a two-digit number: $'  ; Message to display
    result db 'The whole number is: $'    ; Output message

section .bss
    num resb 1  ; To store the final whole number

section .text
    global _start

_start:
    ; Display the message
    mov ah, 09h
    lea dx, msg
    int 21h

    ; Read first character (digit 1)
    mov ah, 01h
    int 21h
    sub al, '0'    ; Convert from ASCII to numeric
    mov bl, al     ; Store first digit in BL

    ; Read second character (digit 2)
    mov ah, 01h
    int 21h
    sub al, '0'    ; Convert from ASCII to numeric

    ; Combine the two digits
    mov bh, 0      ; Clear BH to use BX for multiplication
    mov ax, 10     ; Multiply first digit (in BL) by 10
    mul bl         ; AX = BL * 10
    add ax, bx     ; AX = AX + AL (whole number in AX)

    ; Store the result in the 'num' variable (optional)
    mov [num], al

    ; Display result message
    mov ah, 09h
    lea dx, result
    int 21h

    ; Exit the program
    mov ah, 4Ch
    int 21h

```
