# Major Component Selection & Comparison

## 1. Motor Driver

The motor driver is a critical component for controlling the direction and speed of the motor. Since my design requires bidirectional control of a motor, I needed a motor driver with high efficiency, integrated protection features, and compatibility with my microcontroller. I considered multiple options before finalizing my choice, ensuring that the selected component met my design criteria.

### Component Options

| Component      | Description                                 | Pros                                                                                                                                                                                                                  | Cons                                                                                  | Cost   | Link                         |
|----------------|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|--------|------------------------------|
| IFX9201SGAUMA1 | Intelligent motor driver IC                 | - High efficiency, integrated protections, supports bidirectional motor control<br>- Designed for automotive and industrial applications, ensuring durability<br>- Compact design with minimal external components | - Higher cost than simpler motor drivers<br>- Requires careful PCB layout for optimal performance | $4.50  | [IFX9201 Datasheet](#)       |
| DRV8835       | Dual H-Bridge motor driver, supports low voltage | - Compact, efficient, supports PWM<br>- Simple implementation with minimal external circuitry                                                                                                                          | - Lower current limit than other options, limiting motor selection<br>- May require external protection components | $2.50  | [DRV8835 Datasheet](#)       |
| TB6612FNG     | Motor driver with built-in MOSFETs          | - High efficiency, good for small motors, easy to use<br>- Can drive two motors independently                                                                                                                         | - Slightly larger footprint<br>- Lower power handling capability compared to IFX9201   | $3.00  | [TB6612FNG Datasheet](#)     |

**Chosen Component:** **IFX9201SGAUMA1**

**Rationale:**  
I selected the IFX9201SGAUMA1 motor driver because it offers robust protection features such as overcurrent and thermal protection, making it reliable for my design. Additionally, it supports bidirectional control, which is crucial for my use of a water generator turbine as a reversible motor. Its high efficiency and integrated fault detection make it an optimal choice despite its slightly higher cost.

---

## 2. Voltage Regulator  

The voltage regulator ensures that my microcontroller and other components receive a stable 3.3V power supply. Given my system’s 9V-12V input voltage, I evaluated both linear and switching regulators to determine the best balance between efficiency and simplicity.  

### Component Options  

| Component       | Description                              | Pros                                                                                          | Cons                                                                 | Cost  | Link                           |  
|---------------|------------------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------|------|--------------------------------|  
| AP63203WU-7  | Switching regulator, 9V-12V to 3.3V      | - High efficiency (~88-90%), minimal heat dissipation  <br> - Integrated inductor simplifies PCB design  <br> - Compact, reducing board space requirements | - Requires careful layout for optimal performance  <br> - Slightly more expensive than linear regulators | $2.00 | [AP63203WU-7 Datasheet](#) |  
| MIC4680-3.3   | Switching regulator                      | - High efficiency, small size, better thermal performance  <br> - Reduced power loss, ideal for battery-operated designs | - Slightly complex implementation  <br> - More expensive than linear regulators | $3.50 | [MIC4680 Datasheet](#) |  
| MP2359        | Step-down converter                      | - Compact, efficient, good power management  <br> - Works well with higher input voltages   | - Requires more external components, increasing PCB complexity  <br> - Design complexity compared to linear regulators | $2.50 | [MP2359 Datasheet](#) |  

**Chosen Component:** **AP63203WU-7**  

**Rationale:**  
I selected the AP63203WU-7 because it offers high efficiency with minimal heat dissipation, making it ideal for my system’s power requirements. Its integrated inductor reduces PCB complexity, and its compact size helps optimize board space. This component provides a good balance between performance, cost, and ease of implementation.

---

## 3. Microcontroller Selection & Pin Allocation

My design requires a microcontroller that supports PWM, UART, and SPI communication while offering sufficient GPIOs for motor control and sensor interfacing. After careful evaluation, I selected the **PIC18F47Q10** due to its compatibility with my motor driver and other components.

**Chosen Microcontroller:** **PIC18F47Q10**

This selection ensures efficient communication and control of the motor driver and voltage regulation system, maintaining seamless functionality within the project.

---

## 4. Motor Pump  

In my design, I am repurposing a Motor Pump to function as a bidirectional motor. This choice is based on the need for a compact, efficient motor with built-in water resistance, making it ideal for my application.  

### Component Options  

| Component   | Description                                      | Pros                                                                                         | Cons                                                                                      | Cost   | Link                         |  
|------------|--------------------------------------------------|----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|--------|------------------------------|  
| SEN0229    | Water generator turbine repurposed as a motor   | - Provides high torque for its size  <br> - Compact, water-resistant, and durable            | - Designed primarily as a generator, requiring control adjustments  <br> - Potential efficiency losses when used as a motor | $12.00 | [SEN0229 Datasheet](#)       |  
| JGA25-371  | DC gear motor with bidirectional control        | - High torque and efficiency  <br> - Compact size with built-in gearbox                      | - Not inherently water-resistant  <br> - Requires additional sealing for wet environments | $15.00 | [JGA25-371 Datasheet](#)     |  
| RS-385     | Small brushed DC motor                          | - Affordable and widely available  <br> - Can be used with bidirectional motor drivers       | - Lower torque output compared to geared motors  <br> - Not water-resistant               | $8.00  | [RS-385 Datasheet](#)        |  


**Rationale:**  
The SEN0229 is originally designed as a generator but can function as a motor when electrical power is applied. Its robust design and ability to operate in wet environments make it an innovative choice for this application. The IFX9201SGAUMA1 motor driver allows for precise control of the SEN0229 in bidirectional mode, ensuring smooth operation. This approach offers a unique way to utilize an existing component creatively, reducing the need for additional mechanical modifications.

---

## Final Microcontroller Choice & Justification

The **PIC18F47Q10** is the best choice because it provides the necessary communication protocols, PWM control, and sufficient GPIOs to interface with the motor driver and other components. It ensures smooth integration with the IFX9201SGAUMA1 motor driver and the AP63203WU-7 voltage regulator. Additionally, the SEN0229 water generator turbine, when used as a motor, introduces a novel approach to actuation, and the PIC18F47Q10 offers the required control flexibility. This selection balances cost, efficiency, and ease of implementation, ensuring optimal system performance.

---

## Additional Components

Apart from the major components, various small components are incorporated into the design to ensure proper functionality:

- **LEDs** – Used for debugging and status indication.
- **Capacitors** – Used for power stabilization and noise reduction.
- **Resistors** – Used for current limiting and signal conditioning.
- **Diodes** – Provide protection against voltage spikes and reverse polarity.
- **Connectors and Headers** – Facilitate secure connections between components and external interfaces.

These additional components play a vital role in ensuring the stability and reliability of the overall system.

---

## MPLAB Test

![MPLAB](docs/subfolder/MP_Lab Screenshot.png)

---

## Pin Allocation

| Module     | # Available         | Needed | Associated Pins (or * for any)                                  |
| ---------- | ------------------- | ------ | --------------------------------------------------------------- |
| GPIO       | 21                  | 5      | RC_2 , RC_3 , RC_4 , RA_5 , RA_6                                |
| ADC        | 10                  | 0      | ...                                                             |
| UART       | 2                   | 2      | RX-RC_6 , TX-RC_7                                               |
| SPI        | 2                   | 2      | SI-RC_4 , SO-RC_5                                               |
| I2C        | 2                   | 0      | ...                                                             |
| PWM        | ?                   | ?      | ...                                                             |
| ICSP       | 3                   | 3      | RB-6,RB-7,MCLR                                                  |
| ...        | ...                 | ...    | ...                                                             |

---

## Power Budget

[Power Budget ](https://docs.google.com/spreadsheets/d/156U78wOTZNQjrQSCz7AoKw_OFbUZ7FwH/edit?usp=sharing&ouid=107614409505094169035&rtpof=true&sd=true)
