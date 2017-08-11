# Brake

The brake firmware is responsible for reading values from the brake actuator, sending reports on its state, and receiving brake commands and fault reports from the control CAN. When the brake firmware receives a brake command message, it will attempt to match the requested brake position by accumulating and releasing brake pressure. Receiving a fault report will cause the brake module to disable.

## Faults

There are four possible fault states:

* Startup check failure
    * On startup, the firmware will check sensor pins on the brake actuator to ensure it is functioning properly.
    * If it is determined that the actuator is not operating normally, the module will not enable.
    * **NOTE:** On actuator control boards below version 1.0.1, the startup check must be disabled during the cmake step with `-DBRAKE_STARTUP_TEST=FALSE`.
* Sensor disconnect
    * The firmware will periodically check the sensors to ensure none of them read a value of zero (an indication that they have become disconnected).
    * If a sensor is detected to be disconnected, control will be disabled and a fault report will be published
* Command timeout
    * The firmware will monitor the time between receiving commands and ensure there is a steady connection to a higher level application
    * If it is determined that commands are no longer being received, control will be disabled and a fault report will be published
* Operator override
    * The firmware will monitor for a manual actuation of the brake pedal by a human operator in the event of an emergency override of OSCC control
    * If an operator override is detected, control will be disabled and a fault report will be published

All modules listen for the fault report message and disable themselves if learning of a fault in a different module.

## Commands and Reports

* Receives brake commands from the API
* Receives fault reports from other modules
* Sends brake reports with current state information
* Sends fault reports on fault event

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

#### Transmit Rate: once, on fault event

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

Follow the [[general build instructions|Firmware#1-firmware_building-and-uploading-firmware]] and then run:

```
make brake
```

To upload to the Arduino:

```
make brake-upload
```
