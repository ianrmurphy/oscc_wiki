# Throttle

The throttle firmware is responsible for reading values from the accelerator position sensor, sending reports on its state, and receiving throttle commands and fault reports from the control CAN. When the throttle firmware receives a throttle command message, it will then output the commanded high and low spoof signals onto its connection to the ECU, sending it throttle requests. Receiving a fault report will cause the throttle module to disable.

## Faults

There are three possible fault states:

* Sensor disconnect
    * The firmware will periodically check the sensors to ensure none of them read a value of zero (an indication that they have become disconnected).
    * If a sensor is detected to be disconnected, control will be disabled and a fault report will be published
* Command timeout
    * The firmware will monitor the time between receiving commands and ensure there is a steady connection to a higher level application
    * If it is determined that commands are no longer being received, control will be disabled and a fault report will be published
* Operator override
    * The firmware will monitor for a manual actuation of the accelerator pedal by a human operator in the event of an emergency override of OSCC control
    * If an operator override is detected, control will be disabled and a fault report will be published

All modules listen for the fault report message and disable themselves if learning of a fault in a different module.

## Commands and Reports

* Receives throttle commands from the API
* Receives fault reports from other modules
* Sends throttle reports with current state information
* Sends fault reports on fault event

## CAN message specifications

### Throttle Command

#### ID: 0x62

#### Transmit Rate: defined by sending application.

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

Follow the [[general build instructions|Firmware#1-firmware_building-and-uploading-firmware]] and then run:

```
make throttle
```

To upload to the Arduino:

```
make throttle-upload
```

