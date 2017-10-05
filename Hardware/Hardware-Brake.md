# Background

As braking in the Kia Soul is a traditional mechanical system, the factory standard Soul has no ability to control braking electronically. There are a number of models of vehicles with electronically controlled brake systems, notably the 2004-2009 Prius. This model Prius uses an electronically controlled actuator with no microprocessor, it is controlled from the Prius ECU. There are 7 pressure sensors on the device, 10 proportional solenoids, an accumulator, a pump, diagnostics components, and a pressure relief valve. This unit can be sourced from auto salvage yards and installed into the existing Kia brake system without adversely effecting the stock brake system and adding by-wire control. When sourcing these units be sure to request the pigtail adapter for the actuator as well.

![Prius Brake Setup](images/brake_module/brake_actuator.png)

The image below illustrates the brake actuator as it is installed in a Prius. Notice you can see which solenoids are normally open and which are normally closed.

![Prius Brake Setup](images/brake_module/brake_stock_diagram.png)

# Control

The Prius brake actuator is a simple but powerful device. It is useful to first understand how the device works when it is installed in a Prius.

The brake actuator has a pump and an accumulator. When the pump is energized, pressure is built up in the accumulator. This pressure can then be distributed to 4 hydraulic lines, each of which can be controlled independently using two proportional solenoids: one to fill pressure and one to spill pressure. Using pulse width modulation (PWM), the amount that the solenoids open or close can be varied. Each of these four hydraulic lines has a pressure sensor for measuring brake pressure at the wheels.

When a Prius is powered on two master cylinder lockout solenoids that are closed, it prevents fluid from passing from the master cylinder to the brakes. The sensation of fluid passing from the master cylinder to brakes is emulated by a stroke simulator. Pressure in the master cylinder is measured and braking is then done by the regenerative braking system.

Certain solenoids are normally closed while others are normally open. The solenoid system is designed in such a way that in the event of power failure the brake pressure at the rear wheels is spilled entirely, and the pressure at the front wheels is regulated by driver (the master cylinder lockout solenoids open). **As the actuator is installed in a Prius, without power supplied to the actuator fluid passes through the device to the front wheels only, no fluid passes through to the the rear wheels.** In order to avoid this behavior in the Kia Soul only the front two lines of the actuator unit are used and the rear two lines are plugged. If actuator is not powered, fluid will pass through it from the master cylinder to all four brake lines on the vehicle.

In an autonomous situation (where a driver is not using the brake pedal) the 2 master cylinder solenoids will be closed, preventing brake pressure at the the wheels spilling back into the master cylinder. There are two master cylinder pressure sensors that can be used to detect pressure at the pedal.

The accumulator has a pressure sensor for detecting the pressure of the fluid built up in the accumulator. The accumulator pump is a high current (~30 Amps) pump and can be operated at a slow or fast speed.

Using an Arduino, a PID controller is built to manage the brake cylinder's pressure at the wheel. The brake pressure of each wheel is controlled by pulsing the solenoids to fill pressure to the wheels, checking the pressure, and then either increasing or decreasing based on the new pressure reading until the target pressure is reached. The PWM duty cycle of the solenoids is a function of the PID controller.

There is small "stop light switch" on the brake pedal. This switch controls the brake lights and communicates to the ECU that the brake pedal has been depressed. If the Kia Soul brakes without pressure being increased at the wheels and the brake pedal has not been depressed, the car throws a fault. Because of this it is necessary to emulate the stop light switch with our control unit.

# Resources & Further Reading

* [Actuator Pinout](images/brake_module/brake_actuator_pinout.png)
* [Actuator Pressure Sensor Response](docs/brake_module/pressure_sensor_output.ods)


# Additional Information
The brake actuator PolySync uses is Toyota part number 44510-47050. There is a similar actuator with a larger accumulator used in the 06-07 Highlander Hybrid and 06-08 Lexus RX400H, part number 44510-48060.
