![Logo](https://raw.githubusercontent.com/wiki/PolySync/OSCC/images/oscc_logo_title.png)

# Introduction

![Driving](https://raw.githubusercontent.com/wiki/PolySync/OSCC/images/driving.gif)


Open Source Car Control (OSCC) provides developers with a collection of software and hardware solutions for computer control of their autonomous development vehicles. It's a modular, stable way of using software to interface with a vehicle’s communications network and control systems.

OSCC allows developers to:
* Send control commands to the vehicle
* Read control messages from the vehicle’s OBD-II CAN network
* Forward reports for current vehicle control states (e.g. steering angle & wheel speeds) 

Control commands are issued to the vehicle component ECUs via the steering wheel torque sensor, throttle position sensor, and brake position sensor. (Because the gas-powered Kia Soul isn’t brake by-wire capable, an auxiliary actuator is added to enable braking.) This low-level interface means that OSCC offers full-range control of the vehicle without altering the factory safety-case, spoofing CAN messages, or hacking ADAS features.

Although we currently support only the 2014+ Kia Soul (w/ Kia Soul EV coming soon), the API and firmware have been designed to make it easy to add functionality to any by-wire, OBD-II capable vehicle. Additionally, the separation between API and firmware means it's easier to modify and test parts of your program without having to update the flashed OSCC modules.
