# Practical 02 — Arithmetic Operations & Data Movement (8085)

> **Aim:** Perform 8-bit addition, subtraction, and memory data exchange using the 8085 microprocessor.  
> **Apparatus:** PC with GNUSim8085 assembler/simulator *(software simulation — no external circuit diagram)*.

---

## Code

### A1. 8-Bit Addition (Without Carry)

Adds values at `2000H` and `2001H`, storing the sum at `2002H`.

```asm
LDA 2000H       ; Load first 8-bit number from 2000H into Accumulator (A)
MOV B, A        ; Copy first number from A to Register B
LDA 2001H       ; Load second 8-bit number from 2001H into A
ADD B           ; A = A + B
STA 2002H       ; Store sum at memory location 2002H
HLT             ; Halt execution
```

### A2. 8-Bit Addition (With Carry)

Adds values at `2050H` and `2051H`, storing the sum at `2052H` and carry at `2053H`.

```asm
MVI C, 00H      ; Initialize Register C = 00H to track carry
LDA 2050H       ; Load first 8-bit number from 2050H into A
MOV B, A        ; Copy first number to Register B
LDA 2051H       ; Load second 8-bit number from 2051H into A
ADD B           ; A = A + B
JNC STORE       ; If Carry Flag (CY) = 0, jump to STORE
INR C           ; If CY = 1, increment carry register C
STORE: STA 2052H ; Store 8-bit sum at 2052H
MOV A, C        ; Move carry value into A
STA 2053H       ; Store carry at 2053H
HLT             ; Halt execution
```

### B1. 8-Bit Subtraction (Without Borrow)

Subtracts value at `2000H` from value at `2001H`, storing the difference at `2002H`.

```asm
LDA 2000H       ; Load subtrahend from 2000H into A
MOV B, A        ; Copy subtrahend to Register B
LDA 2001H       ; Load minuend from 2001H into A
SUB B           ; A = A - B
STA 2002H       ; Store difference at memory location 2002H
HLT             ; Halt execution
```

### B2. 8-Bit Subtraction (With Borrow)

Subtracts value at `2050H` from value at `2051H`, storing the difference at `2052H` and borrow at `2053H`.

```asm
MVI C, 00H      ; Initialize Register C = 00H to track borrow
LDA 2050H       ; Load subtrahend from 2050H into A
MOV B, A        ; Copy subtrahend to Register B
LDA 2051H       ; Load minuend from 2051H into A
SUB B           ; A = A - B (sets CY = 1 if borrow occurs)
JNC STORE       ; If no borrow (CY = 0), jump to STORE
INR C           ; If borrow occurred (CY = 1), increment C
STORE: STA 2052H ; Store difference at 2052H
MOV A, C        ; Move borrow flag into A
STA 2053H       ; Store borrow at 2053H
HLT             ; Halt execution
```

### C. Movement / Exchange of Data

Exchanges the contents of memory locations `1000H` and `2000H`.

```asm
LDA 1000H       ; Load data from 1000H into A
MOV B, A        ; Save (1000H) temporarily in Register B
LDA 2000H       ; Load data from 2000H into A
STA 1000H       ; Write (2000H) into 1000H
MOV A, B        ; Retrieve original (1000H) from B into A
STA 2000H       ; Write original (1000H) into 2000H
HLT             ; Halt execution
```

---

## Write-Up & Sample Results

### How It Works
All 8085 arithmetic operations route through the 8-bit **Accumulator (`A`)** and ALU, updating status flags such as the **Carry Flag (`CY`)** on overflow (`ADD`) or borrow (`SUB`). Registers `B` and `C` act as temporary operands/carry counters, while `LDA` and `STA` transfer bytes directly between `A` and 16-bit memory addresses.

### Sample Test Cases

| Operation | Inputs | Expected Output |
| :--- | :--- | :--- |
| Addition (no carry) | `(2000H) = 80H`, `(2001H) = 15H` | `(2002H) = 95H` |
| Addition (with carry) | `(2050H) = C2H`, `(2051H) = 4FH` | `(2052H) = 11H`, `(2053H) = 01H` |
| Subtraction (no borrow) | `(2000H) = 15H`, `(2001H) = 80H` | `(2002H) = 6BH` |
| Subtraction (with borrow) | `(2050H) = 45H`, `(2051H) = 15H` | `(2052H) = D0H` *(two's complement of `-30H`)*, `(2053H) = 01H` |
| Data Exchange | `(1000H) = 15H`, `(2000H) = 45H` | `(1000H) = 45H`, `(2000H) = 15H` |
