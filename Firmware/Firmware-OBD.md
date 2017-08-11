# Kia OBD-II CAN Messages

## Steering Wheel Angle

### ID: 0x2B0

| Type     | Size (bytes) | Description |
| -------- | ------------ | ----------- |
| int16_t  | 2            | **Steering Wheel Angle** <br> 1/10th degrees <br> Need to scale up by 10 to get value in degrees. |
| uint8_t  | 6            | **Reserved** |

## Wheel Speed

### ID: 0x4B0

| Type     | Size (bytes) | Description |
| -------- | ------------ | ----------- |
| int16_t  | 2            | **Front Left Wheel Speed** <br> 1/50th MPH <br> Need to scale up by 50 to get value in MPH. |
| int16_t  | 2            | **Front Right Wheel Speed** <br> 1/50th MPH <br> Need to scale up by 50 to get value in MPH. |
| int16_t  | 2            | **Rear Left Wheel Speed** <br> 1/50th MPH <br> Need to scale up by 50 to get value in MPH. |
| int16_t  | 2            | **Rear Right Wheel Speed** <br> 1/50th MPH <br> Need to scale up by 50 to get value in MPH. |

## Brake Pressure

### ID: 0x220

| Type     | Size (bytes) | Description |
| -------- | ------------ | ----------- |
| int16_t  | 2            | **Master Cylinder Pressure** <br> 1/10th bars <br> Need to scale up by 10 to get value in bars. |
| uint8_t  | 6            | **Reserved** |
