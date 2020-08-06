# Qingping Temp & RH Barometer LoRaWAN Modbus Protocol

- [Qingping Temp & RH Barometer LoRaWAN Modbus Protocol](#qingping-temp--rh-barometer-lorawan-modbus-protocol)
  - [1．Protocol Specification](#1protocol-specification)
  - [2．Protocol Command Format Specification](#2protocol-command-format-specification)
    - [2.1 2．Protocol Format](#21-2protocol-format)
    - [2.2 Command Specification](#22-command-specification)
    - [2.2.1 CMD List](#221-cmd-list)
    - [2.2.2 Sensor Data Report](#222-sensor-data-report)
      - [2.2.2.1 History data format](#2221-history-data-format)
      - [2.2.2.2 实时数据格式说明](#2222-实时数据格式说明)
    - [2.2.2 事件上报](#222-事件上报)
      - [2.2.2.1 传感器数据格式](#2221-传感器数据格式)
      - [2.2.2.2 事件类型](#2222-事件类型)
    - [2.2.3 设备获取网络时间](#223-设备获取网络时间)
    - [2.2.4 事件下发](#224-事件下发)
    - [2.2.5 配置设置](#225-配置设置)

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

| Byte Order Number         | Description                                                                          | Value                                                                                                                                                                                                                       |
| ------------------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1                         | Device address                                                                       | 0x01                                                                                                                                                                                                                        |
| 2                         | Data report code                                                                     | 0x41                                                                                                                                                                                                                        |
| 3                         | Data length                                                                          | 0x25                                                                                                                                                                                                                        |
| 4                         | Data type (history data)                                                             | 0x00                                                                                                                                                                                                                        |
| 5 - 8                     | Timestamp                                                                            | 0x5C7788B6                                                                                                                                                                                                                  |
| 9 – 10                    | Data storage interval (seconds)                                                      | 0x0005                                                                                                                                                                                                                      |
| 11 – 16                   | Sensor data for group 1                                                              | 第1－3字节为温湿度：0x2FC29A, 温度湿度各占12bit, 高12bit为温度，0x02FC, 温度值正向偏移500，实际值为0x02FC-500=264(放大10倍)，低12bit为湿度：0x29A(放大10倍) <br>第4－5字节为气压：0x2766(放大100倍) <br>第6字节为电量：0x4E |
| 17－22                    | 第2组传感器数据                                                                      |                                                                                                                                                                                                                             |
| 每6个字节为一组传感器数据 | LoRa：一帧历史数据最多有5组传感器数据 Wi-Fi/NB-Iot: 一帧历史数据最多有40组传感器数据 |                                                                                                                                                                                                                             |
| 41 - 42                   | 校验码                                                                               | 0x488C                                                                                                                                                                                                                      |

各组传感器时间戳需要根据起始时间戳和数据采集间隔来计算。

如以上参考数据中：

第一组传感器采集时间为：0x5C7788B6

第二组传感器采集时间为：0x5C7788B6 + 0x0005（数据采集间隔）

第三组传感器采集时间为：0x5C7788B6 + 0x0005 + 0x0005

...

以此类推至最后的有效数据采集时间。

#### 2.2.2.2 实时数据格式说明

一条完整实时数据上报格式如下：

01 41 15 01 5C 77 88 B6 2F C2 9A 27 66 4E 31 2E 30 2E 30 5F 30 30 34 31 5D C6

实时数据解析：

| 字节序号 | 说明               | 数值                                                                                                                                                                                                                        |
| -------- | ------------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | 设备地址           | 0x01                                                                                                                                                                                                                        |
| 2        | 数据上报功能码     | 0x41                                                                                                                                                                                                                        |
| 3        | 数据长度           | 0x15                                                                                                                                                                                                                        |
| 4        | 数据类型(实时数据) | 0x01                                                                                                                                                                                                                        |
| 5 - 8    | 时间戳             | 0x5C7788B6                                                                                                                                                                                                                  |
| 9 – 14   | 传感器数据         | 第1－3字节为温湿度：0x2FC29A, 温度湿度各占12bit, 高12bit为温度，0x02FC, 温度值正向偏移500，实际值为0x02FC-500=264(放大10倍)，低12bit为湿度：0x29A(放大10倍) <br>第4－5字节为气压：0x2766(放大100倍) <br>第6字节为电量：0x4E |
| 15 - 25  | 版本号             | 对应ASCII：1.0.0_0041                                                                                                                                                                                                       |
| 26 - 27  | 校验码             | 0x5DC6                                                                                                                                                                                                                      |

### 2.2.2 事件上报

返回参数格式说明：

| 类型 | ADDR | CMD  | LEN  | DATA     |        |            |            |
| ---- | ---- | ---- | ---- | -------- | ------ | ---------- | ---------- |
| 接收 | 0x01 | 0x44 | 0x0B | 事件类型 | 时间戳 | 传感器数据 | 事件设置值 |
|      |      |      |      | 1字节    | 4字节  | 6字节      | 2字节      |

#### 2.2.2.1 传感器数据格式

数据格式具体请参考 [2.2.2.2 实时数据格式说明](/main/private/lorawan#2222-实时数据格式说明) 中表格里的“传感器数据”格式

#### 2.2.2.2 事件类型

| 事件类型 | 说明               |
| -------- | ------------------ |
| 0x07     | 设置大于温度值上报 |
| 0x08     | 设置小于温度值上报 |
| 0x0A     | 设置大于湿度值上报 |
| 0x0B     | 设置小于湿度值上报 |
| 0x0D     | 设置大于气压值上报 |
| 0x0E     | 设置小于气压值上报 |

### 2.2.3 设备获取网络时间

| 类型 | ADDR | CMD  | LEN  | DATA   | CRC |
| ---- | ---- | ---- | ---- | ------ | --- |
| 发送 | 0x01 | 0x45 | 0x00 | 空     |     |
| 接收 | 0x01 | 0x45 | 0x04 | 时间戳 |     |

如：

设备上报请求时间命令，服务器收到后需下发时间戳 如下发时间：2019/5/27 17:2:17
时间戳为：1558947737，对应下发协议：01 45 04 5C EB A7 99 2C AB

### 2.2.4 事件下发

设置传感器上报策略格式说明：

| 类型 | ADDR | CMD  | LEN  | DATA     |          |          |          |         |
| ---- | ---- | ---- | ---- | -------- | -------- | -------- | -------- | ------- |
| 发送 | 0x01 | 0x42 | 0x0C | 事件类型 | 重复次数 | 开始时间 | 结束时间 | 设置值  |
|      |      |      |      | 占1字节  | 占1字节  | 占4字节  | 占4字节  | 占2字节 |

事件类型：参考**2.2.2 b 项说明**

重复次数：1次：0x01, 每天：0xFE(暂定只有这两种状态) 该配置暂未使用，默认为0x01

开始时间、结束时间：单位分钟，以0点0分作为基点进行分钟偏移。如设置7：00－10：00时间段，则对应为420分钟－600分钟。如为全时间段则设置开始时间等于结束时间即可，如设置开始时间为0，结束时间为0，即可表示一天全时段。

注：设置值 温度需要放大10倍后正向偏移500，湿度放大10倍，气压放大100倍。

单位说明：

温度：摄氏度

湿度：%RH

气压：kPa

如：设置全时段，温度大于26度上报

01 42 0C 07 01 00 00 00 00 00 00 00 00 02 F8 23 E4

### 2.2.5 配置设置

| 类型 | ADDR | CMD  | LEN  | DATA                    |                       |                                 |                               |                           |
| ---- | ---- | ---- | ---- | ----------------------- | --------------------- | ------------------------------- | ----------------------------- | ------------------------- |
| 发送 | 0x01 | 0x47 | 0x06 | 数据上报间隔2字节(分钟) | 数据采集间隔2字节(秒) | 蓝牙广播间隔2字节(秒, 保留未用) | 通知重复间隔2字节（保留未用） | 温度单位1字节（保留未用） |

如设置间隔时间：

上报数据间隔：1小时

数据采集间隔：15分钟

保留未用项用0补全下发。

01 47 09 00 3C 03 84 00 00 00 00 00 28 5E
