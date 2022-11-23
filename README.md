# aqara-local-network-api

Xiaomi Aqara Gateway (米家多功能网关) is a ZigBee gateway which is able to connect with a few sensors, e.g. motion sensor, window/door sensor and smart socket plug. 

The Aqara Gateway exposes a local network API for collecting data and controlling ZigBee (sub-)devices which connect to the gateway. 

## Protocol Overview

Aqara local network API uses UDP protocol in either multicast or unicast, depends on command. When multicast is used, IP multicast address is `224.0.0.50`.

Datagram is in JSON format, with some of below properties:

| Property | Description                              | Possible Values                          |
| -------- | ---------------------------------------- | ---------------------------------------- |
| cmd      | Each datagram must has a command name.   | `whois`, `iam`, ` get_id_list`, ` get_id_list_ack`, ` heartbeat`, `read`, `read_ack`,  `write`, `write_ack`, |
| model    | Device model.                            | `gateway`, `magnet`, ` sensor_ht`, ` motion`, ` ctrl_neutral1`, ` ctrl_neutral2`, `86sw1`, `86sw2`, `plug`, `cube` |
| sid      | Device ID. May be either gateway ID -- which is MAC address in lower case without colon, or sub-device ID. |                                          |
| short_id | Short ID of ZigBee device. For gateway, it's `0`. _[Seems no use in protocol]_ |                                          |
| data     | The data to be transferred. Content is a JSON string. |                                          |

## Device Discovery





- [Device discovery](device_discover.md)
- [Read/write operation](device_read_write.md)
- [Device heartbeat](device_heartbeat.md)
- ​