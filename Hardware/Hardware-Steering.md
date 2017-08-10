## Background

The steering system of the Kia Soul is an Electric Power Assisted Steering (EPAS) system. The steering column contains a high current DC motor, as well as a torque sensor. The torque sensor measures the direction and amount of force on the steering wheel and outputs an Analog signal to the EPAS microprocessor. The microprocessor then controls the motor to "assist" the vehicle steering.

Below is a simple wiring diagram showing the connection between the the EPAS microprocessor and the torque sensor.

![Stock Steering Wiring](images/steering_module/steering_stock_diagram.png)

## Control


The EPAS motor can be controlled by removing the torque sensor input to the EPAS microprocessor and injecting spoofed torques. However, because the microprocessor uses the feedback from the torque sensor for its internal control loop, a new control loop must be created. This control loop will accept steering angle as its input. Steering angle data is available on the [[private Kia CAN bus|CAN-Gateway-&-Control-CAN]].

The image below shows a high level system of control after we create a system for spoofing torques.
![PID](images/steering_module/steering_pid.png)

The Kia ECU implements fault detection on the torque sensor by detecting discontinuities in the analog signals coming from the sensors. If any discontinuities appear the car will go into a fault state, with the symptom of disabling the power steering. To overcome this, the new torque spoofing microprocessor will interpolate between the torque sensor values and the spoofed values before sensing spoofed signals.

A relay is used to switch the input of the EPAS microprocessor from the stock torque sensors and the spoofed torques.

## Hardware

The new PolySync throttle/steering shield is undergoing testing and validation. Board designs and schematics will be available as soon at the boards are thoroughly proven.


### Parts

| Part          | Price  |
| ------------- | -----:|
| [[Arduino Uno|https://www.arduino.cc/en/Main/arduinoBoardUno]]      | $24.95 |
| [[PolySync Steering/Throttle Shield R0|http://www.oscc.io]]      | TBD |


### Assembly

1. Print [[the enclosure|https://github.com/PolySync/OSCC/blob/master/3d_models/dash_enclosure/STC_housing.STL]] if you haven't yet.
2. Screw the Arduino Uno to the enclosure.
3. Press the sensor interface board onto the Arduino.

### Install

The wiring harness for the torque sensor can be easily spliced onto in order to inject spoofed torque signals to the EPAS microcontroller. The image below shows the high current motor and the connector coming from the torque sensor. There are **redundant torque sensors** (2) and we need to splice into both signals and spoof both.

![Sensor](images/steering_module/steering_motor.png)






1. Cut the **blue** and **green** wires coming from the torque sensors.
2. Splice matching color wires onto the 4 new wire ends to lengthen each wire.
3. Connect the four wires to the EPAS microprocessor.
 * **Green** from the APS goes to SIG IN A.
 * **Blue** from the APS goes to SIG IN B.
 * Remaining **green** goes to SIG OUT A.
 * Remaining **blue** goes to SIG OUT B.
2. Power the unit with the emergency stop power bus.
3. Wire the module to the Control CAN bus.

![Steering Wiring](images/steering_module/steering_diagram.png)

# Resources & Further Reading
- [Torque Sensor Response](docs/steering_module/torque_sensor_spoof.ods)
