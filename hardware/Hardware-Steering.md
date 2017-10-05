## Background

The steering system of the Kia Soul is an Electric Power Assisted Steering (EPAS) system. The steering column contains a high current DC motor, as well as a torque sensor. The torque sensor measures the direction and amount of force on the steering wheel and outputs an Analog signal to the EPAS microprocessor. The microprocessor then controls the motor to "assist" the vehicle steering.

Below is a simple wiring diagram showing the connection between the the EPAS microprocessor and the torque sensor.

![Stock Steering Wiring](images/steering_module/steering_stock_diagram.png)

## Control

The EPAS motor can be controlled by removing the torque sensor input to the EPAS microprocessor and injecting spoofed torques.

The Kia ECU implements fault detection on the torque sensor by detecting discontinuities in the analog signals coming from the sensors. If any discontinuities appear the car will go into a fault state, with the symptom of disabling the power steering. To overcome this, the new torque spoofing microprocessor will interpolate between the torque sensor values and the spoofed values before sensing spoofed signals.

A relay is used to switch the input of the EPAS microprocessor from the stock torque sensors and the spoofed torques.

![Steering Wiring](images/steering_module/steering_diagram.png)

# Resources & Further Reading
- [Torque Sensor Response](docs/steering_module/torque_sensor_spoof.ods)
