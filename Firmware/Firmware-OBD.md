# Kia OBD-II CAN network messages

## Steering Wheel Angle

### ID: 0x2B0

| Bits  | Value |
| ----- | ----- |
|  0-15 | Steering Wheel Angle (in 1/10th degrees, need to scale up by 10 to get value in degrees) |
| 16-63 | Reserved (we aren't using these for anything currently) |

## Wheel Speed

### ID: 0x4B0

| Bits  | Value |
| ----- | ----- |
|  0-15 | Front left wheel speed (in 1/50th mph, need to scale up by 50 to get value in mph) |
| 16-31 | Front right wheel speed (same units as above) |
| 32-47 | Rear left wheel speed (same units as above) 	|
| 48-63 | Rear right wheel speed (same units as above) 	|

## Brake Pressure

### ID: 0x220

| Bits  | Value |
| ----- | ----- |
|  0-15 | Master cylinder pressure (1/10th bars, need to scale up by 10 to get value in bars) |
| 16-63 | Reserved (not being used currently) |
