
# Motor Driver Subsystem API Documentation

## Introduction
This document outlines the Application Programming Interface (API) for the Motor Driver Subsystem in a daisy-chained UART communication system. It describes how the subsystem receives, validates, and responds to UART messages. The motor is controlled using SPI through the IFX9201SGAUMA1 driver IC. The communication is carefully framed and designed to prevent message corruption, allow subsystem coordination, and maintain system integrity. Each design decision is supported by a clear explanation of what, why, when, where, and how it was made.

---

## Message Flow and Protocol
The motor driver subsystem is part of a chain of UART-connected devices, each listening to all communication. Messages follow a strict 8-byte protocol format starting and ending with the bytes `'F'` and `'S'`, which act as clear delimiters. If the message is valid and addressed to the motor subsystem (`'S'`), it is executed. Otherwise, the message is ignored or forwarded unmodified. SPI is used to issue motor commands to the IFX9201, ensuring precise motor control. After execution, the subsystem broadcasts its status to the HMI (`K`) and MQTT server (`T`) to keep them updated.

---

## Message Types

### Messages Received
| Type | Description | Action |
|------|-------------|--------|
| `'0'` | Motor Control Command | Executes the corresponding SPI motor command if the receiver ID is `'S'` |

### Messages Sent
| Type | Description | Action |
|------|-------------|--------|
| `'0'` | Motor Status Update | Broadcasts the motor's current state to `'K'` (HMI) and `'T'` (MQTT) after executing a command |

---

## Message Format (8 Bytes Total)

### Format Structure
| Byte | Field         | Description |
|------|---------------|-------------|
| 1    | `'F'`         | First start byte, used to mark message beginning |
| 2    | `'S'`         | Second start byte, used to mark message beginning |
| 3    | Sender ID     | Identifier of the system that created the message |
| 4    | Receiver ID   | Intended recipient system (e.g., `'S'`) |
| 5    | Message Type  | Type `'0'` indicates motor control/status message |
| 6    | Data          | `'1'` = Motor Off, `'2'` = Motor Forward, `'3'` = Motor Reverse |
| 7    | `'F'`         | First end byte, marks message end |
| 8    | `'S'`         | Second end byte, marks message end |

---

## Incoming Message Example
| Byte | Value | Meaning |
|------|-------|---------|
| 1    | `'F'` | Start of frame |
| 2    | `'S'` | Start of frame |
| 3    | `'T'` | Sender: Team Controller |
| 4    | `'S'` | Receiver: This subsystem |
| 5    | `'0'` | Type: Motor Control Command |
| 6    | `'2'` | Data: Run motor forward |
| 7    | `'F'` | End of frame |
| 8    | `'S'` | End of frame |

## Outgoing Message Example
| Byte | Value | Meaning |
|------|-------|---------|
| 1    | `'F'` | Start of frame |
| 2    | `'S'` | Start of frame |
| 3    | `'S'` | Sender: This subsystem (motor driver) |
| 4    | `'T'` or `'K'` | Receiver: MQTT or HMI system |
| 5    | `'0'` | Type: Status update |
| 6    | `'2'` | Data: Motor is running forward |
| 7    | `'F'` | End of frame |
| 8    | `'S'` | End of frame |

---

## Handling Logic (UART + SPI)

### Receiving:
- Incoming UART bytes are continuously shifted into a buffer.
- If the message has the correct framing (`'F'`, `'S'` at both ends), it is validated.
- Only messages addressed to `'S'` are processed; others are ignored or forwarded.
- Valid messages result in SPI commands: `motor_off`, `motor_forward`, or `motor_reverse`.
- After command execution, two status messages are sent to `'K'` and `'T'`.

### Sending:
- Messages are constructed with updated sender/receiver/type/data fields.
- Each byte is transmitted one by one using `EUSART1_Write()`.
- The same status message is sent twice (once per subsystem).

---

## Receiver Logic (5W Analysis)

### What:
The subsystem listens for UART messages, filters and validates them, and performs motor control actions if applicable. Messages not for this subsystem are forwarded without modification.

### Why:
This approach ensures only relevant commands trigger actions, improving reliability and preventing accidental or duplicated operations due to malformed or misrouted messages.

### When:
The validation and control logic is triggered inside the infinite `while(1)` loop in `main.c`. The subsystem is always active and responsive to incoming messages.

### Where:
All logic is implemented in the `main.c` file of the firmware project. Message reading is done via the `EUSART1_Read()` function, and parsing occurs using a rolling 8-byte buffer.

### How:
As bytes are received, they're added to a buffer. The code checks the framing pattern and message contents. If valid and for `'S'`, the corresponding SPI function is called. Otherwise, the message is skipped or forwarded.

---

## Sender Logic (5W Analysis)

### What:
After processing a motor control command, the subsystem sends out two UART status messages to keep other systems informed of the action just taken.

### Why:
Broadcasting the motor state ensures that both HMI and server subsystems remain synchronized and aware of the motor status, which is important for real-time monitoring and control feedback.

### When:
This logic runs immediately after a valid message is processed and the motor state is changed. It ensures quick and consistent feedback to the team network.

### Where:
The message creation and transmission occur in `main.c` using helper functions like `send_status_to()` and `send_uart_message()`.

### How:
For each status message, a formatted 8-byte array is filled with the correct values and then sent byte by byte through the UART using the transmission-ready flag.

---

## Execution Flow Example
1. Subsystem receives `'FS'`, `'T'`, `'S'`, `'0'`, `'2'`, `'FS'`, indicating a motor forward command.
2. The message is validated and parsed, and `motor_forward()` is executed via SPI.
3. Two status updates are sent back:
   - `'FS'`, `'S'`, `'T'`, `'0'`, `'2'`, `'FS'`
   - `'FS'`, `'S'`, `'K'`, `'0'`, `'2'`, `'FS'`

These messages confirm that the motor is now running forward.

---
