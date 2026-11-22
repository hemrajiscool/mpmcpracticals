# Practical 06 — LED Blinking with 8051

> **Aim:** Interface an LED with the 8051 microcontroller and blink it ON and OFF at regular intervals using an assembly language program.  
> **Apparatus:** Keil uVision IDE and Proteus.

---

## Code

```asm
ORG 0000H
UP:     SETB P2.0           ; Turn LED ON (P2.0 = 1)
        ACALL DELAY         ; Hold ON state
        CLR P2.0            ; Turn LED OFF (P2.0 = 0)
        ACALL DELAY         ; Hold OFF state
        SJMP UP             ; Repeat indefinitely

DELAY:  MOV R4, #35         ; Nested software delay loop
H1:     MOV R3, #255
H2:     DJNZ R3, H2
        DJNZ R4, H1
        RET
END
```

---

## Circuit Diagram

### Proteus Simulation Circuit

![LED Blinking Proteus Circuit](../assets/p06-led-blink-proteus.png)

---

## Write-Up & Components

### How It Works
The program alternates bit **`P2.0`** between logic `1` (`SETB P2.0`) and logic `0` (`CLR P2.0`) inside an infinite loop. Between each transition, `ACALL DELAY` executes a nested `DJNZ` countdown (`35 × 255` iterations) so the blinking interval is visible.

### Key Components & Connections

| Component | Proteus Name / Value | Role & Connection |
| :--- | :--- | :--- |
| Microcontroller | `80C51` (`AT89C51`) | Drives digital output on `P2.0` (Pin 21) |
| Indicator LED (`D1`) | `LED-RED` | Anode (`+`) to `P2.0` (Pin 21), Cathode (`-`) to `GND` |
| Current-Limiting Resistor *(real hardware)* | `220 Ω – 330 Ω` | Connected in series with the LED on physical circuits to prevent overcurrent damage |
