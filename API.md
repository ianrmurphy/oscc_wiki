# OSCC API

OSCC's API is the entry point for your application's communication with various on-vehicle modules. By utilizing the provided functions, you can perform the following operations:

* Send control commands to the vehicle
* Receive reports on each OSCC module's state
* Obtain control messages from the vehicle's OBD-II CAN network
* Open and close CAN communications with the modules
* Enable and disable the system

The API uses a callback-based approach to provide asynchronous, interrupt-driven I/O, allowing you to decide how to handle new information after receiving reports. Using OSCC through the API also allows you to send commands to the vehicle while still ensuring that safe values are being calculated before being written to the various vehicle ECUs.
Working with the API is the **safest and most modular way** of using OSCC in your own software.

## Function Overview

**Open and close CAN channel to OSCC Control CAN.**

```c
oscc_result_t oscc_open( uint channel );
oscc_result_t oscc_close( uint channel );
```

These methods are the start and end points of using the OSCC API in your application. ```oscc_open``` will open a socket connection
on the specified CAN channel, enabling it to quickly receive reports from and send commands to the firmware modules.
When you are ready to terminate your application, ```oscc_close``` can terminate the connection.

**Enable and disable all OSCC modules.**

```c
oscc_result_t oscc_enable( void );
oscc_result_t oscc_disable( void );
```

After you have initialized your CAN connection to the firmware modules, these methods can be used to enable or disable the system. This
allows your application to choose when to enable sending commands to the firmware. Although you can only send commands when the system is
enabled, you can receive reports at any time.

**Publish control commands to the corresponding module.**

```c
oscc_result_t publish_brake_position( double normalized_position );
oscc_result_t publish_steering_torque( double normalized_torque );
oscc_result_t publish_throttle_position( double normalized_position );
```

These commands will forward a double value, *[0.0, 1.0]*, to the specified firmware module. The API will construct the appropriate values
to send spoof commands into the vehicle ECU's to achieve the desired state. The API also contains safety checks to ensure no invalid values
can be written onto the hardware.

**Register callback function to handle OSCC report and OBD messages.**

```c
oscc_result_t subscribe_to_brake_reports( void(*callback)(oscc_brake_report_s *report) );
oscc_result_t subscribe_to_steering_reports( void(*callback)(oscc_steering_report_s *report) );
oscc_result_t subscribe_to_throttle_reports( void(*callback)(oscc_throttle_report_s *report) );
oscc_result_t subscribe_to_fault_reports( void(*callback)(oscc_fault_report_s *report) );
oscc_result_t subscribe_to_obd_messages( void(*callback)(struct can_frame *frame) );
```

In order to receive reports from the modules, your application will need to register a callback handler with the OSCC API.
When the appropriate report for your callback function is received from the API's socket connection, it will then forward the
report to your software.

In addition to OSCC specific reports, it will also forward any non-OSCC reports to any callback function registered with
```subscribe_to_obd_messages```. This can be used to view CAN frames received from the vehicle's OBD-II CAN channel. If you know
the corresponding CAN frame's ID, you can parse reports sent from the car. Currently, we forward the following OBD messages:

* Steering wheel angle
* Wheel speeds
* Brake pressure

The details of the structure of these messages can be found in the vehicle-specific header files.

## Joystick commander example application

We've created an example application, Joystick Commander, that uses the OSCC API to interface with the firmware. This will allow you to send commands using a game controller, as well as receive reports from the OSCC modules and the on-board OBD-II. These commands are converted into CAN messages, which the OSCC API sends to the respective Arduino modules, and are used to actuate the vehicle. This example exercises all of the functions provided by the API. It shows how the `publish` methods can be used to control the vehicle, and how OBD messages and OSCC reports can be used to determine various parts of the vehicle's state. This application also demonstrates the type of filtering that you may need to do in order to smooth inputs being sent to the vehicle, preventing the car's ECUs from detecting discontinuities in signal and possibly entering a fault state.

[OSCC Joystick Commander](https://github.com/PolySync/oscc-joystick-commander)
