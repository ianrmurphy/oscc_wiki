# Throttle

The throttle firmware is responsible for reading values from the accelerator position sensor, sending reports on its state, and receiving throttle commands and fault reports from the control CAN. When the throttle firmware receives a throttle command message, it will then output the commanded high and low spoof signals onto its connection to the ECU, sending it throttle requests. Receiving a fault report will cause the throttle module to disable.

## System behavior and fault states

The firmware can fault if it is in the enabled state and hasn't received any control commands in a while. It can also fault if it reads invalid values from its connected sensor consecutive times, as this can be a sign that it has become disconnected. When the throttle module faults, it will disable and send out a fault report to the control CAN bus, letting the other modules know that they should also disable.

## CAN messages we send and receive

Receives throttle command messages, fault messages. Sends throttle reports with state information about module, sends fault reports if module detects a fault.

## CAN message specifications

### Throttle Command

#### ID: 0x62

#### Transmit Rate: again, this is controlled by the application sending commands.

| Bits  | Value |
| ----- | ----- |
|  0-15 | OSCC Magic number |
| 16-31 | Spoof value low -- in steps, to be written directly to the DAC. |
| 32-47 | Spoof value high (same as above) |
| 48-55 | Enable -- command to enable or disable throttle control. Zero means disable, non-zero means enable. |
| 56-63 | Reserved, not being used. |

### Throttle Report

#### ID: 0x63

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
| 16-47 | Fault origin ID -- enum equaling FAULT_ORIGIN_THROTTLE |
| 48-63 | Reserved and not being used. |

## Error codes

DTC explanation.

## Firmware Use

Flash onto hardware. Shouldn't require any modification, since it's mainly used to pass information between vehicle and API.

## Build instructions

(on README, currently)
