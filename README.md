# TrafficFlow

## What is TrafficFlow?

TrafficFlow is a device that provides vehicle control to supported vehicles.  Communication with the TrafficFlow device uses a protocol that is designed to be highly abstract and vehicle agnostic. I like to call it the "Vehicle Control Interface Standard".

## Which vehicles support TrafficFlow?

As of now, only FCA vehicles on Powernet architecture are supported.  Support for the CUSW archiecture is planned.

## How will this help OpenPilot?

Currently, OpenPilot must handle the peculiarities of vehicle control per manufacturer.  While that may work just fine for some vehicles, others require a more involved setup that can't be acheived without specialized hardware.  Once the *Vehicle Control Interface Standard* (or *VCIS*) is implemented into OpenPilot, that is what will be used to control the vehicle. 

Because this will allow OpenPilot to control any vehicle adopting this standard, it could theortetically be used to control more than just cars, but four-wheelers, go-karts, or motorcyles, for example, without requiring any code changes.