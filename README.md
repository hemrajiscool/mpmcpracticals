# Microprocessors & Microcontrollers Lab (ET307N)

Ready-to-copy **8085 & 8051 Assembly** programs, **Proteus** circuit diagrams, schematics, and concise component write-ups for all 10 lab practicals.

Every practical page is organized for quick lab use:
1. **Copyable Assembly Code** right at the top
2. **Circuit Diagram / Pinout** (Proteus schematic + reference schematic where applicable)
3. **Concise Write-Up** covering how the circuit works and the key components/pins involved

---

## Practicals Index

| No. | Practical | Platform / Tools | Link |
| :-: | :--- | :--- | :--- |
| 01 | Study of 8085 Microprocessor & 8051 Microcontroller | 8085 / 8051 Trainer Kits | [01-study-8085-8051.md](practicals/01-study-8085-8051.md) |
| 02 | Arithmetic Operations & Data Movement (8085) | GNUSim8085 | [02-8085-arithmetic-and-data-movement.md](practicals/02-8085-arithmetic-and-data-movement.md) |
| 03 | Arithmetic Operations (8051) | Keil uVision / 8051 Assembler | [03-8051-arithmetic-operations.md](practicals/03-8051-arithmetic-operations.md) |
| 04 | Switch Interfacing with 8051 | Keil uVision + Proteus | [04-switch-interfacing.md](practicals/04-switch-interfacing.md) |
| 05 | Relay Interfacing with 8051 | Keil uVision + Proteus | [05-relay-interfacing.md](practicals/05-relay-interfacing.md) |
| 06 | LED Blinking with 8051 | Keil uVision + Proteus | [06-led-blinking.md](practicals/06-led-blinking.md) |
| 07 | 16×2 LCD Interfacing with 8051 | Keil uVision + Proteus | [07-lcd-interfacing.md](practicals/07-lcd-interfacing.md) |
| 08 | ADC0804 Interfacing with 8051 | Keil uVision + Proteus | [08-adc-interfacing.md](practicals/08-adc-interfacing.md) |
| 09 | DAC0808 Waveform Generation (Square, Triangular, Sine) | Keil uVision + Proteus | [09-dac-waveform-generation.md](practicals/09-dac-waveform-generation.md) |
| 10 | Stepper Motor Interfacing with 8051 | Keil uVision + Proteus | [10-stepper-motor-interfacing.md](practicals/10-stepper-motor-interfacing.md) |

---

## Running the Keil + Proteus Practicals (04–10)

1. Create a new **Keil uVision** project targeting **AT89C51** (or `80C51`), add an `.asm` file, and paste the code from the practical.
2. Under **Options for Target → Output**, enable **Create HEX File**, then build the project.
3. Build the schematic in **Proteus** matching the diagram shown in the practical, double-click the 8051 IC, load the generated `.hex` file, and run the simulation.
