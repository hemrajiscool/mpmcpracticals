# Practical 03 — Arithmetic Operations (8051)

> **Aim:** Perform 8-bit addition, subtraction, multiplication, and division using the 8051 microcontroller instruction set.  
> **Apparatus:** PC with Keil uVision / 8051 assembler *(software simulation — no external circuit diagram)*.

---

## Code

### A. 8-Bit Addition

Adds immediate values `20H` and `30H` and stores the sum in internal RAM `10H`.

```asm
ORG 0000H           ; Set program origin to 0000H
MOV A, #20H         ; Load first 8-bit immediate value (20H) into A
ADD A, #30H         ; Add second immediate value (30H) to A
MOV 10H, A          ; Store sum at internal RAM location 10H
HERE: SJMP HERE     ; Halt in an infinite loop
END
```

### B. 8-Bit Subtraction

Subtracts `20H` from `30H` and stores the difference in internal RAM `10H`.

```asm
ORG 0000H           ; Set program origin to 0000H
MOV A, #30H         ; Load minuend (30H) into A
CLR C               ; Clear Carry/Borrow flag before subtraction
SUBB A, #20H        ; Subtract 20H (and CY) from A
MOV 10H, A          ; Store difference at internal RAM location 10H
HERE: SJMP HERE     ; Halt in an infinite loop
END
```

### C. 8-Bit Multiplication

Multiplies `20H` and `30H`; stores low byte at `10H` and high byte at `11H`.

```asm
ORG 0000H           ; Set program origin to 0000H
MOV A, #20H         ; Load first 8-bit operand (20H) into A
MOV B, #30H         ; Load second 8-bit operand (30H) into B
MUL AB              ; A * B -> Low byte in A, High byte in B
MOV 10H, A          ; Store low byte of product at 10H
MOV 11H, B          ; Store high byte of product at 11H
HERE: SJMP HERE     ; Halt in an infinite loop
END
```

### D. 8-Bit Division

Divides `45H` by `20H`; stores quotient at `10H` and remainder at `11H`.

```asm
ORG 0000H           ; Set program origin to 0000H
MOV A, #45H         ; Load dividend (45H) into A
MOV B, #20H         ; Load divisor (20H) into B
DIV AB              ; A / B -> Quotient in A, Remainder in B
MOV 10H, A          ; Store quotient at 10H
MOV 11H, B          ; Store remainder at 11H
HERE: SJMP HERE     ; Halt in an infinite loop
END
```

---

## Write-Up & Sample Results

### How It Works
Unlike the 8085, the 8051 includes hardware instructions for unsigned 8-bit multiplication (`MUL AB`) and division (`DIV AB`) using the **Accumulator (`A`)** and **Register `B`**:
- `ADD A, #data` adds an immediate byte to `A`.
- `SUBB A, #data` subtracts both the operand and the Carry flag (`C`), so `CLR C` is executed first when no prior borrow exists.
- `MUL AB` produces a 16-bit product (`A` = low byte, `B` = high byte).
- `DIV AB` produces an integer quotient in `A` and remainder in `B`.
- Because 8051 has no `HLT` instruction, `HERE: SJMP HERE` traps execution cleanly.

### Sample Test Cases

| Operation | Inputs | Internal RAM Output |
| :--- | :--- | :--- |
| Addition | `20H + 30H` | `(10H) = 50H` |
| Subtraction | `30H - 20H` | `(10H) = 10H` |
| Multiplication | `20H × 30H = 0600H` | `(10H) = 00H` (low byte), `(11H) = 06H` (high byte) |
| Division | `45H / 20H` | `(10H) = 02H` (quotient), `(11H) = 05H` (remainder) |
