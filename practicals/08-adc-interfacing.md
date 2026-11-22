# Practical 08 — ADC0804 Interfacing with 8051

> **Aim:** Interface an 8-bit parallel ADC (`ADC0804`) with the 8051 microcontroller to digitize an analog input voltage and display the 8-bit binary output on LEDs.  
> **Apparatus:** Keil uVision IDE and Proteus.

---

## Code

```asm
ORG 0000H
        MOV P1, #11111111B  ; Configure Port 1 as input (ADC D0-D7)
        MOV P2, #00000000B  ; Configure Port 2 as output (8 LEDs)

MAIN:   SETB P3.6           ; RD = 1 (disable ADC output during conversion)
        CLR P3.5            ; WR = 0
        SETB P3.5           ; Low-to-high pulse on WR starts conversion
H1:     JB P3.4, H1         ; Wait until INTR (P3.4) goes LOW (end of conversion)
        CLR P3.6            ; RD = 0 (enable ADC data output onto Port 1)
        ACALL DELAY
        MOV A, P1           ; Read 8-bit digital value from ADC
        MOV P2, A           ; Display binary result on Port 2 LEDs
        ACALL DELAY
        SETB P3.4           ; Reset P3.4 high for next polling cycle
        SJMP MAIN           ; Repeat continuously

ORG 0210H
DELAY:  MOV R0, #9FH
        MOV R1, #9FH
K:      DJNZ R1, K
K1:     DJNZ R0, K1
        RET
END
```

---

## Circuit Diagram

### Proteus Simulation Circuit

![ADC0804 Interfacing Proteus Circuit](../assets/p08-adc-proteus.png)

---

## Write-Up & Components

### How It Works
**`ADC0804`** is an 8-bit successive-approximation parallel ADC covering `0–5 V` when `VREF/2` (Pin 9) is left open (step size = `5 V / 255 ≈ 19.6 mV`).
1. **Start Conversion:** With `CS̄` tied low, pulsing `WR̄` (`P3.5`) low-to-high triggers a new conversion.
2. **End of Conversion (EOC):** The 8051 polls `INTR̄` (`P3.4`); `ADC0804` pulls `INTR̄` low once conversion finishes.
3. **Read & Display:** Pulling `RD̄` (`P3.6`) low places the 8-bit digital word on `DB0–DB7`, which the 8051 reads on **Port 1** and copies to **Port 2** (`D1–D8`). Adjusting the `1 kΩ` potentiometer (`RV2`) changes the analog voltage at `VIN+` (e.g., `2.5 V ≈ 80H` lights MSB LED `D8`).

### Key Components & Pin Connections

| Component | Proteus Name / Value | Role & Connections |
| :--- | :--- | :--- |
| Microcontroller (`U1`) | `AT89C51` | `P1.0–P1.7` ← `DB0–DB7`; `P2.0–P2.7` → LEDs `D1–D8`; `P3.4` ← `INTR̄`, `P3.5` → `WR̄`, `P3.6` → `RD̄` |
| 8-Bit ADC (`U2`) | `ADC0804` | `VCC` → +5 V; `CS̄`, `A GND`, `D GND`, `VIN-` → GND |
| RC Clock Network (`R1`, `C1`) | `10 kΩ`, `150 pF` | Connected to `CLK R` (Pin 19) & `CLK IN` (Pin 4) for the internal clock generator |
| Analog Input (`RV2`) | `POT-HG` (`1 kΩ`) | Supplies variable `0–5 V` analog voltage to `VIN+` (Pin 6) |
| Output LEDs (`D1–D8`) | `LED-YELLOW` (×8) | Show the 8-bit binary reading on `P2.0` (LSB) through `P2.7` (MSB) |
