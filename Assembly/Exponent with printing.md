```c
data segment
    prompt1 db 13,10, "Enter the base number: $"
    prompt2 db 13,10, "Enter the exponent: $"
    result_msg db 13,10, "Result: $"
ends

stack segment
    dw 128 dup(0)
ends

code segment
    assume cs:code, ss:stack, ds:data
start:
    mov ax, data
    mov ds, ax

    mov ax, stack
    mov ss, ax

    lea dx, prompt1
    mov ah, 9
    int 21h

    mov ah, 01       
    int 21h
    sub al, '0'      
    mov bl, al       


    lea dx, prompt2
    mov ah, 9
    int 21h


    mov ah, 01      
    int 21h
    sub al, '0'       
    mov cl, al      

  
    mov al, 1         
    mov ch, 0       
    mov ah, 0
    
    power:
    mul bl        
    dec cx          
    jnz power  
       
    mov bx, ax
    
    lea dx, result_msg
    mov ah, 9
    int 21h
    
    mov ax, bx
    
    mov bl, 10     
    mov cx, 0      
  
    
    division:
        xor dx, dx
        div bl
        mov dl, ah
        push dx
        mov ah, 0
        inc cx
    test al, al
    jnz division
    
    show:
        pop dx
        add dx, 30h
        mov ah, 02
        int 21h
    loop show

    mov ax, 4c00h
    int 21h
ends

end start
```