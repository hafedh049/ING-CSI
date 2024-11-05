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
; OR
INVENTORY DW 8 dup(0)  ; Initializes an array of 8 words to zero
; OR
INVENTORY DW 8 dup(?)  ; Initializes an array of 8 words to random numbers
```

