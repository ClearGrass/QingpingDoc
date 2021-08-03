# QingPing MQTT Communication Protocol

- [QingPing MQTT Communication Protocol](#qingping-mqtt-communication-protocol)
  - [1. Privatization Support](#1-privatization-support)
  - [2. MQTT Communication Protocol](#2-mqtt-communication-protocol)
    - [2.1 Rule Specification](#21-rule-specification)
    - [2.2 Json Formation Specification](#22-json-formation-specification)
    - [2.2.1 Device Sensor Data Format](#221-device-sensor-data-format)
    - [2.2.2 Device Sensor Data Format - Sub-Structure format](#222-device-sensor-data-format---sub-structure-format)
    - [2.2.3 Device Setting Format Specification](#223-device-setting-format-specification)
    - [2.2.4 Command Type Support List](#224-command-type-support-list)
    - [2.2.5 Result Code of Command List](#225-result-code-of-command-list)
  - [3. Command Specification](#3-command-specification)
    - [3.1 Server Sends BLE Connection Request](#31-server-sends-ble-connection-request)
    - [3.2 Server Sends BLE Disconnection Request](#32-server-sends-ble-disconnection-request)
    - [3.3 Server Sends Open BLE Notification Request](#33-server-sends-open-ble-notification-request)
    - [3.4 Server Sends Close BLE Notification Request](#34-server-sends-close-ble-notification-request)
    - [3.5 Response For BLE Notification From Device](#35-response-for-ble-notification-from-device)
    - [3.6 Server Sends BLE Data(with response)](#36-server-sends-ble-datawith-response)
    - [3.7 Server Reads BLE Data](#37-server-reads-ble-data)
    - [3.8 Response For BLE Data From Device](#38-response-for-ble-data-from-device)
    - [3.9 Broadcast Data From Device](#39-broadcast-data-from-device)
    - [3.10 Device Requests Device List](#310-device-requests-device-list)
    - [3.11 Server Response Device List](#311-server-response-device-list)
    - [3.12 Server Send Setting For Temporary Report And Duration Time](#312-server-send-setting-for-temporary-report-and-duration-time)
    - [3.13 Heartbeat Package](#313-heartbeat-package)
    - [3.14 Reconnect MQTT](#314-reconnect-mqtt)
    - [3.15 Device Unbinding](#315-device-unbinding)
    - [3.16 Modify Data Report Interval](#316-modify-data-report-interval)
    - [3.17 Sensor Data Report](#317-sensor-data-report)
    - [3.18 Server Response For History Data Report](#318-server-response-for-history-data-report)
    - [3.19 Device Report Log](#319-device-report-log)
    - [3.20 Binding Status](#320-binding-status)
    - [3.21 Server Sends OTA Command](#321-server-sends-ota-command)
    - [3.22 Device Responses For OTA Command](#322-device-responses-for-ota-command)
    - [3.23 Device Requests Device List(with device name)](#323-device-requests-device-listwith-device-name)
    - [3.24 Server Responses Device List(with device name)](#324-server-responses-device-listwith-device-name)
    - [3.25 Binding Status For Third Part's Device](#325-binding-status-for-third-parts-device)
    - [3.26 Server Requests To Read Device Setting](#326-server-requests-to-read-device-setting)
    - [3.27 Device Responses Device Setting](#327-device-responses-device-setting)

## 1. Privatization Support

## 2. MQTT Communication Protocol

### 2.1 Rule Specification

- All messages are encoded/decoded by Json format;
- Every device supports two topics for reporting and receiving messages.
  - Up Topic: used by the device to report data, events or other information to the server
  - Down Topic: used by the device to receive the response or setting from the server

### 2.2 Json Formation Specification

The table lists all possible parameters, devices of different product may have different items of the table.

| Parameter             | Type      | Description                                                                                                                                                      |
| --------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type                  | Integer   | Command type(Supported type refer to [2.2.4 Command Type Support List](#224-command-type-support-list))                                                          |
| mac                   | String    | BLE MAC Address                                                                                                                                                  |
| buffer                | String    | BLE data content                                                                                                                                                 |
| length                | Integer   | BLE data content length                                                                                                                                          |
| srv_uuid              | String    | BLE Service UUID                                                                                                                                                 |
| chr_uuid              | String    | BLE Feature UUID                                                                                                                                                 |
| adv_data              | String    | BLE broadcast data                                                                                                                                               |
| rssi                  | Integer   | RSSI                                                                                                                                                             |
| timeout               | Integer   | Connecting timeout(in second). Note: After make connection successfully if there is no data communication in 10 seconds, the connection will be closed           |
| sw_version            | String    | Firmware version                                                                                                                                                 |
| wifi_info             | String    | Wi-Fi Information used now, such as SSID, RSSI, Channel, Wi-Fi MAC                                                                                               |
| up_itvl               | Integer   | Temporary report interval(second)                                                                                                                                |
| duration              | Integer   | Temporary report duration(second)                                                                                                                                |
| dev_list              | List      | Device list                                                                                                                                                      |
| mqtt_cfg              | Structure | MQTT Configuration（host, port, usrname, password, clientid, subscribe_topic, publish_topic）                                                                    |
| wifi_cfg              | Structure | Wi-Fi Setting（SSID, PASSWORD）                                                                                                                                  |
| sensor_data           | Structure | Sensor data from the device(data formate refer to [2.2.1 Device Sensor Data Format](#221-device-sensor-data-format))                                             |
| setting               | Structure | Setting for data report interval, acquisition interval and so on(refer to [2.2.3 Device Setting Format Specification](#223-device-setting-format-specification)) |
| co2_sampling_interval | Integer   | CO2 sensor acquisition interval(second)                                                                                                                          |
| pm_sampling_interval  | Integer   | PM2.5 PM10 sensor acquisition interval(second)                                                                                                                   |
| temperature_unit      | String    | Temperature unit                                                                                                                                                 |
| timestamp             | Integer   | Timestamp now(second)                                                                                                                                            |
| result                | Integer   | Result code(refer to [2.2.5 Result Code of Command List](#225-result-code-of-command-list))                                                                      |

### 2.2.1 Device Sensor Data Format

| Parameter   | Type      | Unit | Description                     |
| ----------- | --------- | ---- | ------------------------------- |
| battery     | Structure | %    | Battery(percent)                |
| temperature | Structure | C    | Temperature(After compensation) |
| humidity    | Structure | %    | Humidity(percent)               |
| pm1         | Structure | ppm  | PM1                             |
| pm25        | Structure | ppm  | PM2.5                           |
| pm10        | Structure | ppm  | PM10                            |
| co2         | Structure | ppm  | Carbon Dioxide                  |
| tvoc        | Structure | ppm  | TVOC                            |
| radon       | Structure | ppm  | Radon                           |

### 2.2.2 Device Sensor Data Format - Sub-Structure format

| Parameter         | Type    | Description                                                                                                                           |
| ----------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| value             | Integer | The value is in default unit, will not change by setting                                                                              |
| status            | Integer | 0: Normal, For battery means discharging<br>1: Abnormal，For battery means charging<br>2: Initialize, For battery means fully charged |
| level             | Integer | Sensor data level(only for some kind of sensors)                                                                                      |
| unit              | String  | Unit for the value                                                                                                                    |
| status_duration   | Integer | Status duration(second)                                                                                                               |
| status_start_time | Integer | Start time of the status                                                                                                              |

### 2.2.3 Device Setting Format Specification

| Parameter             | Type    | Unit   | Description                                   |
| --------------------- | ------- | ------ | --------------------------------------------- |
| report_interval       | Integer | Second | Sensor data report interval                   |
| collect_interval      | Integer | Second | Sensor data acquisition interval              |
| co2_sampling_interval | Integer | Second | CO2 sensor acquisition interval               |
| pm_sampling_interval  | Integer | Second | PM2.5 PM10 sensor acquisition interval        |
| temperature_unit      | String  | -      | temperature unit setting                      |
| night_mode_start_time | Integer | Second | start time for night mode(minutes from 00:00) |
| night_mode_end_time   | Integer | Second | end time fro night mode(minutes from 00:00)   |

### 2.2.4 Command Type Support List

| Value     | Description                                                |
| --------- | ---------------------------------------------------------- |
| 1         | Server sends BLE connection request                        |
| 2         | Server sends BLE disconnection request                     |
| 3         | Server sends open BLE notification request                 |
| 4         | Server sends close BLE notification request                |
| 5         | Response for BLE notification from device                  |
| 6         | Server sends BLE data(with response)                       |
| 7         | Server reads BLE data                                      |
| 8         | Response for BLE data from device                          |
| 9         | Broadcast data from device                                 |
| 10        | Device requests device list                                |
| 11        | Server response device list                                |
| 12        | Server send setting for temporary report and duration time |
| 13        | Heartbeat package                                          |
| 14        | Reconnect MQTT                                             |
| 15        | Server sends BLE data(without response)                    |
| 16        | Modify MQTT connection setting                             |
| 17        | Modify data report interval                                |
| 12 and 17 | Sensor data report(12: real-time data, 17: history data)   |
| 18        | Server response for history data report in type 17         |
| 19        | Device report log                                          |
| 20        | Binding status                                             |
| 23        | Server sends OTA command                                   |
| 24        | Devices response for OTA command                           |
| 25        | Device requests device list(with device name)              |
| 26        | Server responses device list(with device name)             |
| 27        | Binding status for third part's device                     |
| 28        | Request to read device setting                             |

### 2.2.5 Result Code of Command List

| Value | Description                   |
| ----- | ----------------------------- |
| 1     | Success                       |
| -1    | Connect timeout               |
| -2    | Bluetooth busy                |
| -3    | write data failed             |
| -4    | read data failed              |
| -5    | Open BLE notification failed  |
| -6    | Close BLE notification failed |
| -7    | Unbinding device              |
| -8    | Connect exception             |
| -9    | Device offline                |
| -10   | Sparrow unbinding             |

## 3. Command Specification

### 3.1 Server Sends BLE Connection Request

```json
{
    "type" : "1",
    "mac"  : "112233445566"
}
```

### 3.2 Server Sends BLE Disconnection Request

```json
{
    "type" : "2",
    "mac"  : "112233445566"
}
```

### 3.3 Server Sends Open BLE Notification Request

```json
{
    "type" : "3",
    "mac"  : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100"
}
```

### 3.4 Server Sends Close BLE Notification Request

```json
{
    "type" : "4",
    "mac" : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100"
}
```

### 3.5 Response For BLE Notification From Device

```json
{
    "type" : "5",
    "mac" : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100",
    "buffer" : "000102030405060708"
}
```

### 3.6 Server Sends BLE Data(with response)

```json
{
    "type" : "6",
    "mac" : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100",
    "buffer" : "000102030405060708"
}
```

### 3.7 Server Reads BLE Data

```json
{
    "type" : "7",
    "mac" : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100"
}
```

### 3.8 Response For BLE Data From Device

```json
{
    "type" : "8",
    "mac" : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100",
    "buffer" : "000102030405060708"
}
```

### 3.9 Broadcast Data From Device

```json
{
    "type" : "9",
    "mac" : "112233445566",
    "adv_data" : "fdcd123456",
    "rssi" : "-30"
}
```

### 3.10 Device Requests Device List

```json
{
    "type" : "10"
}
```

### 3.11 Server Response Device List

```json
{
    "type" : "11",
    "dev_list": ["112233445566", "AABBCCDDEEFF"]
}
```

### 3.12 Server Send Setting For Temporary Report And Duration Time

```json
{
    "type" : "12",
    "up_itvl" : "10",   // report data every 10 seconds
    "duration": "60"    // last fro 60 seconds
}
```

### 3.13 Heartbeat Package

```json
{
    "type": "13",
    "wifi_info": "cleargrass_SZ,-68,1,88:D7:F6:6A:A5:80",
    "sw_version": "2.0.7_0017"
}
```

### 3.14 Reconnect MQTT

```json
{
    "type" : "14"
}
```

### 3.15 Device Unbinding

```json
{
    "result" : "-10"
}
```

### 3.16 Modify Data Report Interval

```json
{
    "id": 1223,
    "need_ack": 1,
    "type": "17",
    "desc": "change settings",
    "setting": {
        "report_interval": 60,
        "collect_interval": 10,
        "co2_sampling_interval": 2,
        "pm_sampling_interval":1,
        "temperature_unit":"C",
        "night_mode_start_time":1200,
        "night_mode_end_time":480
    },
    "timestamp": 1592192453
}
```

### 3.17 Sensor Data Report

```json
{
    "id": 1223,
    "type": "12",   // (12: real-time data, 17: history data)
    "timestamp":1594815555,
    "mac" : "112233445566",
    "need_ack": 1, // 1: need ack from server; ignore: no ack from server
     "sensorData": [
        {
            "battery": {"value":99},
            "timestamp": {"value":1592192453},
            "temperature": {"value":25.1,"unit":"C"},
            "humidity": {"value":99,"unit":"%"},
            "pm10": {"value":99}
        },
        {
            "battery": {"value":99},
            "timestamp": {"value":1592192453},
            "temperature": {"value":25.1,"unit":"C"},
            "humidity": {"value":99,"unit":"%"},
            "pm10": {"value":99}
        }
    ]
}
```

### 3.18 Server Response For History Data Report

```json
{
    "type": "18",
    "ack_id": 1223,   //ack_id is the id from the report data of device
    "code": 0,
    "desc": ""
}
```

### 3.19 Device Report Log

```json
{
    "type": "19",
    "desc": "upload log",
}
```

### 3.20 Binding Status

```json
{
   "type": "20",
    "status": 1 // 1: binding, 2: unbinding
}
```

### 3.21 Server Sends OTA Command

```json
{
    "type" : "23",
    "ota_type" : 0,   // 0: Wi-Fi Module OTA, 1: MCU OTA
    "url": "OTA package URI"
}
```

### 3.22 Device Responses For OTA Command

```json
{
    "type" : "24",
    "error_code" : 0, // 0: succeed, other: failed
    "percent":100, // OTA progress [0-100]
}
```

### 3.23 Device Requests Device List(with device name)

```json
{
    "type" : "25"
}
```

### 3.24 Server Responses Device List(with device name)

```json
{
    "type" : "26",
    "homekit_dev_list": [
        {
            "mac": "112233445566",
            "name": "xxx的温湿度计",
        },
        {
            "mac": "112233445577",
            "name": "xxx的温湿度计2",
        },
    ]
}
```

### 3.25 Binding Status For Third Part's Device

```json
{
    "type": "27",
    "params":{
            "type": "bind", //bind unbind
            "company_id": 1
    }
}
```

### 3.26 Server Requests To Read Device Setting

```json
{
    "type": "28"
}
```

### 3.27 Device Responses Device Setting

```json
{
    "type": "28",
    "setting": {
        "report_interval": 60,
        "collect_interval": 10,
        "pm_sampling_interval":1,
        "temperature_unit":"C",
        "display_off_time":15,
        "power_off_time":15,
        "night_mode_start_time":1200,
        "night_mode_end_time":480,
    }
}
```
