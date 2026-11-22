# Practical 03 — Arithmetic Operations (8051)

## Code

### A. Addition (`20H + 30H` → `10H`)

```asm
ORG 0000H
MOV A, #20H
ADD A, #30H
MOV 10H, A
HERE: SJMP HERE
END
```

### B. Subtraction (`30H - 20H` → `10H`)

```asm
ORG 0000H
MOV A, #30H
CLR C
SUBB A, #20H
MOV 10H, A
HERE: SJMP HERE
END
```

### C. Multiplication (`20H × 30H` → Low at `10H`, High at `11H`)

```asm
ORG 0000H
MOV A, #20H
MOV B, #30H
MUL AB
MOV 10H, A
MOV 11H, B
HERE: SJMP HERE
END
```

### D. Division (`45H ÷ 20H` → Quotient at `10H`, Remainder at `11H`)

```asm
ORG 0000H
MOV A, #45H
MOV B, #20H
DIV AB
MOV 10H, A
MOV 11H, B
HERE: SJMP HERE
END
```

---

## Write-Up

**Aim:** To perform arithmetic operations like addition, subtraction, multiplication, and division using the 8051 microcontroller.

**Apparatus:** PC and Keil uVision IDE (8051 Assembler).

### Theory
The 8051 executes 8-bit arithmetic via Accumulator `A` and Register `B` using `ADD`, `SUBB`, `MUL AB` (16-bit product), and `DIV AB` (quotient and remainder), storing results in internal RAM.

### Steps
1. Set program origin to `0000H` using `ORG 0000H`.
2. Load immediate operands into `A` (and `B` for `MUL AB` / `DIV AB`); clear carry (`CLR C`) before `SUBB`.
3. Execute the arithmetic instruction (`ADD`, `SUBB`, `MUL AB`, or `DIV AB`).
4. Store the result from `A` into internal RAM `10H` (and high byte/remainder from `B` into `11H`).
5. Halt execution with `HERE: SJMP HERE` and terminate with `END`.

### Expected Results

| Operation | Calculation | Internal RAM Output |
| :--- | :--- | :--- |
| Addition | `20H + 30H = 50H` | `I:0x10 = 50H` |
| Subtraction | `30H - 20H = 10H` | `I:0x10 = 10H` |
| Multiplication | `20H × 30H = 0600H` | `I:0x10 = 00H` (low), `I:0x11 = 06H` (high) |
| Division | `45H ÷ 20H` | `I:0x10 = 02H` (quotient), `I:0x11 = 05H` (remainder) |

### Conclusion
8-bit addition, subtraction, multiplication, and division programs were assembled and verified in Keil uVision for the 8051 microcontroller.

---

## Software Operation Guide (Keil uVision)

1. **Create Folder & Project:**
   - Create a folder, e.g., `C:\MPMC\P03_Arithmetic\`.
   - Open **Keil uVision** → **Project → New uVision Project** → save inside that folder as `add.uvproj` (or `sub`, `mul`, `div`).
   - In **Select Device for Target**, choose **Atmel / Microchip → `AT89C51`** → click **OK**.
   - When prompted *Copy STARTUP.A51 to Project Folder?*, click **No**.
2. **Add Assembly Source File:**
   - Click **File → New**, paste the program, and press **Ctrl+S** to save as `add.asm` inside `C:\MPMC\P03_Arithmetic\`.
   - In the left **Project** pane, expand **Target 1**, right-click **Source Group 1 → Add Existing Files to Group 'Source Group 1'**.
   - Change **Files of type** dropdown to **`Asm Source file (*.s*; *.src; *.a*)`**, select `add.asm`, click **Add**, then **Close**.
3. **Build & Simulate:**
   - Click **Project → Build Target** (`F7`) and verify `0 Error(s), 0 Warning(s)` in **Build Output**.
   - Click **Debug → Start/Stop Debug Session** (`Ctrl+F5`) → click **OK** on the evaluation-mode popup.
   - Click **Debug → Run** (`F5`) briefly, then **Debug → Stop** (`Esc`).
4. **Verify Registers & Internal RAM:**
   - Check `a`, `b`, and `psw` in the left **Registers** pane.
   - Open **View → Memory Windows → Memory 1**, type **`I:0x00`** in the **Address** bar, and press **Enter** to inspect internal RAM bytes `0x10` and `0x11`.
