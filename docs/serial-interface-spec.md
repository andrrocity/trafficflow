# **Device Serial Interface Protocol**

## Introduction

This document describes how to communicate with the **TrafficFlow\*** vehicle control device.

---

## Communication Transport

This specification aassumes a reliable streaming transport, like TCP.

## Communcaiton Protocol

Communcation takes place thought a command. A command consists of a name and content, which is a collectkon of key/value pairs.

### Command Format

|    Name | Data Type | Description                                 |
| ------: | :-------: | ------------------------------------------- |
| Command |  String   | The command name.                           |
|   Count |   UInt8   | The numbrer of key/value pairs that follow. |
|     Key |  String   | The key name for the value that follows.    |
|   Value |  String   | The value for the key that precedes.        |
|     ... |           | `Key` and `Value` repeated `Count` times.   |

### String Encoding

Only UTF-8 character encoding is supported. A string is sent in chunks, with each chunk starting with a single byte indicating the length of the string that follows. The last chunk is indicated by length byte of zero.

```none
([Length {UInt8)][UTF8 Text...])+[0 (UInt8)]
```

---

## **Commands**

The device will never send a command unsolicited. The client can send the following commands to get data.

### **Request Lateral and/or Longitudinal Control**

Request: `VEHICLE-CONTROL`

The device times out these requested control values after 500ms. So, this command must be sent at least every 500ms to keep the control values maintained.

|    Key | Value                                                                                                                                                                                                                                   |
| -----: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  `lat` | _(Number, Optional)_ The magnitude of torque overlay to apply. The value is interpreted as a double-precision point. It is expressed aas a percentage ranging from -100.0 to 100.0. Negative values indicate counterclockwise rotation. |
| `long` | _(Number, Optional)_ The magnitude of engine or brake. The value is interpreted as a double-precision point. It is expressed as a percentage ranging from -100.0 to 100.0. Negative values indicate braking.                            |

Response: None

---

### **Read Log Entries**

Requests the device send any pending log entries.

Request: `READ-LOG`

Response: `LOG-ENTRY`

|       Key | Value                                                                           |
| --------: | ------------------------------------------------------------------------------- |
| `content` | _(String, Optional)_ The log entries, individually separated with a line break. |
|    `lost` | _(Number, Optional)_ The number of log entries skipped since the last read.     |

### **Request Device Information**

Requests device inforamtoin.

Request: `GET-DEVICE-INFO`

Response: `DEVICE-INFO`

|             Key | Value                                           |
| --------------: | ----------------------------------------------- |
|          `name` | _(String, Optional)_ The device's display name. |
|       `version` | _(String)_ The software version.                |
|    `build-date` | _(Date)_ The software build date.               |
| `serial-number` | _(String)_ The device's serial number.          |

---

### **Request Statistics**

Requests device statstics.

Request: `GET-NETWORK-INFO`

Response: `NETWORK-INFO`

|                Key | Value                                                                                           |
| -----------------: | ----------------------------------------------------------------------------------------------- |
|    `can[index]-rx` | _(Number)_ The number of messages received on CAN channel indiciated bv `index`.                |
|    `can[index]-tx` | _(Number)_ The number of messages sent on CAN channel indiciated bv `index`.                    |
| `can[index]-nodes` | _(Date)_ Comma seperated list of node identifiers detected on CAN channel indicated by `index`. |
