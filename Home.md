![Logo](https://raw.githubusercontent.com/wiki/PolySync/OSCC/images/oscc_logo_title.png)

# Introduction

![Driving](https://raw.githubusercontent.com/wiki/PolySync/OSCC/images/driving.gif)


Open Source Car Control (OSCC) is an assemblage of software and hardware designs that enable computer control of modern cars in order to facilitate the development of autonomous vehicle technology. It is a modular and stable way of using software to interface with a vehicle’s communications network and control systems.

OSCC enables developers to send control commands to the vehicle, read control messages from the vehicle’s OBD-II CAN network, and forward reports for current vehicle control state. Such as steering angle & wheel speeds. Control commands are issued to the vehicle component ECUs via the steering wheel torque sensor, throttle position sensor, and brake position sensor. (Because the gas-powered Kia Soul isn’t brake by-wire capable, an auxiliary actuator is added to enable braking.) This low-level interface means that OSCC offers full-range control of the vehicle without altering the factory safety-case, spoofing CAN messages, or hacking ADAS features.

Although we currently support only the 2014 or later Kia Soul (w/ Kia Soul EV & Kia Niro support coming in Q3/Q4 2017, respectively), the API and firmware have been designed to make it easy to add new vehicle support. Additionally, the separation between API and firmware means it is easier to modify and test parts of your program without having to update the flashed OSCC modules.


## - [[API|API]]
## - [[Hardware|Hardware]]
## - [[Firmware|Firmware]]
