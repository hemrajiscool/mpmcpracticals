# Practical 02 — Arithmetic Operations & Data Movement (8085)

## Code

### A1. Addition Without Carry (`2000H` + `2001H` → `2002H`)

```asm
LDA 2000H
MOV B, A
LDA 2001H
ADD B
STA 2002H
HLT
```

### A2. Addition With Carry (`2050H` + `2051H` → `2052H`, Carry → `2053H`)

```asm
MVI C, 00H
LDA 2050H
MOV B, A
LDA 2051H
ADD B
JNC STORE
INR C
STORE: STA 2052H
MOV A, C
STA 2053H
HLT
```

### B1. Subtraction Without Borrow (`2001H` - `2000H` → `2002H`)

```asm
LDA 2000H
MOV B, A
LDA 2001H
SUB B
STA 2002H
HLT
```

### B2. Subtraction With Borrow (`2051H` - `2050H` → `2052H`, Borrow → `2053H`)

```asm
MVI C, 00H
LDA 2050H
MOV B, A
LDA 2051H
SUB B
JNC STORE
INR C
STORE: STA 2052H
MOV A, C
STA 2053H
HLT
```

### C. Movement / Exchange of Data (`1000H` ↔ `2000H`)

```asm
LDA 1000H
MOV B, A
LDA 2000H
STA 1000H
MOV A, B
STA 2000H
HLT
```

---

## Write-Up

**Aim:** To perform arithmetic operations like addition, subtraction, and movement of data using the 8085 microprocessor.

**Apparatus:** PC and GNUSim8085 Microprocessor Simulator.

### Theory
The 8085 ALU performs 8-bit arithmetic using the Accumulator (`A`) and registers (`B`, `C`), updating the Carry flag (`CY`) on overflow or borrow while transferring data via `LDA` and `STA`.

### Steps
1. Load the first 8-bit operand from memory into Accumulator `A` and copy it to register `B` (initialize `C = 00H` if tracking carry/borrow).
2. Load the second 8-bit operand from memory into `A`.
3. Execute `ADD B` or `SUB B`; if carry/borrow is checked, branch via `JNC STORE` and increment `C` when `CY = 1`.
4. Store the result from `A` (and carry/borrow from `C`) into destination memory locations, then halt with `HLT`.
5. For data exchange, buffer `(1000H)` in `B`, copy `(2000H)` to `1000H`, then store `B` into `2000H`.

### Sample Inputs & Expected Results

| Program | Input Memory (Hex / Dec) | Expected Output (Hex / Dec) |
| :--- | :--- | :--- |
| Add (no carry) | `2000H = 80H` (`128`), `2001H = 15H` (`21`) | `2002H = 95H` (`149`) |
| Add (with carry) | `2050H = C2H` (`194`), `2051H = 4FH` (`79`) | `2052H = 11H` (`17`), `2053H = 01H` (`1`) |
| Sub (no borrow) | `2000H = 15H` (`21`), `2001H = 80H` (`128`) | `2002H = 6BH` (`107`) |
| Sub (with borrow) | `2050H = 45H` (`69`), `2051H = 15H` (`21`) | `2052H = D0H` (`208`), `2053H = 01H` (`1`) |
| Data Exchange | `1000H = 15H` (`21`), `2000H = 45H` (`69`) | `1000H = 45H` (`69`), `2000H = 15H` (`21`) |

### Conclusion
Addition, subtraction (with and without carry/borrow), and memory data exchange were executed and verified on GNUSim8085.

---

## Software Operation Guide (GNUSim8085)

1. **Create & Save File:**
   - Open **GNUSim8085** and click **File → New**.
   - Paste one program from the Code section above and save via **File → Save** (e.g., `C:\MPMC\P02_8085\add.asm`).
2. **Load Input Data into Memory:**
   - GNUSim8085 accepts memory addresses and data in **decimal**. Use the **Decimal - Hex Conversion** box on the bottom-left to convert hex to decimal (`2000H = 8192`, `2050H = 8272`, `1000H = 4096`).
   - Open the **Memory** tab on the right panel, enter the decimal start address (e.g., `8192`) in **Start**, and click **OK**.
   - In the bottom-left **Memory** box, enter the decimal address and decimal byte value, then click **Update Memory** for each input address.
3. **Assemble & Run:**
   - Click **Assembler → Assemble** (`F8`) and confirm `Program assembled successfully` in the bottom-right message panel.
   - Click **Debug → Execute** (`F9`).
4. **Verify Output:**
   - Inspect **Registers** (`A`, `BC`) and **Flag** (`C` for carry/borrow) on the left panel, and read the stored result in the **Memory** tab on the right.
   - Before running the next program, click **Reset → Reset All** (`Ctrl+R`).
