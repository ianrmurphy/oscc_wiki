# Background

The Kia Soul has a handful of different CAN buses on board. The OBD-II CAN network has vehicle state information such as steering wheel angle, wheel speeds, and brake pressure. This information is useful for algorithms like PID control and path planning.

Rather than sharing the vehicle's OBD-II bus and possibly interfering with the vehicle's native messages, OSCC has its own CAN bus called Control CAN where commands and reports are sent and received.

The CAN Gateway acts as a bridge between the vehicle's native OBD-II bus and Control CAN, forwarding relevant OBD-II messages from the OBD-II bus to the Control CAN bus, which can be consumed by applications subscribing to the OBD messages.

The image below demonstrates how the CAN gateway sits on both the Control CAN bus and the OBD-II CAN bus, but only publishes CAN messages in one direction: towards the Control CAN bus from the OBD-II CAN bus.

![CAN Gateway](/images/can/can_gateway_diagram.png)

There is a CAN specification in each module's firmware page:

* [[Brake|Firmware-Brake]]
* [[Steering|Firmware-Steering]]
* [[Throttle|Firmware-Throttle]]
* [[OBD-II|Firmware-OBD]]

# Hardware
### Parts

| Part          | Price  |  Quantity |
| ------------- | -----:|  -----:|
| [[Arduino Uno|https://www.seeedstudio.com/CANBUS-Shield-V12-p-2256.html]]      | $24.95 | 1 |
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
* [[Making your own Twisted Pair|https://www.electronics-notes.com/articles/constructional_techniques/hints_and_tips/how-to-make-twisted-pair-wire.php]]
