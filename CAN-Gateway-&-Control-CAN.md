# Background

The Kia Soul has a handful of different CAN buses on board. The [[C-CAN bus|can-frames]] has vehicle state information such as steering wheel angle, wheel speeds, and brake pressure. This information needs to be available on the Control CAN bus because it is used in low level PID control for steering angle and can be useful in high level control, such as path planning.

Additionally, it’s not a great idea to simply extend the C-CAN bus by adding new control modules onto the bus. This is because the new control system will be publishing CAN messages using CAN frames that may interfere with the existing C-CAN protocol. Because of this it is necessary to bridge C-CAN messages to the new Control CAN bus.


The Control CAN bus is the CAN bus that is added to the vehicle in order to link all the new control modules, as well as send control commands to those modules. 

The image below demonstrates how the CAN gateway sits on both the Control CAN bus and the C-CAN bus, but only publishes CAN messages in one direction: towards the Control CAN bus from the C-CAN bus.

[[https://github.com/PolySync/SelfDrivingSoul/blob/master/img/CAN_gateway.png|alt=CAN Gateway]] 

A [[CAN specification|CAN-specification]] exists for the Control CAN bus.

# Hardware
### Parts

| Part          | Price  |  Quantity |
| ------------- | -----:|  -----:|
| [[Aruino Uno|https://www.seeedstudio.com/CANBUS-Shield-V12-p-2256.html]]      | $24.95 | 1 |
| [[Seeed Studio CAN-BUS Shield, v1.2|https://www.seeedstudio.com/CANBUS-Shield-V12-p-2256.html]]      | $23.50 | 2 |
| [[ScanTool OBD Interface Cable|https://www.amazon.com/ScanTool-143301-J1962M-Female-Interface/dp/B0091QW67O/ref=sr_1_8?ie=UTF8&qid=1476389497&sr=8-8&keywords=OBD+cable]] | $6.89 | 1 |
| 22 AWG Stranded Twisted Pair Wire |  | 30'
| [[Solder Cup DB-9 Connector|https://www.amazon.com/Pc-Accessories-Connectors-Plastic-Connector/dp/B01HM6UBN0/ref=sr_1_4?ie=UTF8&qid=1476943955&sr=8-4&keywords=solder+DB-9+connector]] | $11.98 | 1 |

### Assembly
1. Print [[the enclosure|https://github.com/PolySync/OSCC/blob/master/3d_models/dash_enclosure/STC_housing.STL]] if you haven't yet.
2. Modify the SPI chip, select pin on one of the shields.
 - The Seeedstudio CAN shields have an SPI chip select pin pad on the board (default pin 9). Because two boards will be used simultaneously, one of the boards needs to be modified to use another pin (pin 10). The pad can be cut as described in the [[CAN shield documentation|http://wiki.seeed.cc/CAN-BUS_Shield_V1.2/]]. 
3. Stack the shields and Arduino Uno and then place them in an enclosure.
4. Upload the firmware from the repo.

### Install

The CAN Gateway module is a simple install. One of the Arduino CAN shields needs to be connected to the Kia C-CAN bus, which is available through two pins on the OBD connector. You will need to run twisted pair wire for the Control CAN bus.

1. Use the OBD cable to connect the car to the designated C-CAN shield of the module.
2. Connect the other CAN shield to the Control CAN Bus.
3. Run power for the module from the non E-stop power.
4. Run twisted pair to each control module and an additional length of twisted pair terminated with a DB-9 connector for connecting to a CAN interface.

### Resources & Further Reading

* [[Seeedstudio CAN Shield 1.2 Documentation|http://wiki.seeed.cc/CAN-BUS_Shield_V1.2/]]
* [[Kia CAN Protocol|Can-Frames]]
* [[Control CAN Specification|CAN-specification]]
* [[Making you own Twisted Pair|https://www.electronics-notes.com/articles/constructional_techniques/hints_and_tips/how-to-make-twisted-pair-wire.php]]
