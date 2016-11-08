# Chassis State 1
**ID**: 0x210

**Source**: CAN Gateway Module

**Transmit Rate**: 50 ms





* **bit 0** | Reserved
* **bit 1** | Steering Wheel Angle Valid
* **bit 2** | Steering Wheel Angle Rate Valid
* **bit 3** | Brake Pressure Valid
* **bit 4** | Wheel Speed Valid
* **bit 5** | Left Turn Signal On
* **bit 6** | Right Turn Signal On
* **bit 7** | Brake Signal On
* **bits 8-15** | Reserved
* **Byte 16-31** | Steering Wheel Angle
* **Byte 32-47** | Steering Wheel Angle Rate
* **Byte 48-63** | Brake Pressure


# Chassis State 2
**ID**: 0x211

**Source**: CAN Gateway Module

**Transmit Rate**: 50 ms


* **bits 0-15** | LF Wheel Speed
* **bits 16-31** | RF Wheel Speed
* **bits 32-47** | LR Wheel Speed
* **bits 48-63** | RR Wheel Speed


# Brake Command
**ID**: 0x060

**Transmit Rate**: 20 ms

**Receive Timeout**: 100 ms


* **bits 0-15** | Pressure Command
 * 0 = 0% 65535 = 100%
* **bit 16** | Brake On
 * 0 = off, 1 = on
* **bit 17-23** | Reserved0
* **bit 24** | Enabled
* **bit 25** | Clear
* **bit 26** | Ignore
* **bit 27-31** | Reserved1
* **bit 32-39** | Reserved2
* **bit 40-47** | Reserved3
* **bit 48-55** | Reserved4
* **bit 56-63** | Count



# Brake Report
**ID**: 0x061
**Receive Rate**: 50 ms

**Receive Timeout**: 100 ms


* **bits 0-15** | Pedal Input
 * 0 = 0% 65535 = 100%
* **bits 16-31** | Pedal Command
 * 0 = 0% 65535 = 100%
* **bits 32-47** | Pedal Output
 * 0 = 0% 65535 = 100%k
* **bit 48** | Brake On Output
* **bit 49** | Brake On Command
* **bit 50** | Brake On Input
* **bit 51** | Watchdog Brake
* **bits 52-55** | Watchdog Source
* **bit 56** | Enabled
* **bit 57** | Override
* **bit 58** | Driver Activity
* **bit 59** | Watchdog Counter Fault State
* **bit 60** | Channel 1 Fault State
* **bit 61** | Channel 2 Fault State
* **bit 62** | Brake Connector Fault State
* **bit 63** | Connector Fault State


# Throttle Command
**ID**: 0x062

**Transmit Rate**: 20 ms

**Receive Timeout**:


* **bits 0-15** | Pedal Command
 * 0 = 0% 65535 = 100%
* **bits 16-23** | Reserved0
* **bit 24** | Enabled
* **bit 25** | Clear
* **bit 26** | Ignore
* **bit 27-31** | Reserved1
* **bit 32-39** | Reserved2
* **bit 40-47** | Reserved3
* **bit 55-48** | Reserved4
* **bit 56-63** | Count


# Throttle Report
**ID**: 0x063

**Receive Rate**: 20 ms

**Receive Timeout**:


* **bits 0-15** | Pedal Input
 * 0 = 0% 65535 = 100%
* **bits 16-31** | Pedal Command
 * 0 = 0% 65535 = 100%
* **bits 32-47** | Pedal Output
 * 0 = 0% 65535 = 100%k
* **bit 48-51** | Reserved0
* **bits 52-55** | Watchdog Source
* **bit 56** | Enabled
* **bit 57** | Override
* **bit 58** | Driver Activity
* **bit 59** | Watchdog Counter Fault State
* **bit 60** | Channel 1 Fault State
* **bit 61** | Channel 2 Fault State
* **bit 62** | Brake Connector Fault State
* **bit 63** | Connector Fault State


# Steering Command
**ID**: 0x064

**Receive Rate**: 20 ms

**Receive Timeout**: 150 ms


* **bits 0-15** | Steering Wheel Angle Command
 * 0 = 0% 65535 = 100%
* **bit 16** | Enabled
* **bit 17** | Clear
* **bit 18** | Ignore
* **bit 19-23** | Reserved0
* **bit 24-31** | Steering Wheel Max Velocity
 * 0 = No limit
 * Value 0x01 means 2 degrees/second.
 * Value 0xFA means 500 degrees/second. [2 degrees/second per bit]
* **bit 32-47** | Torque
* **bit 48-55** | Reserved3
* **bit 56-63** | Count


# Steering Report
**ID**: 0x065

**Receive Rate**: 20 ms

**Receive Timeout**: TODO


* **bits 0-15** | Angle
 * 0 = 0% 65535 = 100%
* **bits 16-31** | Angle Command
 * 0 = 0% 65535 = 100%
* **bits 32-47** | Vehicle Speed
 * Vehicle speed. [0.01 kilometers/hour per bit]
* **bit 48-55** | Torque
* **bit 56** | Enabled
* **bit 57** | Override
* **bit 58** | Driver Activity
* **bit 59** | Watchdog Counter Fault State
* **bit 60** | Channel 1 Fault State
* **bit 61** | Channel 2 Fault State
* **bit 62** | Fault Calibration
* **bit 63** | Connector Fault State
