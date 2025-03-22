# Motor Driver Subsystem API Documentation

## Introduction
This document specifies the Application Programming Interface (API) for the motor driver subsystem. The API ensures compatibility with the team's overall communication system, facilitating UART-based message exchange for motor control and status monitoring.

## Message Flow and Protocol
The motor driver subsystem communicates within a daisy-chained UART network, following a structured messaging protocol. Messages adhere to the team's specifications and include commands for motor control, sensor data reporting, and acknowledgment responses.

### Message Types

#### Messages Sent by the Motor Driver Subsystem:
1. **Motor Status Update** (Sent to Team Server)  
   - Reports motor operational status, speed, and error conditions.
2. **Acknowledgment Message** (Sent to Sender)  
   - Confirms receipt and execution of motor control commands.

#### Messages Received by the Motor Driver Subsystem:
1. **Motor Control Command** (Received from Team Controller)  
   - Commands the motor driver to start, stop, or adjust speed.
2. **Broadcast System Messages** (Received from Team Server)  
   - General status updates or synchronization messages.

## Message Format
Each message follows a structured format, ensuring proper data interpretation and transmission.

### Message: Motor Control Command
| Byte  | Variable Name | Data Type | Min Value | Max Value | Description |
|-------|--------------|-----------|-----------|-----------|-------------|
| 1     | message_type | uint8_t   | 10        | 10        | Identifies as Motor Control Command |
| 2     | motor_id     | uint8_t   | 1         | 5         | Identifies the specific motor |
| 3     | motor_speed  | int8_t    | -100      | 100       | Motor speed percentage |
| 4     | motor_state  | uint8_t   | 0         | 1         | 0 = Stop, 1 = Start |

### Message: Motor Status Update
| Byte  | Variable Name | Data Type | Min Value | Max Value | Description |
|-------|--------------|-----------|-----------|-----------|-------------|
| 1     | message_type | uint8_t   | 20        | 20        | Identifies as Motor Status Update |
| 2     | motor_id     | uint8_t   | 1         | 5         | Identifies the specific motor |
| 3     | motor_speed  | int8_t    | -100      | 100       | Current motor speed |
| 4     | error_code   | uint8_t   | 0         | 255       | 0 = No Error, Others = Error Codes |

## Handling Code Implementation
The motor driver subsystem must:
- Receive UART messages, extract relevant data, and act accordingly.
- Pass on messages that are not addressed to it.
- Filter out malformed messages.
- Acknowledge correctly formatted messages.
- Implement rate-limiting for message transmission.

### Message Receiver Implementation
- Handle incoming messages from UART.
- Validate message format and discard malformed messages.
- Execute commands intended for the motor driver subsystem.
- Forward messages not addressed to the subsystem.

### Message Sender Implementation
- Format and send motor status updates.
- Include required prefix and suffix.
- Ensure messages comply with team specifications.

