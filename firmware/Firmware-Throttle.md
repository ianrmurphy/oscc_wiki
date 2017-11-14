# Throttle

The throttle firmware is responsible for reading values from the accelerator position sensor, sending reports on its state, and receiving throttle commands and fault reports from the control CAN. When the throttle firmware receives a throttle command message, it will then output the commanded high and low spoof signals onto its connection to the ECU, sending it throttle requests. Receiving a fault report will cause the throttle module to disable.

## Faults

There are three possible fault states:

* Sensor disconnect
    * The firmware will periodically check the sensors to ensure none of them read a value of zero (an indication that they have become disconnected)
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


### Throttle Enable

#### ID: 0x52

> *Note*: Throttle reports should be checked after sending throttle enable command to ensure that the module received the command and enabled properly.

| Type     | Size (bytes) | Description |
| -------- | ------------ | ----------- |
| uint8_t  | 2            | **OSCC Magic Number** <br> Identifies CAN frame as coming from OSCC. <br> Byte 0 should be 0x05. <br> Byte 1 should be 0xCC. |
| uint8_t  | 6            | **Reserved** |

### Throttle Disable

#### ID: 0x53

> *Note*: Throttle reports should be checked after sending throttle disable command to ensure that the module received the command and disabled properly.

| Type     | Size (bytes) | Description |
| -------- | ------------ | ----------- |
| uint8_t  | 2            | **OSCC Magic Number** <br> Identifies CAN frame as coming from OSCC. <br> Byte 0 should be 0x05. <br> Byte 1 should be 0xCC. |
| uint8_t  | 6            | **Reserved** |

### Throttle Command

#### ID: 0x62

> *Note*: Limit the rate at which you send commands onto the CAN bus to prevent overloading the bus and the processors on the modules. A rate of around 20Hz should be sufficient.

| Type     | Size (bytes) | Description |
| -------- | ------------ | ----------- |
| uint8_t  | 2            | **OSCC Magic Number** <br> Identifies CAN frame as coming from OSCC. <br> Byte 0 should be 0x05. <br> Byte 1 should be 0xCC. |
| uint16_t | 2            | **Spoof Value Low** <br> Value to be sent on the low spoof signal to the DAC. <br> Voltage converted to a 12-bit step value. |
| uint16_t | 2            | **Spoof Value High** <br> Value to be sent on the high spoof signal to the DAC. <br> Voltage converted to a 12-bit step value. |
| uint8_t  | 2            | **Reserved** |

### Throttle Report

#### ID: 0x63

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

#### Transmit Rate: as needed

| Type     | Size (bytes) | Description |
| -------- | ------------ | ----------- |
| uint8_t  | 2            | **OSCC Magic Number** <br> Identifies CAN frame as coming from OSCC. <br> Byte 0 should be 0x05. <br> Byte 1 should be 0xCC. |
| uint32_t | 4            | **Fault Origin ID** <br> Enum value equaling FAULT_ORIGIN_THROTTLE. |
| uint8_t  | 2            | **Reserved**

## DTCs

| Bit Position | DTC Description |
| ------------ | --------------- |
| 0            | **Invalid Sensor Value** <br> The firmware has detected that one of the sensors has become disconnected. |

## Build Instructions

Follow the [README build instructions](https://github.com/PolySync/oscc/tree/devel#building-the-firmware), then run:

```
make throttle
```

To upload to the Arduino:

```
make throttle-upload
```

