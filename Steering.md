## Background

The steering system of the Kia Soul is an Electric Power Assisted Steering (EPAS) system. The steering column contains a high current DC motor, as well as a torque sensor. The torque sensor measures the direction and amount of force on the steering wheel and outputs an Analog signal to the EPAS microprocessor. The microprocessor then controls the motor to "assist" the vehicle steering.

Below is a simple wiring diagram showing the connection between the the EPAS microprocessor and the torque sensor.

[[https://github.com/PolySync/SelfDrivingSoul/blob/master/img/steering_detail_stock.png|alt=Stock Steering Wiring]]

## Control


The EPAS motor can be controlled by removing the torque sensor input to the EPAS microprocessor and injecting spoofed torques. However, because the microprocessor uses the feedback from the torque sensor for its internal control loop, a new control loop must be created. This control loop will take steering angle as its input. Steering angle data is available on the [[private Kia CAN bus|CAN-Gateway-&-Control-CAN]].

The image below shows a high level system of control after we create a system for spoofing torques.
[[https://github.com/PolySync/SelfDrivingSoul/blob/master/img/steering_pid.png|alt=PID]]

The Kia ECU implements fault detection on the torque sensor by detecting discontinuities in the analog signals coming from the sensors. If any discontinuities appear the car will go into a fault state, with the symptom of disabling the power steering. To overcome this, the new torque spoofing microprocessor will interpolate between the torque sensor values and the spoofed values before sensing spoofed signals.

A relay is used to switch the input of the EPAS microprocessor from the stock torque sensors and the spoofed torques.

## Hardware

The new PolySync throttle/steering shield is undergoing testing and validation. Board designs and schematics will be available as soon at the boards are thoroughly proven. 


### Parts

| Part          | Price  |
| ------------- | -----:|
| [[Aruino Uno|https://www.seeedstudio.com/CANBUS-Shield-V12-p-2256.html]]      | $24.95 |
| [[PolySync Steering/Throttle Shield R0|http://www.polysync.io]]      | $50.00 |


### Assembly
1. Print [[the enclosure|https://github.com/PolySync/OSCC/blob/master/3d_models/dash_enclosure/STC_housing.STL]] if you haven't yet.
2. Screw the Arduino Uno to the enclosure.
3. Press the steering control board onto the Arduino.

### Install

The wiring harness for the torque sensor can be easily spliced onto in order to inject spoofed torque signals to the EPAS microcontroller. The image below shows the high current motor and the connector coming from the torque sensor. There are **redundant torque sensors** (2) and we need to splice into both signals and spoof both.

[[https://github.com/PolySync/SelfDrivingSoul/blob/master/img/steering_motor.png|alt=Sensor]]






1. Cut the **blue** and **green** wires coming from the torque sensors.
2. Splice matching color wires onto the 4 new wire ends to lengthen each wire.
3. Connect the four wires to the EPAS microprocessor.
 * **Green** from the APS goes to SIG IN A.
 * **Blue** from the APS goes to SIG IN B.
 * Remaining **green** goes to SIG OUT A.
 * Remaining **blue** goes to SIG OUT B
2. Power the unit with the emergency stop power bus.
3. Wire the module to the Control CAN bus.

[[https://github.com/PolySync/SelfDrivingSoul/blob/master/img/steering_detail.png|alt=Steering Wiring]]

# Resources & Further Reading
- [[Torque Sensor Response|https://github.com/PolySync/OSCC/blob/master/vehicle_info/kia_soul_ps/steering/torque_sensor_spoof.ods]]
