
# Assembly Program Sections

An assembly program can be divided into three sections:

1. The **data** section
2. The **bss** section
3. The **text** section
## The Data Section

The data section is used for declaring initialized data or constants. This data does not change at runtime. You can declare various constant values, file names, buffer sizes, etc., in this section.

The syntax for declaring the data section is:

```sass
section .data
```

## The BSS Section

The bss section is used for declaring variables. The syntax for declaring the bss section is:

```sass
section .bss
```

## The Text Section

The text section is used for keeping the actual code. This section must begin with the declaration `global _start`, which tells the kernel where the program execution begins.

The syntax for declaring the text section is:

```css
section .text
   global _start
_start:
```

## Comments

Assembly language comments begin with a semicolon (`;`). They can contain any printable character, including blank spaces. Comments can appear on a line by themselves, like:

```css
; This program displays a message on screen
```

or, on the same line as an instruction:

```css
add eax, ebx     ; adds ebx to eax
```

## Assembly Language Statements

Assembly language programs consist of three types of statements:

1. **Executable instructions** or simply **instructions**
2. **Assembler directives** or **pseudo-ops**
3. **Macros**

- **Executable instructions** tell the processor what to do. Each instruction consists of an operation code (opcode) and generates one machine language instruction.
- **Assembler directives** are non-executable and do not generate machine language instructions. They tell the assembler about various aspects of the assembly process.
- **Macros** are a text substitution mechanism.

### Syntax of Assembly Language Statements

Assembly language statements are entered one statement per line. Each statement follows this format:

```css
[label]   mnemonic   [operands]   [;comment]
```

The fields in square brackets are optional. A basic instruction has two parts: the **mnemonic**, which is the name of the instruction, and the **operands**, which are the parameters of the command.

### Examples of Assembly Language Statements

```c
INC COUNT        ; Increment the memory variable COUNT
MOV TOTAL, 48    ; Transfer the value 48 to the memory variable TOTAL
ADD AH, BH       ; Add the content of the BH register into the AH register
AND MASK1, 128   ; Perform AND operation on MASK1 and 128
ADD MARKS, 10    ; Add 10 to the variable MARKS
MOV AL, 10       ; Transfer the value 10 to the AL register
```

## The "Hello World" Program in Assembly

The following assembly language code displays the string "Hello, World!" on the screen:

```c
section .text
   global _start     ; must be declared for linker (ld)

_start:              ; tells linker entry point
   mov edx, len      ; message length
   mov ecx, msg      ; message to write
   mov ebx, 1        ; file descriptor (stdout)
   mov eax, 4        ; system call number (sys_write)
   int 0x80          ; call kernel

   mov eax, 1        ; system call number (sys_exit)
   int 0x80          ; call kernel

section .data
msg db 'Hello, world!', 0xa  ; string to be printed
len equ $ - msg              ; length of the string
```

When the above code is compiled and executed, it produces the following result:

```
Hello, world!
```

## Compiling and Linking an Assembly Program in NASM

Make sure you have set the path of `nasm` and `ld` binaries in your PATH environment variable. Follow these steps for compiling and linking the above program:

1. Type the above code using a text editor and save it as `hello.asm`.
2. Make sure that you are in the same directory where you saved `hello.asm`.
3. To assemble the program, type:

   ```c
   nasm -f elf hello.asm
   ```

   If there is any error, it will be prompted at this stage. Otherwise, an object file of your program named `hello.o` will be created.

4. To link the object file and create an executable named `hello`, type:

   ```c
   ld -m elf_i386 -s -o hello hello.o
   ```

5. Execute the program by typing:

   ```c
   ./hello
   ```

If everything is done correctly, it will display:

```
Hello, world!
```