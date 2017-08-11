# Steering

The steering firmware is responsible for reading values from the torque sensor, sending reports on its state, and receiving steering commands and fault reports from the control CAN. When the steering firmware receives a steering command message, it will then output the commanded high and low spoof signals onto its connection to the EPAS ECU, sending it torque requests. Receiving a fault report will cause the steering module to disable.

## Faults

There are three possible fault states:

* Sensor disconnect
    * The firmware will periodically check the sensors to ensure none of them read a value of zero (an indication that they have become disconnected).
    * If a sensor is detected to be disconnected, control will be disabled and a fault report will be published
* Command timeout
    * The firmware will monitor the time between receiving commands and ensure there is a steady connection to a higher level application
    * If it is determined that commands are no longer being received, control will be disabled and a fault report will be published
* Operator override
    * The firmware will monitor for a manual actuation of the steering wheel by a human operator in the event of an emergency override of OSCC control
    * If an operator override is detected, control will be disabled and a fault report will be published

All modules listen for the fault report message and disable themselves if learning of a fault in a different module.

## Commands and Reports

* Receives steering commands from the API
* Receives fault reports from other modules
* Sends steering reports with current state information
* Sends fault reports on fault event

## CAN message specifications

### Steering Command

#### ID: 0x64

#### Transmit Rate: defined by sending application.

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

#### Transmit Rate: once, on fault event

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

Follow the [[general build instructions|Firmware#1-firmware_building-and-uploading-firmware]] and then run:

```
make steering
```

To upload to the Arduino:

```
make steering-upload
```
