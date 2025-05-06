

# ⚡ Power Budget Analysis (Detailed)

This section presents the final power budget evaluation for the **Motor Driver Subsystem**, reflecting the actual hardware implementation. The purpose of this analysis is to ensure all components operate safely within power limits, without thermal overload or voltage collapse, while supporting subsystem reliability and expandability. All data is derived from **official datasheets**, **measured lab current draw**, and **worst-case electrical estimates**, taking into account the system's operating environment and shared power delivery from the upstream daisy-chain.

---

## 🔋 Component-Level Power Breakdown

The following table summarizes the power consumption characteristics for each major and minor component, along with operating voltages and calculated power draw:

| **Component**                    | **Voltage (V)** | **Current (A)**    | **Power (W)** = V × A   |
|----------------------------------|------------------|---------------------|--------------------------|
| SEN0229 Water Motor (Load)       | 3.3V             | 0.20 A              | 0.66 W                   |
| PIC18F47Q10 MCU                  | 3.3V             | 0.25 A              | 0.825 W                  |
| IFX9201SGAUMA1 Motor Driver      | 3.3V             | 0.10 A              | 0.33 W                   |
| Debug LEDs (2x 20mA)             | 3.3V             | 0.04 A              | 0.132 W                  |
| AP63203WU-7 Buck Regulator       | 12V → 3.3V       | ~0.30 A (Load)      | ~3.06 W (Input)          |
| 10kΩ Resistors (x2)              | 3.3V             | 0.00066 A (total)   | 0.002 W                  |
| 220kΩ Resistors (x2)             | 3.3V             | 0.00003 A (total)   | 0.00005 W                |
| Capacitors (All)                 | —                | Negligible          | 0 W                      |
| Inductor (Buck)                  | —                | Passive             | 0 W                      |
| Resettable Fuse                  | —                | Passive             | 0 W (unless tripped)     |
| Barrel Jack Input Connector      | 9V               | —                   | 0 W                      |
| Upstream/Downstream Headers      | —                | —                   | 0 W                      |

---

### 🔢 Final Load Calculation

- **Total Current Draw at 3.3V** ≈ **0.68 A**
- **Total Power Consumption at 3.3V** ≈ **1.98 W**
- **Estimated Input Power from 12V side** ≈ **3.06 W**, accounting for regulator efficiency (~88–90%).

---

## 📊 How the Power Budget Informed Design Decisions

### 1. **Voltage Rail Selection**
We standardized all logic and control hardware to operate on a **3.3V rail**. This removed the need for multiple regulators, minimized component incompatibilities, and allowed uniform signal thresholds across MCU, motor driver, and peripherals.

### 2. **Power Regulation Strategy**
A critical aspect of power planning was regulator choice. We selected the **AP63203WU-7**, a compact, synchronous buck converter with a 2A current rating and integrated protection features.

### 3. **Motor Load Evaluation**
The SEN0229 water turbine was tested to draw up to **200mA** in steady-state operation with 3.3V applied via the IFX9201. The **motor driver itself adds another 100mA**, yielding a worst-case actuation current near **300mA**.

### 4. **Protection Circuitry**
To ensure fault tolerance, we added a **resettable fuse** at the regulator input rated slightly above expected input current (~1A hold).

### 5. **Peripheral Consumption**
Two debugging LEDs and several passive components were included in the power budget. While their draw is negligible, their inclusion reinforces the **completeness** of the design.

---

## 🧰 Broader Design Insights from the Power Budget

This power budget was not a standalone checklist, but a **driving force** behind hardware decisions:

- **MCU Current Planning**
- **Heat Management**
- **Trace Sizing and PCB Routing**
- **Safe Expandability**

---

## 📈 Conclusion: Using the Power Budget for Reliability and Expansion

By carefully constructing this power budget, we achieved three critical goals:

1. **Operational Stability**
2. **Thermal Safety**
3. **Design Scalability**

This attention to power analysis ensures that the **Motor Driver Subsystem functions safely, efficiently, and predictably** under all expected conditions.

## Power Budget Link for Excel Sheet

[Power Budget ](https://docs.google.com/spreadsheets/d/156U78wOTZNQjrQSCz7AoKw_OFbUZ7FwH/edit?usp=sharing&ouid=107614409505094169035&rtpof=true&sd=true)



