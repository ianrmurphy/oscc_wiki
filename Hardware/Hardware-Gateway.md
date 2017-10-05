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

### Resources & Further Reading

* [[Seeedstudio CAN Shield 1.2 Documentation|http://wiki.seeed.cc/CAN-BUS_Shield_V1.2/]]
* [[Making your own Twisted Pair|https://www.electronics-notes.com/articles/constructional_techniques/hints_and_tips/how-to-make-twisted-pair-wire.php]]
