---
title: "Insta InstaRemote control via MQTT"
description: "Integrate your Insta InstaRemote via Zigbee2MQTT with whatever smart home
 infrastructure you are using without the vendors bridge or gateway."
---

*To contribute to this page, edit the following
[file](https://github.com/Koenkk/zigbee2mqtt.io/blob/master/docs/devices/InstaRemote.md)*

# Insta InstaRemote

| Model | InstaRemote  |
| Vendor  | Insta  |
| Description | ZigBee Light Link wall/handheld transmitter |
| Supports | action |
| Picture | ![Insta InstaRemote](../images/devices/InstaRemote.jpg) |
| White-label | Gira 2430-100, Gira 2435-10, Jung ZLLCD5004M, Jung ZLLLS5004M, Jung ZLLA5004M, Jung ZLLHS4 |

## Notes

### Transmitters Loosing Connection in ZigBee 3 Networks
With their factory firmware, the transmitters loose network connection after a few hours when ZigBee 3 devices are present in the network (which is a pretty much standard nowadays). For the Jung wall and handheld transmitters there is a firmware update available that fixes this problem (see [OTA updates](#ota-updates) below), but in turn decreases battery lifetime down to a few months.

Unfortunately Gira seems to have dropped support for their ZigBee transmitters completely and does not offer any firmware updates at all. For the Gira handheld transmitter the Jung update seems to work (and to fix the problem), but for the Gira wall transmitter this is not the case (it only has 6 buttons instead of 8 on the Jung wall transmitter and would therefore need a different firmware). There does not seem to be real solution for this problem rendering the Gira wall transmitters pretty much useless nowadays.

### Factory Reset (8-Button Devices)
* Press and hold buttons `3` and `4` simultaneously for about 10 seconds until the green LEDs start to flash.
* Release buttons `3` and `4` and then briefly press button `O` within 10 seconds.
* The LEDs should light up green for 3 seconds and the transmitter has been reset.  
![Reset](../images/InstaRemote-reset.jpg)

### Join Network (8-Button Devices)
* Press and hold buttons `5` and `I` simultaneously until the green LEDs start to flash. Then release the buttons again.
* After 10 more seconds the transmitter will start to search for an open network in order to join it.
* If the transmitter was able to join a network, the LEDs will light up green for 3 seconds (otherwise the LEDs will flash red quickly for 3 seconds).  
![Join Network](../images/InstaRemote-join-network.jpg)


## OTA updates
This device supports OTA updates, for more information see [OTA updates](../information/ota_updates.md).

**For the device to ask for/accept OTA updates, it needs to be in "programming mode" (same mode as for joining a network, see above).**  
In case the device does still not accept updates or seems to be stuck somehow, it may help to do a factory reset, join the network again and then again enter programming mode before starting the OTA update again.

## Manual Home Assistant configuration
Although Home Assistant integration through [MQTT discovery](../integration/home_assistant) is preferred,
manual integration is possible with the following configuration:


{% raw %}
```yaml
sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    icon: "mdi:gesture-double-tap"
    value_template: "{{ value_json.action }}"

sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    icon: "mdi:signal"
    unit_of_measurement: "lqi"
    value_template: "{{ value_json.linkquality }}"

binary_sensor:
  - platform: "mqtt"
    state_topic: "zigbee2mqtt/<FRIENDLY_NAME>"
    availability_topic: "zigbee2mqtt/bridge/state"
    payload_on: true
    payload_off: false
    value_template: "{{ value_json.update_available}}"
```
{% endraw %}


