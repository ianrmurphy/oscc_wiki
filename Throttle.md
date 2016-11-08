# Background

The throttle system of the Kia Soul is an Electric Throttle Control (ETC) system. Instead of a mechanical cable linkage between the throttle body and the accelerator pedal there is a position sensor on the accelerator pedal and a motorized throttle body.

[[https://github.com/PolySync/SelfDrivingSoul/blob/master/img/throttle_detail_stock.png|alt=Stock Throttle Wiring]]

# Control


The ETC system can be controlled by removing the accelerator position sensor (APS) input to the ETC microprocessor and injecting spoofed position values. The pedal position sensor uses redundant position sensors which both output analog signals. The full range of sensor position values correlate to the full range of throttle from "closed throttle" to "wide open throttle." By injecting the two spoofed position sensor values the throttle can be controlled.

The Kia ECU implements fault detection on the accelerator pedal position sensor by detecting discontinuities in the analog signals coming from the sensors. If any discontinuities appear the car will go into a fault state, with the symptom of having the mapping of the accelerator pedal greatly reduced. To overcome this the new throttle microprocessor will interpolate between the sensor position values and the spoofed values before sensing spoofed positions.

A relay is used to switch the input of the ETC microprocessor from the stock pedal position sensor and the spoofed positions.

# Hardware

The new PolySync throttle/steering shield is undergoing testing and validation. Board designs and schematics will be available as soon at the boards are thoroughly proven.

### Parts

| Part          | Price  |
| ------------- | -----:|
| [[Arduino Uno|https://www.seeedstudio.com/CANBUS-Shield-V12-p-2256.html]]      | $24.95 |
| [[PolySync Steering/Throttle Shield R0|http://www.polysync.io]]      | $50.00 |



### Assembly
1. Print [[the dash enclosure|https://github.com/PolySync/OSCC/blob/master/3d_models/dash_enclosure/STC_housing.STL]] if you haven't yet.
2. Screw the Arduino Uno to the enclosure.
3. Press the steering control board onto the Arduino.
4. Upload the firmware to the controller.

### Installation

[[https://github.com/PolySync/SelfDrivingSoul/blob/master/img/throttle_detail.png|alt=Throttle Wiring]]

1. Cut the **blue** and **green** wires coming from the APS.
2. Splice matching color wires onto the 4 new wire ends to lengthen each wire.
3. Connect the four wires to the throttle control module.
 * **Green** from the APS goes to SIG IN A.
 * **Blue** from the APS goes to SIG IN B.
 * Remaining **green** goes to SIG OUT A.
 * Remaining **blue** goes to SIG OUT B
2. Power the unit with the emergency stop power bus.
3. Wire the module to the Control CAN bus.


# Resources & Further Reading
- [[Accelerator position sensor values|https://github.com/PolySync/SelfDrivingSoul/blob/master/Throttle/Datasheets/PositionSensor.pdf]]
- [[Accelerator Pedal Wiring Diagram|https://github.com/PolySync/SelfDrivingSoul/blob/master/Throttle/Datasheets/AcceleratorPositionSensorPinout.pdf]]
