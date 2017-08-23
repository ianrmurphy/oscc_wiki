# Hardware

The 2014 Kia Soul ships with **steering-by-wire** and **throttle-by-wire**. This means the actuators of these two systems are, at some point, controlled electronically. Because of this, these systems can be exploited in order to gain full control of the actuators.

When considering brake control in the Soul, it is important to note that this vehicle does not have electronically controlled brakes. As a solution, you can add a brake-by-wire system found on some hybrid vehicles. The actuators from these hybrid vehicles can be added in-line to the Kia brake system in order to control brake pressure.

To achieve lateral and longitudinal control of the Kia Soul it is necessary to control three separate automotive systems, interface with the existing **Vehicle CAN bus**, and power the additional microprocessors and actuators. Each of the control modules introduced into the vehicle are built around Arduino controllers. Arduino controllers provide a cheap and easy way to introduce embedded controllers into the system, and offer flexibility when controlling vehicles other than the Kia Soul.

A new network of controllers will be created that communicate via a separate CAN bus, called the **Control CAN bus**. Control commands can be published to this bus in the form of CAN frames from a node executing a path planning algorithm or a simple game controller. An example of the latter is included in the [[Joystick Commander|https://github.com/PolySync/oscc-joystick-commander/]] example application.

Controlling a Kia Soul fully by wire requires the creation of three modules to interface with physical actuators, a CAN gateway module to translate Kia specific CAN messages onto a new **Control CAN bus**, and a power distribution system that has emergency stop capability. Each of these individual systems have their own page at right.

![Overview](images/system/system_overview.png)

These systems all work together to provide full control of the vehicle. The image above illustrates a high level overview of the systems needed to control a vehicle, as well as a network of sensors used for perception.