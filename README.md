![TF Logo](images/tf-logo.png)

> If you'd like to order a prototype device, visit the [device order page](http://buy.trafficflow.codemotive.io/).  Please allow 3-4 weeks for shipping.  Join the OP Community Discord server for announcements and discussions regarding the TrafficFlow device [here](http://opdiscord.debugged.me).

## What is TrafficFlow?

TrafficFlow is a device that provides lateral and longitudinal control to FCA vehicles on the Powernet architecture.  The device is easy to install, no splicing is required.  TrafficFlow can apply engine torque, braking torque, and full range steering wheel control at any speed.

Control requests are sent to the TrafficFlow device over the vehicle's own CAN bus, or over a serial connection over USB.  The DBC file which defines
the message formats for communication over CAN can be found [right here](docs/trafficflow.dbc).

This device is designed to be used with OpenPilot to remove all limitations that are currently present with FCA vehicles.  However, the device contains no dependency to OpenPilot and can be used for any purpose. 

## Which vehicles does TrafficFlow support?

Presently, FCA vehicles using the Powernet architecture, equipped with LaneSense and Adaptive Cruise, are supported:

- 2015+ Chrysler 300
- 2014+ Jeep Grand Cherokee
- 2017+ Chrysler Pacifica
- 2011+ Dodge Charger
- 2015+ Dodge Challenger

> **Note:** The model years listed above are not accurate - I will update after I am certain. 

Support is planned for the CUSW archiecture which would expand support the Jeep Cherokee (non-Grand).

## What features will I get with this device and OpenPilot?

- Ability for OpenPilot to longitudally control the vehicle, even bringing it to a standstill for stop-and-go scenarios.
- Lifts the steering torque limit that prevents OpenPilot from making it around some curves without driver input.
- TrafficFlow is able to provide information and control many other things to make many new features possible.  For example, TF can report blind spot monitoring information and control the turn signals to allow for a fully automated lane change feature.  
- Updating the device's firmware is easy with the TrafficFlow Configurator for Mac (Windows version is being worked on), as I'll be continuously working on adding features and fixing bugs.  For fun and testing, you can drive your car with a game controller through the Configurator app.
- Includes some extra, neat little features, such as Stealth Mode and Light Show. More information to come on that later...

## What does it look like?

This is the current prototype unit.

![TF Device Photo](images/tf-device-photo-small.jpg)
