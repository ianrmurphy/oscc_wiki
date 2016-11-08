Controlling a Kia Soul fully by wire requires the creation of three modules to interface with physical actuators, a CAN gateway module to translate Kia specific CAN messages onto a new **Control CAN bus**, and a power distribution system that has emergency stop capability. Each of these individual systems have their own page.

- [[Steering|steering]]
- [[Throttle|throttle]]
- [[Brake|brake]]
- [[CAN Gateway|CAN-Gateway-&-Control-CAN]]
- [[Power Distribution|power-distribution]]

These systems all work together to provide full control of the vehicle. The image below illustrates a high level overview of the systems needed to control a vehicle, as well a network of sensors used for perception.

[[https://github.com/PolySync/OSCC/blob/master/img/overview.png|alt=Overview]]




Below is a more detailed schematic of the control system. 
<a href="https://github.com/PolySync/OSCC/raw/master/img/system.png">[[https://github.com/PolySync/OSCC/raw/master/img/system.png|alt=Detailed]]</a>