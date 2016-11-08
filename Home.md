<img src="https://github.com/PolySync/SelfDrivingSoul/blob/master/img/driving.gif" align=right>

The Open Source Car Control (OSCC) project was dreamt up to help lower the steep barrier of entry for an individual to get their head and hands into building an autonomous vehicle. Other by-wire vehicle platforms (components + vehicle) can cost upwards of $140,000, and are “black boxed,” preventing further investigation and innovation into autonomy. We figured out a way to offer a more affordable, open-source option to the public. By using tools and parts common across robotics and automotive, in addition to our modules and software you've built, you can build a self-driving car for under $15,000. 
This wiki will guide you through the process, acting as the main source of documentation for developers and engineers working with (or contributing to) the Open Source Car Control (OSCC) project. The goal of this wiki is to house and present all of the information you need to modify a Kia Soul (or similar vehicle) for full by-wire control.  

# Introduction

The 2014 Kia Soul ships with **steering-by-wire** and **throttle-by-wire**. This means the actuators of these two systems are, at some point, controlled electronically. Because of this, these systems can be exploited in order to gain full control of the actuators. 

When considering brake control in the Soul, it is important to note that these vehicles don’t have electronically controlled brakes. As a solution, you can add a brake-by-wire system found on some hybrid vehicles. The actuators from these hybrid vehicles can be added in-line to the Kia brake system in order to control brake pressure.

To achieve lateral and longitudinal control of the Kia Soul it is necessary to control three separate automotive systems, interface with the existing vehicle CAN bus, and power the additional microprocessors and actuators. Each of the control modules introduced into the vehicle are built around Arduino controllers. Arduinos provide a cheap and easy way to introduce embedded controllers into the system, and offer flexibility when controlling vehicles other than the Kia Soul.

A new network of controllers will be created that communicate via a CAN bus, called the **Control CAN bus**. Control commands can be published to this bus in the form of CAN frames from a node executing a path planning algorithm or a simple game controller.

PolySync is working on creating Arduino shields that provide all the function needed for interfacing with the steering, throttle, and brake system. Check back soon for schematics and board layouts! 



### - [[Overview|overview]]
### - [[Steering|steering]]
### - [[Throttle|throttle]]
### - [[Brake|brake]]
### - [[CAN Gateway & Control CAN|CAN-Gateway-&-Control-CAN]]
### - [[Power Distribution|power-distribution]]
