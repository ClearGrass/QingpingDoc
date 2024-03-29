# 青萍设备公有 MQTT 通讯协议

- [青萍设备公有 MQTT 通讯协议](#青萍设备公有-mqtt-通讯协议)
  - [1. 私有化支持](#1-私有化支持)
  - [2. MQTT 通讯协议](#2-mqtt-通讯协议)
    - [2.1 规则说明](#21-规则说明)
    - [2.2 Json 数据包字段及格式说明](#22-json-数据包字段及格式说明)
    - [2.2.1 设备传感器采集数据格式说明](#221-设备传感器采集数据格式说明)
    - [2.2.2 设备传感器采集数据格式子结构体说明](#222-设备传感器采集数据格式子结构体说明)
    - [2.2.3 设备配置格式说明](#223-设备配置格式说明)
    - [2.2.4 命令支持列表说明](#224-命令支持列表说明)
    - [2.2.5 命令执行结果状态支持列表说明](#225-命令执行结果状态支持列表说明)
  - [3. 命令说明](#3-命令说明)
    - [3.1 服务端发起BLE连接](#31-服务端发起ble连接)
    - [3.2 服务端断开BLE连接](#32-服务端断开ble连接)
    - [3.3 服务端打开BLE指定特征通知](#33-服务端打开ble指定特征通知)
    - [3.4 服务端关闭BLE指定特征通知](#34-服务端关闭ble指定特征通知)
    - [3.5 设备端BLE通知数据返回](#35-设备端ble通知数据返回)
    - [3.6 服务端写指定特征数据](#36-服务端写指定特征数据)
    - [3.7 服务端读BLE指定特征数据](#37-服务端读ble指定特征数据)
    - [3.8 设备端读取BLE数据返回](#38-设备端读取ble数据返回)
    - [3.9 设备端BLE广播数据返回](#39-设备端ble广播数据返回)
    - [3.10 设备端获取设备列表](#310-设备端获取设备列表)
    - [3.11 服务端返回设备列表](#311-服务端返回设备列表)
    - [3.12 设置临时上报间隔和持续时间](#312-设置临时上报间隔和持续时间)
    - [3.13 心跳包](#313-心跳包)
    - [3.14 重连 MQTT](#314-重连-mqtt)
    - [3.15 设备未绑定](#315-设备未绑定)
    - [3.16 修改上报频率](#316-修改上报频率)
    - [3.17 传感器数据](#317-传感器数据)
    - [3.18 服务端及设备端应答格式](#318-服务端及设备端应答格式)
    - [3.19 修改 MQTT 连接信息](#319-修改-mqtt-连接信息)
    - [3.20 上报设备日志](#320-上报设备日志)
    - [3.21 服务端发送OTA命令](#321-服务端发送ota命令)
    - [3.22 设备端接收到OTA命令后回复](#322-设备端接收到ota命令后回复)
    - [3.23 设备绑定状态](#323-设备绑定状态)
    - [3.24 设备端获取设备列表(带设备名)](#324-设备端获取设备列表带设备名)
    - [3.25 服务端返回设备列表(带设备名)](#325-服务端返回设备列表带设备名)
    - [3.25 第三方设备绑定状态](#325-第三方设备绑定状态)
    - [3.26 触发读取设备配置](#326-触发读取设备配置)
    - [3.27 读取设备配置回应](#327-读取设备配置回应)

## 1. 私有化支持

## 2. MQTT 通讯协议

### 2.1 规则说明

- 所有命令和应答以 Json 格式通信；
- MQTT Topic 规范，各设备支持上行、下行两个方向的数据通信，分别设置对应 Topic。对于私有化用户，上行和下行 Topics 可以使用默认值或也可自行设定，自定义的可以在上一节进行。
  - 上行 Topic 说明：用于设备发布属性、事件或其他拓展信息
  - 下行 Topic 说明：用于设备获取云端下发的设备配置或其他动作指令

### 2.2 Json 数据包字段及格式说明

下表列出所有可能字段，不同产品设备支持会有不同，请根据实际情况可选查看

| 属性                  | 类型   | 描述                                                                                |
| --------------------- | ------ | ----------------------------------------------------------------------------------- |
| type                  | 数值   | 通讯命令类型（支持命令类型说明如下）                                                |
| mac                   | 字符串 | BLE MAC 地址                                                                        |
| buffer                | 字符串 | BLE 数据内容                                                                        |
| length                | 数值   | BLE 数据内容长度                                                                    |
| srv_uuid              | 字符串 | BLE 服务 UUID                                                                       |
| chr_uuid              | 字符串 | BLE 特征 UUID                                                                       |
| adv_data              | 字符串 | BLE 广播数据                                                                        |
| rssi                  | 数值   | 信号强度                                                                            |
| timeout               | 数值   | 连接超时时间（单位秒），默认连接成功后10秒没有任何数据命令通信则断开连接            |
| sw_version            | 字符串 | 软件版本                                                                            |
| wifi_info             | 字符串 | 当前连接的 Wi-Fi 信息，如：SSID，信号强度，信道，Wi-Fi，MAC                         |
| up_itvl               | 数值   | 临时上报间隔（单位秒）                                                              |
| duration              | 数值   | 临时上报持续时间（单位秒）                                                          |
| dev_list              | 数组   | 设备列表                                                                            |
| mqtt_cfg              | 结构体 | MQTT配置（host, port, usrname, password, clientid, subscribe_topic, publish_topic） |
| wifi_cfg              | 结构体 | Wi-Fi 配置（SSID, PASSWORD）                                                        |
| sensor_data           | 结构体 | 设备传感器采集的数据（格式说明如下）                                                |
| setting               | 结构体 | 传感器采集间隔、上报间隔等（设备配置格式说明如下）                                  |
| timestamp             | 数值   | 时间戳（单位秒）                                                                    |
| co2_sampling_interval | 数值   | CO2 传感器采样间隔（单位秒）                                                        |
| pm_sampling_interval  | 数值   | PM2.5 PM10 传感器采样（单位秒）                                                     |
| temperature_unit      | 数值   | 温度单位（单位秒）                                                                  |
| result                | 数值   | 命令执行结果（支持命令类型说明如下）                                                |

### 2.2.1 设备传感器采集数据格式说明

| 属性        | 类型   | 单位     | 描述                   |
| ----------- | ------ | -------- | ---------------------- |
| battery     | 结构体 | %        | 电池电量百分比         |
| temperature | 结构体 | 摄氏度 C | 温度（算法补偿后的值） |
| humidity    | 结构体 | %        | 湿度百分比             |
| pm1         | 结构体 | ppm      | PM1                    |
| pm25        | 结构体 | ppm      | PM2.5                  |
| pm10        | 结构体 | ppm      | PM10                   |
| co2         | 结构体 | ppm      | 二氧化碳               |
| tvoc        | 结构体 | ppm      | TVOC                   |
| radon       | 结构体 | ppm      | 氡                     |

### 2.2.2 设备传感器采集数据格式子结构体说明

| 属性              | 类型   | 描述                                                                                                                 |
| ----------------- | ------ | -------------------------------------------------------------------------------------------------------------------- |
| value             | 数值   | 数值使用默认单位上报，不根据单位切换而改变                                                                           |
| status            | 数值   | 0：正常（正常时可无该字段），在battery里表示放电<br>1：异常，在battery里表示充电<br>2： 初始化， 在battery里表示满电 |
| level             | 数值   | 传感器检测结果等级（部分传感器会有）                                                                                 |
| unit              | 字符串 | 单位                                                                                                                 |
| status_duration   | 数值   | 状态持续时间（单位：秒）                                                                                             |
| status_start_time | 数值   | 状态开始时间                                                                                                         |

### 2.2.3 设备配置格式说明

| 属性                  | 类型   | 单位 | 描述                                     |
| --------------------- | ------ | ---- | ---------------------------------------- |
| report_interval       | 数值   | 秒   | 传感器数据上报周期                       |
| collect_interval      | 数值   | 秒   | 传感器数据采样间隔                       |
| co2_sampling_interval | 数值   | 秒   | CO2 传感器采样间隔                       |
| pm_sampling_interval  | 数值   | 秒   | PM2.5 PM10 传感器采样间隔                |
| temperature_unit      | 字符串 | -    | 温度单位设置                             |
| night_mode_start_time | 数值   | 秒   | 夜间模式开始事件，为从0点0分开始的分钟数 |
| night_mode_end_time   | 数值   | 秒   | 夜间模式结束时间,为从0点0分开始的分钟数  |

### 2.2.4 命令支持列表说明

| 值     | 描述                                                   |
| ------ | ------------------------------------------------------ |
| 1      | 服务端发起BLE连接                                      |
| 2      | 服务端断开BLE连接                                      |
| 3      | 服务端打开BLE通知                                      |
| 4      | 服务端关闭BLE通知                                      |
| 5      | 设备端BLE通知数据返回                                  |
| 6      | 服务端发送BLE数据(with response)                       |
| 7      | 服务端读取BLE数据                                      |
| 8      | 设备端BLE读数据返回                                    |
| 9      | 设备端返回广播数据                                     |
| 10     | 设备端获取设备列表                                     |
| 11     | 服务端返回设备列表                                     |
| 12     | 服务端设置临时广播上报间隔和持续时间                   |
| 13     | 心跳包                                                 |
| 14     | 重连MQTT                                               |
| 15     | 服务端发送BLE数据(without response)                    |
| 16     | 修改 Mqtt 连接信息                                     |
| 17     | 修改上报频率                                           |
| 12和17 | 传感器数据上报（12表示实时数据，17表示定时(历史)数据） |
| 18     | 服务端收到定时(历史)数据（type为17）上报后应答         |
| 19     | 上报设备日志                                           |
| 20     | 绑定状态                                               |
| 23     | 服务端发送OTA命令                                      |
| 24     | 设备端接收到OTA命令后回复                              |
| 25     | 设备端获取设备列表(带设备名)                           |
| 26     | 服务端返回设备列表(带设备名)                           |
| 27     | 第三方设备绑定状态                                     |
| 28     | 触发读取设备配置                                       |

### 2.2.5 命令执行结果状态支持列表说明

| 值  | 描述            |
| --- | --------------- |
| 1   | 成功            |
| -1  | 连接超时        |
| -2  | 蓝牙忙碌        |
| -3  | 写数据失败      |
| -4  | 读数据失败      |
| -5  | 打开BLE通知失败 |
| -6  | 关闭BLE通知失败 |
| -7  | 非绑定设备      |
| -8  | 连接异常        |
| -9  | 当前设备未连接  |
| -10 | Sparrow未绑定   |

## 3. 命令说明

### 3.1 服务端发起BLE连接

```json
{
    "type" : "1",
    "mac"  : "112233445566"
}
```

### 3.2 服务端断开BLE连接

```json
{
    "type" : "2",
    "mac"  : "112233445566"
}
```

### 3.3 服务端打开BLE指定特征通知

```json
{
    "type" : "3",
    "mac"  : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100"
}
```

### 3.4 服务端关闭BLE指定特征通知

```json
{
    "type" : "4",
    "mac" : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100"
}
```

### 3.5 设备端BLE通知数据返回

```json
{
    "type" : "5",
    "mac" : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100",
    "buffer" : "000102030405060708"
}
```

### 3.6 服务端写指定特征数据

```json
{
    "type" : "6",
    "mac" : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100",
    "buffer" : "000102030405060708"
}
```

### 3.7 服务端读BLE指定特征数据

```json
{
    "type" : "7",
    "mac" : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100"
}
```

### 3.8 设备端读取BLE数据返回

```json
{
    "type" : "8",
    "mac" : "112233445566",
    "srv_uuid" : "4d4650445346425546454a5500002122",
    "chr_uuid" : "0100",
    "buffer" : "000102030405060708"
}
```

### 3.9 设备端BLE广播数据返回

```json
{
    "type" : "9",
    "mac" : "112233445566",
    "adv_data" : "fdcd123456",
    "rssi" : "-30"
}
```

### 3.10 设备端获取设备列表

```json
{
    "type" : "10"
}
```

### 3.11 服务端返回设备列表

```json
{
    "type" : "11",
    "dev_list": ["112233445566", "AABBCCDDEEFF"]
}
```

### 3.12 设置临时上报间隔和持续时间

```json
{
    "type" : "12",
    "up_itvl" : "10",   // 持续60秒，每10秒上报一次
    "duration": "60"
}
注：上报的数据包括网关子设备和网关本身的数据
```

### 3.13 心跳包

```json
{
    "type": "13",
    "wifi_info": "cleargrass_SZ,-68,1,88:D7:F6:6A:A5:80",
    "sw_version": "2.0.7_0017"
}
```

### 3.14 重连 MQTT

```json
{
    "type" : "14"
}
```

### 3.15 设备未绑定

```json
{
    "result" : "-10"
}
```

### 3.16 修改上报频率

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
        "night_mode_start_time":1200, // 夜间模式开始时间,为从0点0分开始的分钟数
        "night_mode_end_time":480 // 夜间模式结束时间,为从0点0分开始的分钟数
    },
    "timestamp": 1592192453
}
注：上报的数据包括网关子设备和网关本身的数据
```

### 3.17 传感器数据

```json
{
    "id": 1223,
    "type": "12",   //type为12时 表示实时数据， type为17时表示定时数据
    "timestamp":1594815555,
    "mac" : "112233445566",
    "need_ack": 1, // 需要服务器应答的时候（比如type为17时），need_ack置为1，不需要应答的时候不用写need_ack字段
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

### 3.18 服务端及设备端应答格式

- 情景1：服务端收到来自设备端的上报数据分两种（type为12实时数据和type为17的定时数据），只对定时数据做应答；
- 情景2：设备端收到服务端下发的配置后进行应答以确认成功收到。

```json
{
    "type": "18",
    "ack_id": 1223,   //ack_id为收到传感器上报的id
    "code": 0,
    "desc": "错误描述"
}
```

### 3.19 修改 MQTT 连接信息

暂时不开放

### 3.20 上报设备日志

```json
{
    "type": "19",
    "desc": "upload log",
}
```

### 3.21 服务端发送OTA命令

```json
{
    "type" : "23",
    "ota_type" : 0,   // 0为WIFI模组OTA,1为MCU OTA
    "url": "OTA包下载链接"
}
```

### 3.22 设备端接收到OTA命令后回复

```json
{
    "type" : "24",
    "error_code" : 0,// 错误码,0为正常,非0出错
    "percent":100,//更新进度,0-100 
}
```

### 3.23 设备绑定状态

```json
{
   "type": "20",
    "status": 1 //1绑定，2未绑定
}
```

### 3.24 设备端获取设备列表(带设备名)

```json
{
    "type" : "25"
}
```

### 3.25 服务端返回设备列表(带设备名)

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

### 3.25 第三方设备绑定状态

```json
{
    "type": "27",
    "params":{
            "type": "bind", //bind unbind
            "company_id": 1
    }
}
```

### 3.26 触发读取设备配置

```json
{
    "type": "28"
}
```

### 3.27 读取设备配置回应

```json
{
    "type": "28",
    "setting": {
        "report_interval": 60,
        "collect_interval": 10,
        "pm_sampling_interval":1,
        "temperature_unit":"C",
        "display_off_time":15, //屏幕自动关闭时间(单位秒)0-300秒(0为不关闭)
        "power_off_time":15, //自动关机时间(单位分钟)0-60分钟(0为不关机)
        "night_mode_start_time":1200, // 夜间模式开始时间,为从0点0分开始的分钟数
        "night_mode_end_time":480, // 夜间模式结束时间,为从0点0分开始的分钟数
    }
}
```