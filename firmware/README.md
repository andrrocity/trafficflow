# Device Firmware

This folder contains device firmware releases should you need to manually flash your device or flash older versions.

## Manual Flashing from Unix

If you have a Mac, you can use the TrafficFlow Configurator app to automatically update your device. If you need to flash manually, follow these steps:

1. Download the firmware .bin file you want to flash.
2. Install the `bossac` program if you don't already have it using your package manager.  If you use `apt` you can install it by running `apt-get install bossa-cli`.
3. Plug the device into the primary USB port.  If you have problems flashing over the primary USB port, you may have better luck trying these same steps while plugged into the debug USB port.
4. Find the device's path in your `/dev` folder, it should start with `cu.usbmodem*`. 
4. Run the following commands:

```bash
stty -f $DEVICE_PATH speed 1200
bossac --port=$DEVICE_PATH --write --verify --reset $FIRMWARE_FILE --boot 
```

Variables `$DEVICE_PATH` and `$FIRMWARE_FILE` defined or replaced as necessary. When complete, the amber status LED should be pulsing.  If its not, replug the device.  If you still don't get an pulsing LED, something went wrong - try flashing again.
