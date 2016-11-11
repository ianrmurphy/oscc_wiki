# Background

All four of the modules that comprise the control system of the Kia need to be powered with 12 volts DC. Three of the units (throttle, steering, and brake) should have an emergency stop button that cuts power to the aforementioned units. However, the CAN gateway module should be powered on at ignition. Additionally, high-current power needs to be supplied to the brake module for the pump motor and the solenoids. All units should be fused. 

An emergency stop button is placed in the car along with a rocker switch that switches power for the whole system. A relay is placed in the engine compartment along with fuses for the system. Power is run for the brake unit under the hood and the steering, throttle, and CAN module in the cabin. It is also likely that sensors and computers will be added, amping up the power requirements of the system.

Below is a diagram for a simple power distribution that meets the requirements discussed above.
[[https://github.com/PolySync/OSCC/blob/master/assets/power-distribution.png|alt=Power Distribution]]

# Hardware


### Parts

| Part          | Price  | Quantity |
| ------------- | -----:| ----:| 
| 12 AWG Primary Wire (red) | < $10.00 | ~20' |
| 12 AWG Primary Wire (black) | < $10.00 | ~20' |
| 22 AWG Stranded Hookup Wire (black) | $12.34 | 100' |
| 22 AWG Stranded Hookup Wire (red) | $12.34 | 100' |
| 22 AWG Stranded Hookup Wire (blue) | $6.43 | 25' |
| Battery Lugs | $6.43 | 25' |
| [[Autonics E-Stop Button|http://www.ursele.com/contact/]]      | $7.95 | 1 |
| [[20.2mm SPST Lighted Rocker|http://www.ursele.com/contact/]]      | $6.95 | 1 |
| [[20.2mm SPST Momentary Button|http://www.ursele.com/contact/]]      | $6.95 | 1 |
| [[Blue Sea Systems Inline Fuse Holder |https://www.amazon.com/Blue-Sea-Systems-Waterproof-Holder/dp/B004ZIUA62]] |     $43.51          | 2 |
| [[ Weatherproof Automotive Relay & Holder|https://www.amazon.com/Pico-Terminal-Automotive-Change-Over-Connector/dp/B007UTFJHS]] | $15.69 | 2 | 
| [[Seaboard HDPE sheet| https://www.amazon.com/Seaboard-Density-Polyethylene-Finish-Length/dp/B00JPHTPCI]] | $18.47 | 1 | 
| [[E-stop Button Cup Holder Mount|https://github.com/PolySync/OSCC/blob/master/3d_models/estop_cupholder/EStopHousing.STL]] | 3D Printed | 1 |
| Zip Ties | ? | 20 |
| Zip Tie sticky foam mount | ? | 20 |




### Installation

1. Cut Seaboard to 5.5" x 6.5" and attach to top of airbox.
2. Attach both fuse holders and relay holders to the seaboard.
3. Run wires for E-stop button and rocker switch into cabin and wire buttons to relay. The relay should be powered only when both the switch is depressed and the E-stop button is not pressed. Both the E-stop button and the relay should cut power to relay.
4. Run wires from the battery to fuses and the relay.
5. Connect the brake module to the switched power of the relay, as well as the throttle and steering modules.
6. The CAN gateway module should receive power from the power bus that is switched from the system power rocker switch. 