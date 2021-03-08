pimatic-raspbee-plus
=======================

Extended version of pimatic-raspbee by [Treban](https://github.com/treban/pimatic-raspbee/)

## Extended Features

* New devices: Cover, Warning(Siren) and SmartPlug
* Hybrid light and sensor device SmartPlug


## Prerequisite

You need a rasbpee or conbee device and the [deCONZ REST API](https://github.com/dresden-elektronik/deconz-rest-plugin/blob/master/README.md#install-deconz) must be installed and configured.

An other good solution is to run deconz as a [docker container](https://github.com/marthoc/docker-deconz).

### => [go to the WIKI](https://github.com/treban/pimatic-raspbee/wiki) - there are installation instructions and other useful tips.

## Installation

Just activate the plugin over the pimatic webUI. The plugin manager automatically installs the package with his dependencys.

## Configuration

You can also load the plugin by adding following in the config.json from your pimatic server:
The extra feature

    {
      "plugin": "raspbee",
      "debug": true,
      "active": true,
      "ip": "<deconz ip>",
      "port": "<deconz port>"
    }

## Usages

To create a connection to the raspbee gateway, **the gateway must be unlocked over the deconz UI.**
[check the unlock howto](https://github.com/treban/pimatic-raspbee/wiki/Connect-the-raspbee-plugin-to-deconz)
Then make a device discovery in pimatic.

## Supported devices

### Lights

| pimatic Device type    | Feature                      | Deconz Resource Type
| ---------------------- | ---------------------------- | --------------
| `RaspBeeSwitch`        | switch on/off                | `On/Off plug-in unit` & `Smart plug`
| `RaspBeeDimmer`        | switch and dimm light        | `Dimmable light`
| `RaspBeeCT`            | change temperature           | `Color temperature light`
| `RaspBeeRGB`           | change color                 | `Color light`
| `RaspBeeRGBCT`         | change color and temperature | `Extended color light`

HINTs:

There are two color devices.
RaspBeeRGB (`Color light`) emulates the ColorTemperature via a color code.
RaspBeeRGBCT (`Extended color light`) supports native ColorTemperature actions.
All lights are detected automatically

Tradfri bulbs do not support hue/sat actions.
therefore only RGB values can be set in the rules.

Deconz also does not allow you to adjust the color or brightness when the lamp is off.
Use a delay in rules. First switch on the lamp and then set the values.


### Groups

| pimatic Device type               | Feature                      | Deconz Resource Type
| --------------------------------- | ---------------------------- | --------------
| `RaspBeeDimmerGroup` (DEPRECATED) | switch and dimm              | `Group`
| `RaspBeeRGBCTGroup` (NEW)         | change color and temperature | `Group`
| `RaspBeeGroupScenes`              | switch scene on              | `Group Scenes`

I have created a new group device with all color controls.

### Sensors

All sensors are represented as a `RaspBeeMultiDevice`.  

The device has 3 arrays.

* associated device IDs  
This array contains all device ID associated to this object.

* support parameter flag  
This array contains all supported ressource types.

* configMap  
During a device discovery, the config map is filled automatically.
Customizable values ​​are stored in this config map.
A custom config can be written to the API via the button (send config) on the new RaspBeeSystem device.

**Supported resource types:**

| supports parameter flag  | Unit                         | Deconz Resource Type
| ------------------------ | ---------------------------- | ---------------
| `battery`                | %                            | (any battery-powered sensor)
| `lowbattery`             | bool                         | (any IAS Zone sensor)
| `carbon`                 | bool                         | ZHACarbonMonoxide
| `switch`                 | string                       | ZHASwitch
| `fire`                   | bool                         | ZHAFire
| `humidity`               | %                            | ZHAHumidity
| `temperature`            | °C                           | ZHATemperature and any sensor with temperaure support
| `presence`               | bool                         | ZHAPresence
| `dark`                   | bool                         | ZHAPresence & ZHALightLevel
| `lux`                    | lux                          | ZHALightLevel
| `daylight`               | bool                         | ZHALightLevel
| `open`                   | bool                         | ZHAOpenClose
| `pressure`               | hPA                          | ZHAPressure
| `water`                  | bool                         | ZHAWater
| `vibration`              | bool                         | ZHAVibration
| `tampered`               | bool                         | (any IAS Zone sensor)
| `consumption`            | Wh                           | ZHAConsumption
| `power`                  | W                            | ZHAPower & ZHAConsumption
| `voltage`                | V                            | ZHAPower
| `current`                | mA                           | ZHAPower


### The other sensor types are DEPRECATED
but still usable

* RaspBeeMotionSensor
* RaspBeeContactSensor
* RaspBeeLightSensor
* RaspBeeSwitchSensor
* RaspBeeWaterSensor

### RaspBeeSystem management device

#### discover lights & discover sensors
This button starts a light or sensor detection from the deconz api.

#### create backup
This button creates a local backup of the deconz config.  
The target folder is configurable. (default folder is the pimatic-app folder)

#### send config
This button sends for all devices all config parameter (configMap) to the deconz apikey.


### ActionProvider

* **"activate group scene -name-"**  

* **"set color temp -name- to -value-"**  

* **"set color rgb -name- to -hexvalue-"**

* **"dim raspbee -name- to -value-"**

**Example:**  
set color temp Light1 to 10 and set color rgb Light2 to #121212 and activate group scene All-ON

**Optional** a -transtime- can be specified. This allows the changeover to be time-controlled.

**Example:**  
dim raspbee Flur to 100 transition time 2s and set color rgb Light RGB to #FF0000 with transition time 2s

### PredicateProvider

* **"received from -name- event "2001""**

Example:
recieved from Switch1 event "2001"


### ButtonEvents
|   | Value | Action
| - | ----- | -------
| 0 | x000  | Initial Press
| 1 | x001  | Hold
| 2 | x002  | Release (after press)
| 3 | x003  | Release (after hold)
| 4 | x004  | Double press
| 5 | x005  | Triple press
| 6 | x006  | Quadruple press
| 7 | x007  | Shake
| 8 | x008  | Drop
| 9 | x009  | Tilt
| 10 | x010 | Many press


## CHANGELOG
[-> see CHANGELOG](https://github.com/bertreb/pimatic-raspbee-plus/blob/main/CHANGELOG.md)

----------------------------
## Contributors pimatic-raspbee

* [kosta](https://github.com/treban)
* [sweebee](https://github.com/sweebee)
* [mwittig](https://github.com/mwittig)
