# Power Budget Calculation ⚡

Below is the power budget analysis for our system, detailing voltage, current, and power consumption for each component.

## Component Power Breakdown

| **Component**                           | **Voltage (V)**     | **Current (A)**        | **Power (W) = V × A** |
|-----------------------------------------|---------------------|------------------------|-----------------------|
| SEN0229 Motor                           | 3.3V                | 0.5A                   | 1.65W                 |
| PIC18F47Q10                             | 3.3V                | 0.04A                  | 0.132W                |
| AP63203WU-7 (Buck Reg.)                 | 12V input → 3.3V output | 0.3A*             | 1.188W                |
| Motor Driver IFX9201SGAUMA1             | 3.3V                | 0.1A                   | 0.33W                 |
| LED Debug (2x LEDs)                     | 3.3V                | 0.02A (each)           | 0.132W                |
| 10µF Capacitor (C1)                     | Passive             | Negligible             | 0W                    |
| 10µF 50V Capacitor (C2)                 | Passive             | Negligible             | 0W                    |
| 0.1µF 25V Capacitor (C3)                | Passive             | Negligible             | 0W                    |
| 2×22µF Capacitor (C4)                   | Passive             | Negligible             | 0W                    |
| Inductor (3.9µH, L1)                    | Passive             | Negligible             | 0W                    |
| Resistor 10kΩ (R1, R2)                  | 3.3V                | 0.33mA (each)          | 0.002W                |
| Resistor 220kΩ (R3, R4)                 | 3.3V                | 0.015mA (each)         | 0.00005W              |
| Fuse Block (02981001ZXT)                | 32V Max             | Passive                | 0W (unless tripped)   |
| **Through Hole Barrel Jack**            | 9V Input           | -                      | 0W (Power Input)      |
| Jumpers (J1, J2)                        | -                   | Passive                | 0W                    |
| DOWNSTREAM Connector (P1)               | -                   | -                      | 0W                    |
| UPSTREAM Connector (P2)                 | -                   | -                      | 0W                    |

## Total Power Consumption

### At 3.3V Load:
- **Total Current Draw**: ~0.68A  
- **Total Power (3.3V Side)**: ~2.24W

- 


