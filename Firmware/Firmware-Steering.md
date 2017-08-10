# Steering

The steering firmware is responsible for reading values from the torque sensor, sending reports on its state, and receiving steering commands and fault reports from the control CAN. When the steering firmware receives a steering command message, it will then output the commanded high and low spoof signals onto its connection to the EPAS ECU, sending it torque requests. Receiving a fault report will cause the steering module to disable.

## System behavior and fault states

The firmware can fault if it is in the enabled state and hasn't received any control commands in a while. It can also fault if it reads invalid values from its connected sensor consecutive times, as this can be a sign that it has become disconnected. When the steering module faults, it will disable and send out a fault report to the control CAN bus, letting the other modules know that they should also disable.

## CAN messages we send and receive

Receives steering command messages, fault messages. Sends steering reports with state information about module, sends fault reports if module detects a fault.

## CAN message specifications

### Steering Command

#### ID: 0x64

#### Transmit Rate: controlled via application sending speed. We control it through joystick commander.

| Bits  | Value |
| ----- | ----- |
|  0-15 | OSCC Magic number. We use these to distinguish our messages from the OBD-II messages sent by the vehicle. |
| 16-31 | Spoof value low -- in steps, to be written directly to the DAC. |
| 32-47 | Spoof value high (same as above) |
| 48-55 | Enable -- command to enable or disable steering control. Zero means disable, non-zero means enable. |
| 56-63 | Reserved, not being used. |

### Steering Report

#### ID: 0x65

#### Transmit Rate: 20ms

| Bits  | Value |
| ----- | ----- |
|  0-15 | OSCC Magic number |
| 16-23 | Enabled -- reports if steering module is enabled (non-zero) or disabled (zero). __*__ |
| 24-31 | Operator Override -- driver override state. __**__ |
| 32-39 | DTCs -- bitfield of DTC's present in the module. __***__ |
| 40-63 | Reserved and not being used. |

### Fault Report

#### ID: 0x99

#### Transmit Rate: as needed

| Bits  | Value |
| ----- | ----- |
|  0-15 | OSCC Magic number |
| 16-47 | Fault origin ID -- enum equaling FAULT_ORIGIN_STEERING |
| 48-63 | Reserved and not being used. |

__*__ *Disabled state will ignore commands.*

__**__ *If this value is zero, there has not been an override, if it is nonzero, an operator has physically overridden the system.*

__***__ *Currently, this is used to report invalid sensor values, but can be extended to report other errors.*

## Error codes

DTC explanation.

## Firmware Use

Flash onto hardware. Shouldn't require any modification, since it's mainly used to pass information between vehicle and API.

## Build instructions

(on README, currently)
