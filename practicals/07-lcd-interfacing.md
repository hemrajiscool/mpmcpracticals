# Practical 07 — 16×2 LCD Interfacing with 8051

## Code

```asm
ORG 0000H
RS BIT P2.0
RW BIT P2.1
EN BIT P2.2

        MOV A, #38H
        ACALL CMD
        MOV A, #0EH
        ACALL CMD
        MOV A, #01H
        ACALL CMD
        MOV A, #06H
        ACALL CMD
        MOV A, #80H
        ACALL CMD

        MOV A, #'W'
        ACALL DATA1
        MOV A, #'E'
        ACALL DATA1
        MOV A, #'L'
        ACALL DATA1
        MOV A, #'C'
        ACALL DATA1
        MOV A, #'O'
        ACALL DATA1
        MOV A, #'M'
        ACALL DATA1
        MOV A, #'E'
        ACALL DATA1

        MOV A, #0C0H
        ACALL CMD
        MOV A, #'T'
        ACALL DATA1
        MOV A, #'O'
        ACALL DATA1
        MOV A, #' '
        ACALL DATA1
        MOV A, #'G'
        ACALL DATA1
        MOV A, #'C'
        ACALL DATA1
        MOV A, #'O'
        ACALL DATA1
        MOV A, #'E'
        ACALL DATA1
        MOV A, #'J'
        ACALL DATA1

HERE:   SJMP HERE

CMD:    ACALL READY
        MOV P1, A
        CLR RS
        CLR RW
        SETB EN
        ACALL DELAY
        CLR EN
        RET

READY:  SETB P1.7
        CLR RS
        SETB RW
H:      CLR EN
        ACALL DELAY
        SETB EN
        JB P1.7, H
        RET

DATA1:  ACALL READY
        MOV P1, A
        SETB RS
        CLR RW
        SETB EN
        ACALL DELAY
        CLR EN
        RET

DELAY:  MOV R4, #180
H1:     MOV R3, #255
H2:     DJNZ R3, H2
        DJNZ R4, H1
        RET
END
```

---

## Circuit Diagram

### Proteus Simulation Circuit

![16x2 LCD Interfacing Proteus Circuit](../assets/p07-lcd-proteus.png)

### 16×2 LCD Pinout Reference

![16x2 LCD Pinout](../assets/p07-lcd-pinout.png)

---

## Write-Up

**Aim:** Interface LCD with 8051 microcontroller.

**Apparatus:** Keil uVision IDE and Proteus.

### Theory
A 16×2 LCD displays ASCII characters via 8 data pins (`D0–D7`) and 3 control pins: `RS` (command/data), `RW` (read/write), and `EN` (enable latch pulse).

### Steps
1. Map control bits `RS = P2.0`, `RW = P2.1`, and `EN = P2.2`, and connect `P1.0–P1.7` to `D0–D7`.
2. Send initialization commands via `CMD`: `38H` (8-bit, 2 lines), `0EH` (display/cursor ON), `01H` (clear), `06H` (cursor auto-increment), and `80H` (Line 1 start).
3. Send ASCII characters `"WELCOME"` one by one via `DATA1`.
4. Send command `0C0H` to jump to Line 2, then send `"TO GCOEJ"` via `DATA1`.
5. Each `CMD` / `DATA1` call checks the busy flag (`P1.7`) via `READY` and pulses `EN` high-to-low before halting at `HERE: SJMP HERE`.

### Conclusion
A 16×2 LCD was interfaced with the 8051 microcontroller in 8-bit mode and displayed `"WELCOME"` / `"TO GCOEJ"` in Proteus.

---

## Software Operation Guide (Keil uVision & Proteus)

### 1. Keil uVision Setup (Generate `.hex`)
1. Create a folder `C:\MPMC\P07_LCD\`.
2. Open **Keil uVision** → **Project → New uVision Project** → save as `Lcdinterfacing.uvproj` inside `C:\MPMC\P07_LCD\`.
3. Select **Atmel / Microchip → `AT89C51`** → **OK** → click **No** for `STARTUP.A51`.
4. Click **File → New**, paste the code above, and save as `Lcdinterfacing.asm` in the same folder.
5. Expand **Target 1** → right-click **Source Group 1 → Add Existing Files to Group 'Source Group 1'** → set **Files of type** to **`Asm Source file (*.s*; *.src; *.a*)`** → add `Lcdinterfacing.asm` → **Close**.
6. Right-click **Target 1 → Options for Target 'Target 1' → Output** tab → tick **Create HEX File** → **OK**.
7. Press **`F7`** (Build) → confirm `0 Error(s), 0 Warning(s)` and `Objects\Lcdinterfacing.hex` created.

### 2. Proteus 8 Professional Setup (Components & Wiring)
1. Create a new Proteus schematic project named `LCDInterface`.
2. Pick components using these exact sections and keywords:

   | Search Section | Exact Keyword | Value / Notes |
   | :--- | :--- | :--- |
   | **Component Mode (`P`)** | `AT89C51` | Microcontroller (`U1`) |
   | **Component Mode (`P`)** | `LM016L` | 16×2 Alphanumeric LCD (`LCD1`) |
   | **Component Mode (`P`)** | `POT` | Contrast potentiometer (`RV1`, double-click → set `1k`) |
   | **Terminals Mode** *(left toolbar)* | `POWER` | +5 V to `LCD1` `VDD` (Pin 2) and top of `RV1` |
   | **Terminals Mode** *(left toolbar)* | `GROUND` | Ground to `LCD1` `VSS` (Pin 1) and bottom of `RV1` |

3. Wire the circuit:
   - `AT89C51` `P1.0–P1.7` (Pins 1–8) → `LM016L` `D0–D7` (Pins 7–14).
   - `P2.0` (Pin 21) → `RS` (Pin 4); `P2.1` (Pin 22) → `RW` (Pin 5); `P2.2` (Pin 23) → `E` (Pin 6).
   - `RV1` wiper → `VEE` (Pin 3).

### 3. Linking Keil & Proteus
1. Double-click **`U1` (`AT89C51`)** → **Program File** → browse to `C:\MPMC\P07_LCD\Objects\Lcdinterfacing.hex` → click **OK**.
2. Click the green **Play** button (bottom-left) and verify `WELCOME` on Line 1 and `TO GCOEJ` on Line 2 of `LM016L`.
