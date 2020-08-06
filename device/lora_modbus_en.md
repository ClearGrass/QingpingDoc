# Qingping Temp & RH Barometer LoRaWAN Modbus Protocol

- [Qingping Temp & RH Barometer LoRaWAN Modbus Protocol](#qingping-temp--rh-barometer-lorawan-modbus-protocol)
  - [1．Protocol Specification](#1protocol-specification)
  - [2．Protocol Command Format Specification](#2protocol-command-format-specification)
    - [2.1 2．Protocol Format](#21-2protocol-format)
    - [2.2 Command Specification](#22-command-specification)
    - [2.2.1 CMD List](#221-cmd-list)
    - [2.2.2 Sensor Data Report](#222-sensor-data-report)
      - [2.2.2.1 History data format](#2221-history-data-format)
      - [2.2.2.2 Real-time Data Format](#2222-real-time-data-format)
    - [2.2.3 Event Report Format](#223-event-report-format)
      - [2.2.3.1 Sensor Data Formate](#2231-sensor-data-formate)
      - [2.2.3.2 Event Type](#2232-event-type)
    - [2.2.4 Request Network Time](#224-request-network-time)
    - [2.2.5 Event Configuration Format](#225-event-configuration-format)
    - [2.2.5.1 Event type](#2251-event-type)
      - [2.2.5.2 Repeat times](#2252-repeat-times)
      - [2.2.5.3 Begin and End Time](#2253-begin-and-end-time)
      - [2.2.5.4 Target Value](#2254-target-value)
    - [2.2.6 Data Report Configuration](#226-data-report-configuration)

## 1．Protocol Specification

- Protocol applies Modbus-RTU like data format, device can report data or make requests itself.
- All data need to be encoded with Base64 (Notice: demos below use hexadecimal digits as examples, but in actual environment you will see the Base64 encoded data)

## 2．Protocol Command Format Specification

### 2.1 2．Protocol Format

|             | ADDR         | CMD          | LEN         | DATA | CRC       |
| ----------- | ------------ | ------------ | ----------- | ---- | --------- |
| Description | Address code | Command code | Data length | data | Check bit |
| Byte length | 1            | 1            | 1           | N    | 2         |

Note:

1. Byte order is Little-Endian
2. Address code is fixed to 0x01

### 2.2 Command Specification

### 2.2.1 CMD List

| CMD  | Description                                                           |
| ---- | --------------------------------------------------------------------- |
| 0xFF | Answer command                                                        |
| 0x41 | Device reports data                                                   |
| 0x42 | Device reports event configuration                                    |
| 0x43 | Device gets event from server                                         |
| 0x44 | Device reports event                                                  |
| 0x45 | Device requests network time (LoRa)                                   |
| 0x46 | Device requests new firmware (Wi-Fi)                                  |
| 0x47 | Server pushes configuration to device or device reports configuration |

Response Format:

<table>
    <tr>
        <th>Type</th>
        <th>ADDR</th>
        <th>CMD</th>
        <th>LEN</th>
        <th colspan="2">DATA</th>
        <th>CRC</th>
    </tr>
    <tr>
        <td rowspan="3">Response</td>
        <td rowspan="3">0x01</td>
        <td rowspan="3">0xFF</td>
        <td rowspan="3">0x02</td>
        <td>Function code of response</td>
        <td>Response status</td>
        <td rowspan="3"> - </td>
    </tr>
    <tr>
        <td></td>
        <td>Success: 0x00</td>
    </tr>
    <tr>
        <td></td>
        <td>Failed: 0x01</td>
    </tr>
</table>

Note:

1. Response command is only supported on devices of Wi-Fi version and NB-Iot version.
2. Response should be make for all non-read requests.
3. For read requests, failed response should be make when no data is available.

### 2.2.2 Sensor Data Report

<table>
    <tr>
        <th>Type</th>
        <th>ADDR</th>
        <th>CMD</th>
        <th>LEN</th>
        <th colspan="4">DATA</th>
        <th>CRC</th>
    </tr>
    <tr>
        <td rowspan="3">Report</td>
        <td rowspan="3">0x01</td>
        <td rowspan="3">0x41</td>
        <td rowspan="3">0x06-0x24</td>
        <td>Data type</td>
        <td colspan="3">Specification</td>
        <td rowspan="3"> - </td>
    </tr>
    <tr>
        <td>0x00</td>
        <td colspan="3">History data</td>
    </tr>
    <tr>
        <td>0x01</td>
        <td>Timestamp(4 bytes)</td>
        <td>Real-time data(6 bytes)</td>
        <td>Version code(10 bytes)</td>
    </tr>
</table>

#### 2.2.2.1 History data format

Deme for one complete history data is:

01 41 25 00 5C 77 88 B6 00 05 2F C2 9A 27 66 4E 2F C2 9A 27 66 4E 2F C2 9A 27 66
4E 2F C2 9A 27 66 4E 2F C2 9A 27 66 4E 48 8C

Analysis of history data:

| Byte Order Number                           | Description                                                                                                      | Value                                                                                                                                                                                                                                                                                                      |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1                                           | Device address                                                                                                   | 0x01                                                                                                                                                                                                                                                                                                       |
| 2                                           | Data report code                                                                                                 | 0x41                                                                                                                                                                                                                                                                                                       |
| 3                                           | Data length                                                                                                      | 0x25                                                                                                                                                                                                                                                                                                       |
| 4                                           | Data type (history data)                                                                                         | 0x00                                                                                                                                                                                                                                                                                                       |
| 5 - 8                                       | Timestamp                                                                                                        | 0x5C7788B6                                                                                                                                                                                                                                                                                                 |
| 9 – 10                                      | Data acquisition interval (seconds)                                                                              | 0x0005                                                                                                                                                                                                                                                                                                     |
| 11 – 16                                     | The 1st group sensor data                                                                                        | The 1-3 bytes are temperature and humidity: 0x2FC29A, The high 12 bits are temperature: 0x02FC, actual temperature value is 0x02FC-500=264 (multiply by 10). The low 12 bits are humidity: 0x29A (multiply by 10) <br>The 4-5 bytes are pressure: 0x2766 (multiply by 100) <br>The 6 byte is battery: 0x4E |
| 17－22                                      | The 2nd group sensor data                                                                                        | Same as 1st group                                                                                                                                                                                                                                                                                          |
| Every 6 bytes form one group of sensor data | LoRa: one frame includes at most 5 groups of data.<br>Wi-Fi/NB-Iot: one frame includes at most 40 groups of data | Same as 1st group                                                                                                                                                                                                                                                                                          |
| 41 - 42                                     | CRC                                                                                                              | 0x488C                                                                                                                                                                                                                                                                                                     |

Note:
Time for each group of sensor data can be get by the beginning timestamp and the data acquisition interval, for example:
Time for the 1st group of sensor data is: 0x5C7788B6
Time for the 2nd group of sensor data is: 0x5C7788B6 + 0x0005（acquisition interval）
Time for the 3rd group of sensor data is: 0x5C7788B6 + 0x0005 + 0x0005
...

#### 2.2.2.2 Real-time Data Format

Deme for one complete real-time data is:

01 41 15 01 5C 77 88 B6 2F C2 9A 27 66 4E 31 2E 30 2E 30 5F 30 30 34 31 5D C6

Analysis of real-time data:

| Byte Order Number | Description                | Value                                                                                                                                                                                                                                                                                                      |
| ----------------- | -------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1                 | Device address             | 0x01                                                                                                                                                                                                                                                                                                       |
| 2                 | Data report code           | 0x41                                                                                                                                                                                                                                                                                                       |
| 3                 | Data length                | 0x15                                                                                                                                                                                                                                                                                                       |
| 4                 | Data type (real-time data) | 0x01                                                                                                                                                                                                                                                                                                       |
| 5 - 8             | Timestamp                  | 0x5C7788B6                                                                                                                                                                                                                                                                                                 |
| 9 – 14            | Sensor data                | The 1-3 bytes are temperature and humidity: 0x2FC29A, The high 12 bits are temperature: 0x02FC, actual temperature value is 0x02FC-500=264 (multiply by 10). The low 12 bits are humidity: 0x29A (multiply by 10) <br>The 4-5 bytes are pressure: 0x2766 (multiply by 100) <br>The 6 byte is battery: 0x4E |
| 15 - 25           | Version code               | Corresponding ASCII: 1.0.0_0041                                                                                                                                                                                                                                                                            |
| 26 - 27           | CRC                        | 0x5DC6                                                                                                                                                                                                                                                                                                     |

### 2.2.3 Event Report Format

| Type   | ADDR | CMD  | LEN  | DATA       |           |             |                     |
| ------ | ---- | ---- | ---- | ---------- | --------- | ----------- | ------------------- |
| Accept | 0x01 | 0x44 | 0x0B | Event type | Timestamp | Sensor data | Event configuration |
|        |      |      |      | 1 byte     | 4 bytes   | 6 bytes     | 2 bytes             |

#### 2.2.3.1 Sensor Data Formate

Please refer to Sensor data format in table of [2.2.2.2 实时数据格式说明](/main/private/lorawan#2222-实时数据格式说明)

#### 2.2.3.2 Event Type

| Type | Description                                                       |
| ---- | ----------------------------------------------------------------- |
| 0x07 | Event trigger condition: temperature is greater than target value |
| 0x08 | Event trigger condition: temperature is less than target value    |
| 0x0A | Event trigger condition: humidity is greater than target value    |
| 0x0B | Event trigger condition: humidity is less than target value       |
| 0x0D | Event trigger condition: pressure is greater than target value    |
| 0x0E | Event trigger condition: pressure is less than target value       |

### 2.2.4 Request Network Time

| Type   | ADDR | CMD  | LEN  | DATA      | CRC |
| ------ | ---- | ---- | ---- | --------- | --- |
| Send   | 0x01 | 0x45 | 0x00 | -         |     |
| Accept | 0x01 | 0x45 | 0x04 | Timestamp |     |

Demo:
Device send a network time request to server, server answers the request with time: 2019/5/27 17:2:17,
the timestamp is: 1558947737 and corresponding message data is: 01 45 04 5C EB A7 99 2C AB

### 2.2.5 Event Configuration Format

Server sends event configuration to device, format is:

| Type | ADDR | CMD  | LEN  | DATA       |              |            |          |              |
| ---- | ---- | ---- | ---- | ---------- | ------------ | ---------- | -------- | ------------ |
| Send | 0x01 | 0x42 | 0x0C | Event type | Repeat times | Begin time | End time | Target value |
|      |      |      |      | 1 byte     | 1 byte       | 4 bytes    | 4 bytes  | 2 bytes      |

### 2.2.5.1 Event type

Please refer to [2.2.3.2 Event Type](/main/private/lorawan#2232-实时数据格式说明)

#### 2.2.5.2 Repeat times

| Times             | Value | Description |
| ----------------- | ----- | ----------- |
| Once in all       | 0x01  | Default     |
| Once time per day | 0xFE  |             |

#### 2.2.5.3 Begin and End Time

Unit: minute. Value is the offset begin from 00:00 every day. For example:
7:00 to 10:00 corresponding to 420 to 600  
If you want get event alert all day, you can set both the begin and end time to 0.

#### 2.2.5.4 Target Value

Temperature should be multiplied by 10 and add 500.  
Humidity should be multiplied by 10.  
Pressure should be multiplied by 100.

Unit:
Temperature：C (Celsius degree)  
Humidity: %RH  
Pressure: kPa

One demo for: Send a alert event when temperature is greater than 26C in all day.

01 42 0C 07 01 00 00 00 00 00 00 00 00 02 F8 23 E4

### 2.2.6 Data Report Configuration

| Type | ADDR | CMD  | LEN  | DATA                          |                                   |                                               |                                       |                                 |
| ---- | ---- | ---- | ---- | ----------------------------- | --------------------------------- | --------------------------------------------- | ------------------------------------- | ------------------------------- |
| Send | 0x01 | 0x47 | 0x06 | Data report interval (minute) | Data collection interval (second) | BLE broadcast interval (second, not used now) | Repeat report interval (not used now) | Temperature unit (not used now) |
|      |      |      |      | 2 bytes                       | 2 bytes                           | 2 bytes                                       | 2 bytes                               | 1 byte                          |

Demo for: Report interval is 1 hour, data collection interval is 15 minutes.

01 47 09 00 3C 03 84 00 00 00 00 00 28 5E
