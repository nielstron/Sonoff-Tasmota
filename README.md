# Sonoff-Tasmota and jedi 2in1 light

## Fork-Disclaimer

This Fork is a version of the tasmota Software that enables the use of the [jedi 2in1 light](http://www.jedi-light.com/de_DE/page/2-in-1) with the Sonoff device (tested only on the sonoff basic).

For this the chip remembers when it was turned off last and whether it is on, so that it can conclude whether the light currently is in warm white or cold white mode. It posts this state per MQTT to `stat/device_name/CTEMP` in [mireds](https://en.wikipedia.org/wiki/Mired) so that it works with [Home Assistant](https://home-assistant.io/). Commands to change the light mode can be sent to `cmnd/device_name/CTEMP` (not hardcoded, this is adjusted to your settings). The transmitted color is automatically replaced by the nearest of the two possible light states (238 and 370). The light will be turned on and off accordingly with the next MQTT power on command.

It is adviced to set button option 11 to 1 (`SetOption11 1`) as that way the user can control the light color manually via the button of the Sonos Device (by turning the lamp on and off again). To do this via MQTT rely on the toggle function, as when sending power on commands, the light color will be enforced to be equal to the color set in the CTEMP channel.

An example Home Assistant configuration could look like this:

```
light:
  - command_topic: cmnd/basement_light/POWER
    name: "Basement light"
    payload_off: "OFF"
    payload_on: "ON"
    platform: mqtt
    qos: 1
    retain: true
    state_topic: stat/basement_light/POWER
    color_temp_command_topic: cmnd/basement_light/CTEMP 
    color_temp_state_topic: stat/basement_light/CTEMP 
  
```
What follows is the original Tasmota device description

## Sonoff-Tasmota

Provide ESP8266 based Sonoff by [iTead Studio](https://www.itead.cc/) and ElectroDragon IoT Relay with Serial, Web and MQTT control allowing 'Over the Air' or OTA firmware updates using Arduino IDE.

Current version is **5.8.0a** - See [sonoff/_releasenotes.ino](https://github.com/arendst/Sonoff-Tasmota/blob/development/sonoff/_releasenotes.ino) for change information.

### ATTENTION All versions

Only Flash Mode DOUT is supported. Do not use Flash Mode DIO / QIO / QOUT as it might seem to brick your device.

See [Wiki](https://github.com/arendst/Sonoff-Tasmota/wiki/Theo's-Tasmota-Tips) for background information.

### ATTENTION Version 5 and up

These versions use a new linker script to free flash memory for future code additions. It moves the settings from Spiffs to Eeprom. If you compile your own firmware download the new linker to your IDE or Platformio base folder. See [Wiki > Prerequisite](https://github.com/arendst/Sonoff-Tasmota/wiki/Prerequisite).

Best practice to implement is:
- Open the webpage to your device
- Perform option ``Backup Configuration``
- Upgrade new firmware using ``Firmware upgrade``
- If configuration conversion fails keep the webpage open and perform ``Restore Configuration``

You should now have a device with 32k more code memory to play with.

### Version Information

- Sonoff-Tasmota provides all (Sonoff) modules in one file and starts with module Sonoff Basic.
- Once uploaded select module using the configuration webpage or the commands ```Modules``` and ```Module```.
- After reboot select config menu again or use commands ```GPIOs``` and ```GPIO``` to change GPIO with desired sensor.

<img src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoffbasic.jpg" width="250" align="right" />

See [Wiki](https://github.com/arendst/Sonoff-Tasmota/wiki) for more information.<br />
See [Community](https://groups.google.com/d/forum/sonoffusers) for forum and more user experience.

The following devices are supported:
- [iTead Sonoff Basic](http://sonoff.itead.cc/en/products/sonoff/sonoff-basic)
- [iTead Sonoff RF](http://sonoff.itead.cc/en/products/sonoff/sonoff-rf)
- [iTead Sonoff SV](https://www.itead.cc/sonoff-sv.html)<img src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoff_th.jpg" width="250" align="right" />
- [iTead Sonoff TH10/TH16 with temperature sensor](http://sonoff.itead.cc/en/products/sonoff/sonoff-th)
- [iTead Sonoff Dual](http://sonoff.itead.cc/en/products/sonoff/sonoff-dual)
- [iTead Sonoff Pow](http://sonoff.itead.cc/en/products/sonoff/sonoff-pow)
- [iTead Sonoff 4CH](http://sonoff.itead.cc/en/products/sonoff/sonoff-4ch)
- [iTead Sonoff 4CH Pro](http://sonoff.itead.cc/en/products/sonoff/sonoff-4ch-pro)
- [iTead S20 Smart Socket](http://sonoff.itead.cc/en/products/residential/s20-socket)
- [iTead Slampher](http://sonoff.itead.cc/en/products/residential/slampher-rf)
- [iTead Sonoff Touch](http://sonoff.itead.cc/en/products/residential/sonoff-touch)
- [iTead Sonoff T1](http://sonoff.itead.cc/en/products/residential/sonoff-t1)
- [iTead Sonoff SC](http://sonoff.itead.cc/en/products/residential/sonoff-sc)
- [iTead Sonoff Led](http://sonoff.itead.cc/en/products/appliances/sonoff-led)<img src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoff4ch.jpg" height="250" align="right" />
- [iTead Sonoff BN-SZ01 Ceiling Led](http://sonoff.itead.cc/en/products/appliances/bn-sz01)
- [iTead Sonoff B1](http://sonoff.itead.cc/en/products/residential/sonoff-b1)
- [iTead Sonoff RF Bridge 433](http://sonoff.itead.cc/en/products/appliances/sonoff-rf-bridge-433)
- [iTead Sonoff Dev](https://www.itead.cc/sonoff-dev.html)
- [iTead 1 Channel Switch 5V / 12V](https://www.itead.cc/smart-home/inching-self-locking-wifi-wireless-switch.html)
- [iTead Motor Clockwise/Anticlockwise](https://www.itead.cc/smart-home/motor-reversing-wifi-wireless-switch.html)
- [Electrodragon IoT Relay Board](http://www.electrodragon.com/product/wifi-iot-relay-board-based-esp8266/)
- [AI Light or any my9291 compatible RGBW LED](http://www.ebay.com/itm/172644855726)


### License

This program is licensed under GPL-3.0
