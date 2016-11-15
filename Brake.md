# Background

As braking in the Kia Soul is a traditional mechanical system, the factory standard Soul has no ability to control braking electronically. There are a number of models of vehicles with electronically controlled brake systems, notably the 2004-2009 Prius. This model Prius uses an electronically controlled actuator with no microprocessor, it is controlled from the Prius ECU. There are 7 pressure sensors on the device, 10 proportional solenoids, an accumulator, a pump, diagnostics components, and a pressure relief valve. This unit can be sourced from auto salvage yards and installed into the existing Kia brake system without adversely effecting the stock brake system and adding by-wire control. When sourcing these units be sure to request the pigtail adapter for the actuator as well.

[[https://github.com/PolySync/OSCC/blob/master/assets/actuator.png|alt=Prius Brake Setup]]

The image below illustrates the brake actuator as it is installed in a Prius. Notice you can see which solenoids are normally open and which are normally closed.

[[https://github.com/PolySync/OSCC/blob/master/assets/PriusBrakeActuator.png|alt=Prius Brake Setup]]

# Control

The Prius brake actuator is a simple but powerful device. It is useful to first understand how the device works when it is installed in a Prius.

The brake actuator has a pump and an accumulator. When the pump is energized, pressure is built up in the accumulator. This pressure can then be distributed to 4 hydraulic lines, each of which can be controlled independently using two proportional solenoids: one to fill pressure and one to spill pressure. Using pulse width modulation (PWM), the amount that the solenoids open or close can be varied. Each of these four hydraulic lines has a pressure sensor for measuring brake pressure at the wheels.

When a Prius is powered on two master cylinder lockout solenoids that are closed, it prevents fluid from passing from the master cylinder to the brakes. The sensation of fluid passing from the master cylinder to brakes is emulated by a stroke simulator. Pressure in the master cylinder is measured and braking is then done by the regenerative braking system.

Certain solenoids are normally closed while others are normally open. The solenoid system is designed in such a way that in the event of power failure the brake pressure at the rear wheels is spilled entirely, and the pressure at the front wheels is regulated by driver (the master cylinder lockout solenoids open). **As the actuator is installed in a Prius, without power supplied to the actuator fluid passes through the device to the front wheels only, no fluid passes through to the the rear wheels.** In order to avoid this behavior in the Kia Soul only the front two lines of the actuator unit are used and the rear two lines are plugged. If actuator is not powered, fluid will pass through it from the master cylinder to all four brake lines on the vehicle.

In an autonomous situation (where a driver is not using the brake pedal) the 2 master cylinder solenoids will be closed, preventing brake pressure at the the wheels spilling back into the master cylinder. There are two master cylinder pressure sensors that can be used to detect pressure at the pedal.

The accumulator has a pressure sensor for detecting the pressure of the fluid built up in the accumulator. The accumulator pump is a high current (~30 Amps) pump and can be operated at a slow or fast speed.

Using an Arduino, a PID controller is built to manage the brake cylinder's pressure at the wheel. The brake pressure of each wheel is controlled by pulsing the solenoids to fill pressure to the wheels, checking the pressure, and then either increasing or decreasing based on the new pressure reading until the target pressure is reached. The PWM duty cycle of the solenoids is a function of the PID controller. 

There is small "stop light switch" on the brake pedal. This switch controls the brake lights and communicates to the ECU that the brake pedal has been depressed. If the Kia Soul brakes without pressure being increased at the wheels and the brake pedal has not been depressed, the car throws a fault. Because of this it is necessary to emulate the stop light switch with our control unit.

# Hardware

The new PolySync brake shield is undergoing testing and validation. Board designs and schematics will be available as soon at the boards are thoroughly proven.

### Parts

| Part          | Price  |
| ------------- | -----:|
| 04-09 Prius Brake actuator with wiring pigtail (Junkyard or eBay)     | ~$200.00 |
| [[Brake Fluid Reservoir| https://www.amazon.com/Ninth-City-Motorcycle-Master-Cylinder-Reservoir/dp/B015NWTBF4/ref=sr_1_9?ie=UTF8&qid=1476400828&sr=8-9&keywords=brake+reservoir]]      | ~$5.99 |
| [[Arduino Uno|https://www.seeedstudio.com/CANBUS-Shield-V12-p-2256.html]]      | $24.95 |
| [[PolySync Brake Shield R0|https://www.oscc.io]]      | $649 (purchased with complete shield kit) |


### Assembly

The brake actuator pigtail must be wired to the control board. There are 38 wires on this connector and it helps to identify each one. The documents linked at the bottom of the page can help identify the wires. It is important to note that 21 of the wires are used and the rest are unused. You will also need to add a 4 pin connector to connect to the interrupt wires, which will be spliced into the stop light switch harness. 

The actuator wiring pigtail has many leads and can be confusing at first glance. Use the pinout diagram linked at the bottom of the page to identify every lead on the pigtail. While some wires colors are used more than once, you can use the groups of wires or the pin numbers to deduce which lead is which. 

1. Print [[the enclosure|https://github.com/PolySync/OSCC/tree/master/3d_models/brake_enclosure]] and enclosure lid.
2. Screw the Arduino Uno to the enclosure.
3. Press the brake control board onto the Arduino.
4. Lengthen the leads on the pigtail if necessary.
5. Feed the wires from the pigtail through the enclosure and attach them to the board using the screw terminals.
6. Run four wires through the enclosure and attach them to stop light switch screw terminals.

### Install

The actuator is mounted in the engine compartment of the vehicle. The brake lines from the master cylinder are routed to the two inlets on the brake actuator. Two of the outlets (rear left and rear right) on the actuator are capped off and the two remaining outlets (front left and front right) are routed to the two inlets on the Kia ABS unit.

The images below show the brake actuator as it is installed in a Kia Soul. You can see the enclosure for the micro controller as well as the added brake fluid reservoir.


[[https://github.com/PolySync/SelfDrivingSoul/blob/master/img/new_brake_setup.png|alt=New Brake Setup]]


[[https://github.com/PolySync/SelfDrivingSoul/blob/master/img/actuator_install.png|alt=Installed Actuator]]

1. A brake fluid reservoir, connected with a T into the existing brake reservoir hose, will need to be mounted above the intake of the brake actuator unless the brake actuator can be installed below the level of brake fluid in the existing reservoir. It might be possible to mount the brake actuator below the stock brake fluid reservoir by moving the battery to the rear of the car and mount the brake actuator in the place of the battery.
2. The brake module is mounted in the engine bay next to the brake actuator, close enough to connect the pigtail connector to the brake module. 
3. The wiring harness for the stop light switch is spliced into. Run wires from the splice through the firewall and up to the brake module and add a 4 pin connector. 
4. The power for the unit is connected to the emergency stoppable power system.
5. The CAN shield is wired to the Control CAN bus.
6. The brakes will need to be bled after the actuator is installed. Luckily, the actuator can supply the pressure needed to bleed the brake system. Connect a computer to the control module via a USB cable and issue a command to increase the pressure. Then bleed the brakes in a pattern specified by the vehicle manufacturer. Add additional brake fluid if necessary. 

# Resources & Further Reading

* [[Actuator Pinout |https://github.com/PolySync/OSCC/blob/master/vehicle_info/toyota_prius_xw20/brake/brake_actuator_pinout.png]]
* [[Actuator Pressure Sensor Response|https://github.com/PolySync/OSCC/blob/master/vehicle_info/toyota_prius_xw20/brake/pressure_sensor_output.ods]]


# Additional Information
The brake actuator PolySync uses is Toyota part number 44510-47050. There is a similar actuator with a larger accumulator used in the 06-07 Highlander Hybrid and 06-08 Lexus RX400H, part number 44510-48060.
