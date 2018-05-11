![Logo](https://raw.githubusercontent.com/wiki/PolySync/OSCC/images/oscc_logo_title.png)

# Introduction

The Open Source Car Control (OSCC) project was created to give everyone the opportunity to build their own development autonomous vehicle. Other by-wire vehicle platforms (components + vehicle) can cost upwards of $140,000, and are “black boxed,” preventing further investigation and innovation into autonomy. We decided to offer a more affordable, open-source option to developers. By using tools and parts common across robotics and automotive, the software of your choice, and some elbow grease you can build an autonomous development vehicle at a significantly lower cost.

This wiki will guide you through the process, acting as the main source of documentation for developers and engineers working with (or contributing to) the Open Source Car Control (OSCC) project. The goal of this wiki is to house and present all of the information you need to modify a Kia Soul (or similar vehicle) for full by-wire control.

Open Source Car Control (OSCC) provides developers with a collection of firmware and hardware designs for computer control of their autonomous development vehicles. It's a modular, stable way of using software to interface with a vehicle’s communications network and control systems.

OSCC allows developers to:

* Send control commands to the vehicle
* Read control messages from the vehicle’s OBD-II CAN network
* Forward reports for current vehicle control states (e.g. steering angle & wheel speeds) 

Control commands are issued to the vehicle component ECUs via the steering wheel torque sensor, throttle position sensor, and brake position sensor. (Because the petrol Kia Soul isn’t brake by-wire capable, an auxiliary actuator is added to enable braking.) This low-level interface means that OSCC offers full-range control of the vehicle without altering the factory safety-case, spoofing CAN messages, or hacking ADAS features.

Although we currently support only the 2014+ Kia Soul (conventional) and Kia Soul EV, the API and firmware have been designed to make it relatively easy to add functionality to any by-wire, OBD-II capable vehicle. Additionally, the separation between API and firmware means it's easier to modify and test parts of your program without having to update the flashed OSCC modules.

## DriveKit
If you're looking for a turn-key solution, we offer a commercially-supported vehicle control interface called DriveKit. DriveKit is a complete system for by-wire control of Kia Soul EV and Kia Niro Hybrid vehicles. It includes an integrated control module, automotive-grade wiring harnesses, e-stop switch, and additional vehicle control features –– plus it installs in under an hour. Visit https://polysync.io/drivekit/ for more information.

