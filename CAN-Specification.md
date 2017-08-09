# Kia OBD-II CAN network messages

## Steering Wheel Angle

### ID: 0x2B0

| Bits  | Value |
| ----- | ----- |
| 0-15  | Steering Wheel Angle (in 1/10th degrees, need to scale up by 10 to get value in degrees) |
| 16-63 | Reserved (we aren't using these for anything currently) |

## Wheel Speed

### ID: 0x4B0

| Bits  | Value |
| ----- | ----- |
| 0-15  | Front left wheel speed (in 1/50th mph, need to scale up by 50 to get value in mph) |
| 16-31 | Front right wheel speed (same units as above) |
| 32-47 | Rear left wheel speed (same units as above) 	|
| 48-63 | Rear right wheel speed (same units as above) 	|
 
## Brake Pressure

### ID: 0x220

| Bits  | Value |
| ----- | ----- |
| 0-15  | Master cylinder pressure (1/10th bars, need to scale up by 10 to get value in bars) |
| 16-63 | Reserved (not being used currently) |

# OSCC Specific Reports and Commands

## Steering Command

### ID: 0x64

### Transmit Rate: controlled via application sending speed. We control it through joystick commander.

| Bits  | Value |
| ----- | ----- |
| 0-15  | OSCC Magic number Bits . We use these to distinguish our messages from the OBD-II messages sent by the vehicle. |
| 16-31 | Spoof value low -- in steps, to be written directly to the DAC. |
| 32-47 | Spoof value high (same as above) |
| 48-55 | Enable -- command to enable or disable steering control. Zero means disable, non-zero means enable. |
| 56-63 | Reserved, not being used. |

## Steering Report

### ID: 0x65

### Transmit Rate: 20ms

| Bits  | Value |
| ----- | ----- |
| 0-15  | OSCC Magic number Bits . |
| 16-23 | Enabled -- reports if steering module is enabled (non-zero) or disabled (zero). Disabled state will ignore commands. |
| 24-31 | Operator Override -- driver override state. If this value is zero, there has not been an override, if it is nonzero, an operator has physically overridden the system. |
| 32-39 | DTCs -- bitfield of DTC's present in the module. Currently, this is used to report invalid sensor values, but can be extended to report other errors. |
| 40-63 | Reserved and not being used. |


## Throttle Command

### ID: 0x62

### Transmit Rate: again, this is controlled by the application sending commands.

| Bits  | Value |
| ----- | ----- |
| 0-15  | OSCC Magic number Bits . |
| 16-31 | Spoof value low -- in steps, to be written directly to the DAC. |
| 32-47 | Spoof value high (same as above) |
| 48-55 | Enable -- command to enable or disable throttle control. Zero means disable, non-zero means enable. |
| 56-63 | Reserved, not being used. |

## Throttle Report

### ID: 0x63

### Transmit Rate: 20ms

| Bits  | Value |
| ----- | ----- |
| 0-15  | OSCC Magic number Bits . |
| 16-23 | Enabled (0 or 1, see explanation above in Steering Report). |
| 24-31 | Operator Override (0 or 1, see explanation above in Steering Report). |
| 32-39 | DTCs (bitfield, see explanation above in steering report). |
| 40-63 | Reserved and not being used. |


## Fault Report

### ID: 0x99

### Transmit Rate: as needed

| Bits  | Value |
| ----- | ----- |
| 0-15  | OSCC Magic number Bits . |
| 16-47 | Fault origin ID -- an enum with the ID of the sending module. Can be FAULT_ORIGIN_BRAKE, FAULT_ORIGIN_STEERING, or FAULT_ORIGIN_THROTTLE. |
| 48-63 | Reserved and not being used. |

## Brake Command

### ID: 0x60

### Transmit Rate: defined by sending application.

| Bits  | Value |
| ----- | ----- |
| 0-15  | OSCC Magic number Bits . |
| 16-31 | Pedal command -- [65535 == 100%] |
| 31-39 | Enable -- command to enable or disable brake control. Zero to disable, non-zero to enable. |
| 40-63 | Reserved and not being used. |

## Brake Report

### ID: 0x61

### Transmit Rate: 20ms

| Bits  | Value |
| ----- | ----- |
| 0-15  | OSCC Magic number Bits . |
| 16-23 | Enabled (0 or 1, see explanation above in Steering Report). |
| 24-31 | Operator Override (0 or 1, see explanation above in Steering Report). |
| 32-39 | DTCs (bitfield, see explanation above in steering report). |
| 40-63 | Reserved and not being used. |