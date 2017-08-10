# Brake

The brake firmware is responsible for reading values from the brake actuator, sending reports on its state, and receiving brake commands and fault reports from the control CAN. When the brake firmware receives a brake command message, it will attempt to match the requested brake position by accumulating and releasing brake pressure. Receiving a fault report will cause the brake module to disable.

## System behavior and fault states

The firmware can fault if it is in the enabled state and hasn't received any control commands in a while. It can also fault if it reads invalid values from its connected sensor consecutive times, as this can be a sign that it has become disconnected. When the brake module faults, it will disable and send out a fault report to the control CAN bus, letting the other modules know that they should also disable.

## CAN messages we send and receive

Receives brake command messages, fault messages. Sends brake reports with state information about module, sends fault reports if module detects a fault.

## CAN message specifications

### Brake Command

#### ID: 0x60

#### Transmit Rate: defined by sending application.

| Bits  | Value |
| ----- | ----- |
|  0-15 | OSCC Magic number |
| 16-31 | Pedal command -- [65535 == 100%] |
| 31-39 | Enable -- command to enable or disable brake control. Zero to disable, non-zero to enable. |
| 40-63 | Reserved and not being used. |

### Brake Report

#### ID: 0x61

#### Transmit Rate: 20ms

| Bits  | Value |
| ----- | ----- |
|  0-15 | OSCC Magic number |
| 16-23 | Enabled (0 or 1, see explanation above in Steering Report). |
| 24-31 | Operator Override (0 or 1, see explanation above in Steering Report). |
| 32-39 | DTCs (bitfield, see explanation above in steering report). |
| 40-63 | Reserved and not being used. |

### Fault Report

#### ID: 0x99

#### Transmit Rate: as needed

| Bits  | Value |
| ----- | ----- |
|  0-15 | OSCC Magic number |
| 16-47 | Fault origin ID -- enum equaling FAULT_ORIGIN_BRAKE |
| 48-63 | Reserved and not being used. |

## Error codes

DTC explanation.

## Firmware Use

Flash onto hardware. Shouldn't require any modification, since it's mainly used to pass information between vehicle and API.

## Build instructions

(on README, currently)
