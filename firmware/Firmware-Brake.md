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

### Brake Enable

#### ID: 0x50

> *Note*: Brake reports should be checked after sending brake enable command to ensure that the module received the command and enabled properly.

| Type     | Size (bytes) | Description |
| -------- | ------------ | ----------- |
| uint8_t  | 2            | **OSCC Magic Number** <br> Identifies CAN frame as coming from OSCC. <br> Byte 0 should be 0x05. <br> Byte 1 should be 0xCC. |
| uint8_t  | 6            | **Reserved** |

### Brake Disable

#### ID: 0x51

> *Note*: Brake reports should be checked after sending brake disable command to ensure that the module received the command and disabled properly.

| Type     | Size (bytes) | Description |
| -------- | ------------ | ----------- |
| uint8_t  | 2            | **OSCC Magic Number** <br> Identifies CAN frame as coming from OSCC. <br> Byte 0 should be 0x05. <br> Byte 1 should be 0xCC. |
| uint8_t  | 6            | **Reserved** |


### Brake Command

#### ID: 0x60

> *Note*: Limit the rate at which you send commands onto the CAN bus to prevent overloading the bus and the processors on the modules. A rate of around 20Hz should be sufficient.

#### Transmit Rate: defined by sending application.

| Type     | Size (bytes) | Description |
| -------- | ------------ | ----------- |
| uint8_t  | 2            | **OSCC Magic Number** <br> Identifies CAN frame as from OSCC. <br> Byte 0 should be 0x05. <br> Byte 1 should be 0xCC. |
| uint16_t | 2            | **Pedal Value** <br> [65535 == 100%] |
| uint8_t  | 4            | **Reserved** |

### Brake Report

#### ID: 0x61

#### Transmit Rate: 20ms

| Type     | Size (bytes) | Description |
| -----    | ----         | ----- |
| uint8_t  | 2            | **OSCC Magic Number** <br> Identifies CAN frame as coming from OSCC. <br> Byte 0 should be 0x05. <br> Byte 1 should be 0xCC. |
| uint8_t  | 1            | **Enabled Status** <br> Zero value means disabled (commands are ignored). <br> Non-zero value means enabled (commands are sent to vehicle). |
| uint8_t  | 1            | **Operator Override** <br> Zero value means there has been no operator override. <br> Non-zero value means an operator has physically overridden the system. |
| uint8_t  | 1            | **DTCs** <br> Bitfield of DTCs present in the module. |
| uint8_t  | 3            | **Reserved** |

### Fault Report

#### ID: 0x99

#### Transmit Rate: once, on fault event

| Type     | Size (bytes) | Description |
| -------- | ------------ | ----------- |
| uint8_t  | 2            | **OSCC Magic Number** <br> Identifies CAN frame as coming from OSCC. <br> Byte 0 should be 0x05. <br> Byte 1 should be 0xCC. |
| uint32_t | 4            | **Fault Origin ID** <br> Enum value equaling FAULT_ORIGIN_BRAKE. |
| uint8_t  | 2            | **Reserved** |

## DTCs

The DTC field is an 8-bit bitfield. When a DTC's bit position is a 1, then that DTC is active.

| Bit Position | DTC Description |
| ------------ | --------------- |
| 0            | **Invalid Sensor Value** <br> The firmware has detected that one of the sensors has become disconnected. |

## Build Instructions

Follow the [README build instructions](https://github.com/PolySync/oscc/tree/devel#building-the-firmware), then run:

```
make brake
```

To upload to the Arduino:

```
make brake-upload
```
