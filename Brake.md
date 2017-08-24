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

### Parts

| Part          | Price  |
| ------------- | -----:|
| 04-09 Prius Brake actuator with wiring pigtail (Junkyard or eBay)     | ~$200.00 |
| [[Brake Fluid Reservoir|http://www.jegs.com/i/Wilwood/950/260-10500/10002/-1]]      | $32.94 |
| [[3/8" brake fluid hose (7')|https://www.pegasusautoracing.com/productselection.asp?Product=3290&utm_source=google&utm_medium=cpc&utm_campaign=3290&gclid=CJy0poynuNECFcdhfgodJZwLPA ]]      | $5.29 ft.|
| [[M12x1.0 to AN -3 male-to-male adapter (4)|https://www.amazon.com/Autobahn88-Stainless-Steel-Fitting-Adapter/dp/B019RVZZ16]]  |$8.99 ea. |
| [[M10x1.0 to AN -3 male-to-male adapter (6)|http://www.jegs.com/i/Russell/799/641431/10002/-1]]|$14.99 ea.|
| [[AN -3 Cap (2)|https://www.summitracing.com/parts/aer-fbm3479/overview/1]]|$18.97 (for 6)|
| [[26" AN -3 brake line 90 degree/straight (2)|http://www.jegs.com/i/Earls/361/63010126/10002/-1]]|$15.84 ea.|
| [[48" AN -3 brake line 90 degree/straight (2)|http://www.jegs.com/i/Earls/361/63010148/10002/-1]]|$21.36 ea.|
| [[90 degree AN -3 male-to-female adapter (4)|http://www.jegs.com/i/Earls/361/966303/10002/-1]]|$6.58 ea. |
| [[3/8" barbed brass tee|http://www.jegs.com/i/Dorman-Products/326/788-031/10002/-1]]| $3.68 ea. |
| [[Arduino Mega|https://www.arduino.cc/en/Main/arduinoBoardMega]] | $24.95 |
| [[PolySync Brake Shield R0|https://www.oscc.io]]      | TBD |

# Resources & Further Reading

* [Actuator Pinout](images/brake_module/brake_actuator_pinout.png)
* [Actuator Pressure Sensor Response](docs/brake_module/pressure_sensor_output.ods)


# Additional Information
The brake actuator PolySync uses is Toyota part number 44510-47050. There is a similar actuator with a larger accumulator used in the 06-07 Highlander Hybrid and 06-08 Lexus RX400H, part number 44510-48060.
