# Background

The throttle system of the Kia Soul is an Electric Throttle Control (ETC) system. Instead of a mechanical cable linkage between the throttle body and the accelerator pedal, there is a position sensor on the accelerator pedal and a motorized throttle body.

![Stock Throttle Wiring](images/throttle_module/throttle_stock_diagram.png)

# Control

The ETC system can be controlled by removing the accelerator position sensor (APS) input to the ETC microprocessor and injecting spoofed position values. The pedal position sensor uses redundant position sensors that both output analog signals. The full range of sensor position values correlate to the full range of throttle from "closed throttle" to "wide open throttle." By injecting the two spoofed position sensor values the throttle can be controlled.

The Kia ECU implements fault detection on the accelerator pedal position sensor by detecting discontinuities in the analog signals coming from the sensors. If any discontinuities appear the car will go into a fault state, with the symptom of having the mapping of the accelerator pedal greatly reduced. To overcome this the new throttle microprocessor will interpolate between the sensor position values and the spoofed values before sensing spoofed positions.

A relay is used to switch the input of the ETC microprocessor from the stock pedal position sensor and the spoofed positions.

# Resources & Further Reading
- [Accelerator Position Sensor Response](docs/throttle_module/throttle_sensor_spoof.ods)
- [Accelerator Pedal Wiring Diagram](/images/throttle_module/aps_wiring.png)
