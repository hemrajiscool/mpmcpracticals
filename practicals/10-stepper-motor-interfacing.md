# Practical 10 — Stepper Motor Interfacing with 8051

## Code

```asm
ORG 0000H
UP:     MOV P2, #09H
        ACALL DELAY
        MOV P2, #0CH
        ACALL DELAY
        MOV P2, #06H
        ACALL DELAY
        MOV P2, #03H
        ACALL DELAY
        SJMP UP
DELAY:  MOV R4, #0FFH
        MOV R3, #0FFH
H1:     DJNZ R3, H1
H2:     DJNZ R4, H2
        RET
END
```

---

## Circuit Diagram

### Proteus Simulation Circuit

![Stepper Motor Proteus Circuit](../assets/p10-stepper-proteus.png)

### Unipolar Stepper Driver Reference Schematic

![Stepper Motor Reference Schematic](../assets/p10-stepper-schematic.jpg)

*(Note: The reference schematic shows `ULN2003` on `P1.0–P1.3`; the Proteus circuit and program above use `ULN2803` on **`P2.0–P2.3`**.)*

---

## Write-Up

**Aim:** Perform stepper motor interfacing using 8051 microcontroller.

**Apparatus:** Keil uVision IDE and Proteus.

### Theory
A 4-phase unipolar stepper motor rotates in discrete steps via a Darlington driver IC (`ULN2803`/`ULN2003`) supplying coil current (~400–500 mA) and internal back-EMF diode protection.

### Steps
1. Set program origin at `0000H`.
2. Output excitation hex code `09H` to Port 2 (`P2.0–P2.3`) and call `DELAY`.
3. Output `0CH`, call `DELAY`; output `06H`, call `DELAY`; output `03H`, call `DELAY`.
4. Loop back to `UP` (`SJMP UP`) to rotate continuously clockwise (reverse sequence `03H → 06H → 0CH → 09H` for anti-clockwise).

### Conclusion
Unipolar stepper motor interfacing with the 8051 microcontroller via `ULN2803` was implemented and continuous step rotation was verified in Keil uVision and Proteus.

---

## Software Operation Guide (Keil uVision & Proteus)

### 1. Keil uVision Setup (Generate `.hex`)
1. Create a folder `C:\MPMC\P10_Stepper\`.
2. Open **Keil uVision** → **Project → New uVision Project** → save as `stepmoto.uvproj` inside `C:\MPMC\P10_Stepper\`.
3. Select **Atmel / Microchip → `AT89C51`** → **OK** → click **No** for `STARTUP.A51`.
4. Click **File → New**, paste the code above, and save as `stepmoto.asm` in the same folder.
5. Expand **Target 1** → right-click **Source Group 1 → Add Existing Files to Group 'Source Group 1'** → set **Files of type** to **`Asm Source file (*.s*; *.src; *.a*)`** → add `stepmoto.asm` → **Close**.
6. Right-click **Target 1 → Options for Target 'Target 1' → Output** tab → tick **Create HEX File** → **OK**.
7. Press **`F7`** (Build) → confirm `0 Error(s), 0 Warning(s)` and `Objects\stepmoto.hex` created.
8. *(Optional Keil check)* Press **`Ctrl+F5`** → open **Peripherals → I/O-Ports → Port 2** → single-step (`F11`) or run (`F5`) to see `P2` cycle through `0x09 → 0x0C → 0x06 → 0x03`.

### 2. Proteus 8 Professional Setup (Components & Wiring)
1. Create a new Proteus schematic project named `Steppermotor`.
2. Pick components using these exact sections and keywords:

   | Search Section | Exact Keyword | Value / Notes |
   | :--- | :--- | :--- |
   | **Component Mode (`P`)** | `AT89C51` *(or `80C51`)* | Microcontroller (`U1`) |
   | **Component Mode (`P`)** | `ULN2803` | 8-channel Darlington driver (`U2`) |
   | **Component Mode (`P`)** | `MOTOR-STEPPER` | 6-wire unipolar stepper motor |
   | **Terminals Mode** *(left toolbar)* | `POWER` *(or `DEFAULT`)* | Supply rail connected to `ULN2803` `COM` (Pin 10) and stepper center taps |

3. Wire the circuit:
   - `AT89C51` `P2.0–P2.3` (Pins 21–24) → `ULN2803` inputs `1B–4B` (Pins 1–4).
   - `ULN2803` outputs `1C–4C` (Pins 18–15) → the 4 outer winding pins of `MOTOR-STEPPER`.
   - Connect the 2 middle center-tap pins of `MOTOR-STEPPER` together to `ULN2803` `COM` (Pin 10) and power (`POWER` / `U2(COM)`).

### 3. Linking Keil & Proteus
1. Double-click **`U1` (`AT89C51`)** → **Program File** → browse to `C:\MPMC\P10_Stepper\Objects\stepmoto.hex` → click **OK**.
2. Click the green **Play** button (bottom-left) and observe the stepper motor shaft stepping smoothly with the live angle readout (`+325`, etc.) updating underneath.
