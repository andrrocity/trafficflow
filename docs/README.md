# TrafficFlow Documentation

## LED Patterns

|  Pattern 	| Name      	|  Speed 	| Effect 	| Indication                                                                                                	|
|:--------:	|-----------	|:------:	|:------:	|-----------------------------------------------------------------------------------------------------------	|
|   None   	| None      	|  None  	|  None  	| If LED is stuck not changing at all, the device has crashed.                                              	|
|    OX    	| Blink     	|  Fast  	|  None  	| Device just booted up, sleeping for 1 second before continuing.                                           	|
|    OX    	| Blink     	|  Fast  	|  Fade  	| Starting device serial USB interface. Discovering and initializing CAN controllers. Starting application. 	|
|    O.X.  	| Blink     	|  Slow  	|  Fade  	| Module discovery in progress.                                                                             	|
| OX.OX... 	| Heartbeat 	| Normal 	|  Fade  	| Module discovery complete. Device is active.                                                              	|
| OX.OX... 	| Heartbeat 	| Normal 	|  None  	| Module discovery complete. Device is inactive (i.e. car ignition is off).                                 	|

## Control via USB Serial 

The protocol for communicating with the device over USB is documented [here](serial-interface-spec.md).

## Control via CAN

In this folder you will find the DBC file defining the messages TrafficFlow uses over CAN to control the vehicle.  The device accepts messages on any channel.