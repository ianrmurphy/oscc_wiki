# OSCC API

OSCC's API is the entry point for your application's communication with the various on-vehicle modules. You can utilize the provided functions to perform the following operations:

* sending control commands to the vehicle
* receiving reports on each OSCC module's state
* obtaining control messages from the vehicle's OBD-II CAN network
* opening and closing CAN communications with the modules
* enabling and disabling the system

The API uses a callback-based approach to provide asynchronous, interrupt-driven I/O, enabling you to decide how to handle new information upon receiving reports. Using OSCC through the API also allows you to send commands to the vehicle while still ensuring that safe values are being calculated before being written to the various vehicle ECUs.
Working with the API is the safest and most modular way of using OSCC in your own software.

## Function Overview

**Use provided CAN channel to open and close communications to CAN bus connected to the OSCC modules.**

```c
oscc_result_t oscc_open( uint channel );
oscc_result_t oscc_close( uint );
```

These methods are the start and end points of using the OSCC API in your application, and must be called to facilitate communication with the firmware modules. `oscc_open` will open a socket connection on the specified CAN channel, and when you are ready to terminate your application, you can close the socket by calling `oscc_close`.

**Send enable or disable commands to all OSCC modules.**

```c
oscc_result_t oscc_enable( void );
oscc_result_t oscc_disable( void );
```

After you have initialized your CAN connection to the firmware modules, these methods can be used to enable or disable the system. This allows your application to choose when to enable sending commands to the firmware. Although you can only send commands when the system is enabled, you can receive reports at any time.

**Publish message with requested normalized value to the corresponding module.**

```c
oscc_result_t publish_brake_position( double normalized_position );
oscc_result_t publish_steering_torque( double normalized_torque );
oscc_result_t publish_throttle_position( double normalized_position );
```

These commands will forward a double value, between [0.0, 1.0] for brake and throttle and [-1.0, 1.0] for steering, to the specified firmware module. The API will then use these values to calculate the voltages needed to send to the vehicle's ECUs and achieve the desired new state. The specifics of how these spoof values are calculated can be found in macros defined in vehicle-specific header files, such as `vehicles/kia_soul.h`.
The API also contains safety checks to ensure no invalid values can be written onto the hardware.

**Register callback function to be called when OBD message received from vehicle.**

```c
oscc_result_t subscribe_to_brake_reports( void(*callback)(oscc_brake_report_s *report)  );
oscc_result_t subscribe_to_steering_reports( void(*callback)(oscc_steering_report_s *report) );
oscc_result_t subscribe_to_throttle_reports( void(*callback)(oscc_throttle_report_s *report) );
oscc_result_t subscribe_to_fault_reports( void(*callback)(oscc_fault_report_s *report) );
oscc_result_t subscribe_to_obd_messages( void(*callback)(struct can_frame *frame) );
```

In order to receive reports from the modules, your application will need to register a callback handler with the OSCC API.
When the appropriate report for your callback function is received from the API's socket connection, it will then forward it to your software.

In addition to OSCC specific reports, it will also forward any non-OSCC reports to any callback function registered with
`subscribe_to_obd_messages`. This can be used to view CAN frames received from the vehicle's OBD-II CAN channel. If you know
the corresponding CAN frame's id, you can parse reports sent from the car. Currently, we forward the following OBD messages:

* steering wheel angle
* wheel speeds
* brake pressure

The details of the structure of these messages can be found in the vehicle-specific header files.

## Joystick commander example application

We've created an example application, joystick commander, that uses the OSCC API to interface with the firmware, allowing you to send commands using a game controller and receive reports from the OSCC modules and the on-board OBD-II. These commands are converted into CAN messages, which the OSCC API sends to the respective Arduino modules and are used to actuate the vehicle. This example exercises all of the functions provided by the API, showing how the `publish` methods can be used to control the vehicle and how OBD messages and OSCC reports can be used to determine various parts of the vehicle's state. This application also demonstrates the type of filtering that you may need to do in order to smooth inputs being sent to the vehicle, preventing the car's ECUs from detecting discontinuities in signal and possibly entering a fault state.

[OSCC Joystick Commander](https://github.com/PolySync/oscc-joystick-commander)
