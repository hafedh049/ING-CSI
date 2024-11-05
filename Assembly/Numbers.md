
Such conversions, however, have overhead, and assembly language programming allows processing numbers in a more efficient way in binary form. Decimal numbers can be represented in two forms:

1. **ASCII form**
2. **BCD (Binary Coded Decimal) form**

### ASCII Representation

In ASCII representation, decimal numbers are stored as a string of ASCII characters. For example, the decimal value 1234 is stored as:

```c
31 32 33 34H
```

Where 31H is the ASCII value for 1, 32H is the ASCII value for 2, and so on. There are four instructions for processing numbers in ASCII representation:

- `AAA` — ASCII Adjust After Addition
- `AAS` — ASCII Adjust After Subtraction
- `AAM` — ASCII Adjust After Multiplication
- `AAD` — ASCII Adjust Before Division

These instructions do not take any operands and assume the required operand to be in the `AL` register.
### BCD Representation

There are two types of BCD representation:

1. **Unpacked BCD representation**
2. **Packed BCD representation**

In unpacked BCD representation, each byte stores the binary equivalent of a decimal digit. For example, the number 1234 is stored as:

```c
01 02 03 04H
```

There are two instructions for processing these numbers:

- `AAM` — ASCII Adjust After Multiplication
- `AAD` — ASCII Adjust Before Division

The four ASCII adjust instructions, `AAA`, `AAS`, `AAM`, and `AAD`, can also be used with unpacked BCD representation. In packed BCD representation, each digit is stored using four bits. Two decimal digits are packed into a byte. For example, the number 1234 is stored as:

```c
12 34H
```

There are two instructions for processing these numbers:

- `DAA` — Decimal Adjust After Addition
- `DAS` — Decimal Adjust After Subtraction

There is no support for multiplication and division in packed BCD representation.