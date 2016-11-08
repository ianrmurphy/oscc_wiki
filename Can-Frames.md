There are a number of different CAN buses present on the Kia Soul. This page captures what is known about various CAN frames the C-CAN bus on the Kia Soul. The C-CAN bus on the Kia Soul is accessible through the OBD connector on pin 6 (CAN High) and ping 14 (CAN Low)  The currently understood CAN frames are:

* [Turn Signals](#Turn Signals)
* [Steering Wheel Angle](#Steering Wheel Angle)
* [Wheel Speeds](#Wheel Speeds)
* [Brake Pressure](#Brake Pressure)

## Turn Signals
**CAN ID = 0x18**

* Left turn: Byte5 = 0xc0
* Right turn: Byte5 0xA0

## Steering Wheel Angle
**CAN ID = 0x2B0**

Scale Factor = 10

* Steering Angle (degrees) = (Byte0 + Byte1 << 8) / Scale Factor

## Wheel Speeds
**CAN ID = 0x4B0**

Scale Factor = 128

* LF Wheel Speed (mpg) = (Byte0 + Byte1 <<8) / Scale Factor
* RF Wheel Speed (mpg) = (Byte2 + Byte3 <<8) / Scale Factor
* LR Wheel Speed (mpg) = (Byte4 + Byte5 <<8) / Scale Factor
* RR Wheel Speed (mpg) = (Byte6 + Byte7 <<8) / Scale Factor

## Brake Pressure
**CAN ID = 0x220**

Scale Factor = ~10

* Master Cylinder Pressure (bar) = (Byte4 + Byte 5 <<8) / Scale Factor