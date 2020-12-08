# 开放接口说明文档

接口默认规范请参见 [规范说明](/main/specification)  
接口 SDK 支持请参见 [开放接口 SDK](/main/ccSdk#1-开放接口-sdk)

- [开放接口说明文档](#开放接口说明文档)
  - [1. 设备相关接口](#1-设备相关接口)
    - [1.1 绑定设备](#11-绑定设备)
      - [1.1.1 请求头](#111-请求头)
      - [1.1.2 请求参数](#112-请求参数)
      - [1.1.3 返回结果](#113-返回结果)
    - [1.2 删除设备](#12-删除设备)
      - [1.2.1 请求头](#121-请求头)
      - [1.2.2 请求参数](#122-请求参数)
    - [1.3 设备列表](#13-设备列表)
      - [1.3.1 请求头](#131-请求头)
      - [1.3.2 请求参数](#132-请求参数)
      - [1.3.3 返回结果](#133-返回结果)
    - [1.4 设备历史数据](#14-设备历史数据)
      - [1.4.1 请求头](#141-请求头)
      - [1.4.2 请求参数](#142-请求参数)
      - [1.4.3 返回结果](#143-返回结果)
    - [1.5 设备历史事件](#15-设备历史事件)
      - [1.5.1 请求头](#151-请求头)
      - [1.5.2 请求参数](#152-请求参数)
      - [1.5.3 返回结果](#153-返回结果)
    - [1.6 修改设备配置](#16-修改设备配置)
      - [1.6.1 请求头](#161-请求头)
      - [1.6.2 请求参数](#162-请求参数)
    - [1.7 分组列表](#17-分组列表)
      - [1.7.1 请求头](#171-请求头)
      - [1.7.2 请求参数](#172-请求参数)
      - [1.7.3 返回结果](#173-返回结果)

## 1. 设备相关接口

### 1.1 绑定设备

- **接口说明：** 绑定设备
- **接口方法：** POST
- **接口地址：** /v1/apis/devices

#### 1.1.1 请求头

| 参数名称      | 参数值           | 描述                                                           |
| :------------ | :--------------- | :------------------------------------------------------------- |
| Authorization | token            | 格式为 "Bearer YourToken"，注意 Bearer 与 token 之间有一个空格 |
| Content-Type  | application/json | 固定值                                                         |

#### 1.1.2 请求参数

| 参数名称     | 类型   | 出现要求 | 描述                                                                      |
| :----------- | :----- | :------- | :------------------------------------------------------------------------ |
| device_token | string | R        | 配对码                                                                    |
| product_id   | int    | R        | 产品ID（参考 [规范说明 - 2.1 产品列表](/main/specification#21-产品列表)） |
| timestamp    | int    | R        | 毫秒级时间戳(13位) 20s内有效,同一个请求不可重复                           |

请求示例：

```sh
https://apis.cleargrass.com/v1/apis/devices

{
    "device_token": "143412",
    "product_id": "1001",
    "timestamp": 1596527411689
}
```

#### 1.1.3 返回结果

| 参数名称            | 类型   | 出现要求 | 描述                                                       |
| :------------------ | :----- | :------- | :--------------------------------------------------------- |
| info                | object | R        | 设备信息                                                   |
| &emsp;name          | string | R        | 设备名称                                                   |
| &emsp;mac           | string | R        | 设备mac地址                                                |
| &emsp;version       | string | R        | 设备版本                                                   |
| &emsp;created_at    | string | R        | 设备注册时间                                               |
| &emsp;product       | object | R        | 产品信息                                                   |
| &emsp;&emsp;id      | string | C        | 产品ID（参考 [规范说明](/main/specification#21-产品列表)） |
| &emsp;&emsp;name    | string | C        | 产品名称                                                   |
| &emsp;&emsp;en_name | string | C        | 产品英文名称                                               |
| data                | object | C        | 设备数据                                                   |
| &emsp;battery       | object | C        | 电量                                                       |
| &emsp;humidity      | object | C        | 湿度                                                       |
| &emsp;pressure      | object | C        | 气压                                                       |
| &emsp;temperature   | object | C        | 温度                                                       |
| &emsp;timestamp     | object | C        | 时间                                                       |
| &emsp;&emsp;value   | float  | C        | 数值                                                       |

***注意：*** 不同类型的设备，data 字段返回的属性种类不一样，具体参考 [规范说明](/main/specification)

示例：

```json
{
    "info":{
        "name": "温湿度气压计",
        "mac": "582D34460442",
        "version": "1.0.1_0049",
        "created_at": 1573034091,
        "product": {"id": 1101, "name": "青萍温湿度气压计","en_name": "Qingping Temp & RH Barometer"}
    },
    "data":{
        "battery": {"value": 70},
        "humidity": {"value": 22.2},
        "pressure": {"value": 103.29},
        "temperature": {"value": 20.8},
        "timestamp": {"value": 1574565267}
    }
}
```

### 1.2 删除设备

- **接口说明：** 删除设备
- **接口方法：** DELETE
- **接口地址：** /v1/apis/devices

#### 1.2.1 请求头

| 参数名称      | 参数值           | 描述                                                           |
| :------------ | :--------------- | :------------------------------------------------------------- |
| Authorization | token            | 格式为 "Bearer YourToken"，注意 Bearer 与 token 之间有一个空格 |
| Content-Type  | application/json | 固定值                                                         |

#### 1.2.2 请求参数

| 参数名称  | 类型     | 出现要求 | 描述                                            |
| :-------- | :------- | :------- | :---------------------------------------------- |
| mac       | []string | R        | mac地址                                         |
| timestamp | int      | R        | 毫秒级时间戳(13位) 20s内有效,同一个请求不可重复 |

请求示例：

```sh
https://apis.cleargrass.com/v1/apis/devices

{
    "mac": ["582D34460442"],
    "timestamp": 1596527411689,
}
```

### 1.3 设备列表

- **接口说明：** 设备列表
- **接口方法：** GET
- **接口地址：** /v1/apis/devices

#### 1.3.1 请求头

| 参数名称      | 参数值 | 描述                                                           |
| :------------ | :----- | :------------------------------------------------------------- |
| Authorization | token  | 格式为 "Bearer YourToken"，注意 Bearer 与 token 之间有一个空格 |

#### 1.3.2 请求参数

| 参数名称  | 类型 | 出现要求 | 描述                                            |
| :-------- | :--- | :------- | :---------------------------------------------- |
| timestamp | int  | R        | 毫秒级时间戳(13位) 20s内有效,同一个请求不可重复 |
| group_id  | int  | O        | 分组id                                          |
| offset    | int  | O        | 偏移量                                          |
| limit     | int  | O        | 最大返回数据条数 不得超过50                     |

请求示例：

```sh
https://apis.cleargrass.com/v1/apis/devices?timestamp=1596527411689
```

#### 1.3.3 返回结果

| 参数名称                  | 类型     | 出现要求 | 描述                                                       |
| :------------------------ | :------- | :------- | :--------------------------------------------------------- |
| total                     | int      | R        | 设备总数                                                   |
| devices                   | []object | R        | 设备数据                                                   |
| &emsp;info                | object   | R        | 设备信息                                                   |
| &emsp;&emsp;name          | string   | R        | 设备名称                                                   |
| &emsp;&emsp;mac           | string   | R        | 设备mac地址                                                |
| &emsp;&emsp;group_id      | string   | R        | 分组id                                                     |
| &emsp;&emsp;group_name    | string   | R        | 分组名称                                                   |
| &emsp;&emsp;status        | object   | C        | 状态                                                       |
| &emsp;&emsp;&emsp;offline | bool     | C        | 状态 （True表示离线False表示在线）                         |
| &emsp;&emsp;version       | string   | R        | 设备版本                                                   |
| &emsp;&emsp;created_at    | string   | R        | 设备注册时间                                               |
| &emsp;&emsp;product       | object   | R        | 产品信息                                                   |
| &emsp;&emsp;&emsp;id      | string   | C        | 产品ID（参考 [规范说明](/main/specification#21-产品列表)） |
| &emsp;&emsp;&emsp;name    | string   | C        | 产品名称                                                   |
| &emsp;&emsp;&emsp;en_name | string   | C        | 产品英文名称                                               |
| &emsp;data                | object   | C        | 设备数据                                                   |
| &emsp;&emsp;battery       | object   | C        | 电量                                                       |
| &emsp;&emsp;humidity      | object   | C        | 湿度                                                       |
| &emsp;&emsp;pressure      | object   | C        | 气压                                                       |
| &emsp;&emsp;temperature   | object   | C        | 温度                                                       |
| &emsp;&emsp;tvoc          | object   | C        | 挥发物质                                                   |
| &emsp;&emsp;co2           | object   | C        | 二氧化碳                                                   |
| &emsp;&emsp;pm25          | object   | C        | pm25                                                       |
| &emsp;&emsp;timestamp     | object   | C        | 时间                                                       |
| &emsp;&emsp;&emsp;value   | float    | C        | 数值                                                       |
| &emsp;&emsp;&emsp;level   | float    | C        | 等级                                                       |
| &emsp;&emsp;&emsp;status  | float    | C        | 状态                                                       |

***注意：*** 不同类型的设备，data 字段返回的属性种类不一样，具体参考 [规范说明](/main/specification)

示例：

```json
{
    "total": 2,
    "devices":[
        {
            "info":{
                "name": "温湿度气压计",
                "mac": "582D34460442",
                "version": "1.0.1_0049",
                "created_at": 1573034091,
                "status":{
                    "offline": false,
                    "altering": false
                },
                "group_id": 11,
                "group_name": "卡卡罗特",
                "product": {"id": 1101, "name": "青萍温湿度气压计","en_name": "Qingping Temp & RH Barometer"}
            },
            "data":{
                "battery": {"value": 70},
                "humidity": {"value": 22.2},
                "pressure": {"value": 103.29},
                "temperature": {"value": 20.8},
                "timestamp": {"value": 1574565267}
            }
        },
        {
            "info":{
                "name": "温湿度气压计",
                "mac": "582D34460442",
                "version": "1.0.1_0049",
                "created_at": 1573034091,
                "status":{
                    "offline": false,
                    "altering": false
                },
                "group_id": 11,
                "group_name": "卡卡罗特",
                "product": {"id": 1101, "name": "青萍温湿度气压计","en_name": "Qingping Temp & RH Barometer"}
            },
            "data":{
                "battery": {"value": 70},
                "humidity": {"value": 22.2},
                "pressure": {"value": 103.29},
                "temperature": {"value": 20.8},
                "timestamp": {"value": 1574565267}
            }
        }
    ]
}
```

### 1.4 设备历史数据

- **接口说明：** 设备历史数据
- **接口方法：** GET
- **接口地址：** /v1/apis/devices/data

#### 1.4.1 请求头

| 参数名称      | 参数值 | 描述                                                           |
| :------------ | :----- | :------------------------------------------------------------- |
| Authorization | token  | 格式为 "Bearer YourToken"，注意 Bearer 与 token 之间有一个空格 |

#### 1.4.2 请求参数

| 参数名称   | 类型   | 出现要求 | 描述                                            |
| :--------- | :----- | :------- | :---------------------------------------------- |
| mac        | string | R        | 设备mac                                         |
| start_time | int    | R        | 开始时间                                        |
| end_time   | int    | R        | 结束时间                                        |
| timestamp  | int    | R        | 毫秒级时间戳(13位) 20s内有效,同一个请求不可重复 |
| offset     | int    | O        | 偏移量                                          |
| limit      | int    | O        | 最大返回数量 不得超过200条                      |

请求示例：

```sh
 https://apis.cleargrass.com/v1/apis/devices/data?mac=582D3446029C&end_time=1573527615&limit=200&start_time=1573354814&timestamp=1596527411689
```

#### 1.4.3 返回结果

| 参数名称          | 类型   | 出现要求 | 描述     |
| :---------------- | :----- | :------- | :------- |
| total             | int    | R        | 总条数   |
| data              | object | R        | 数据正文 |
| &emsp;battery     | object | C        | 电量     |
| &emsp;humidity    | object | C        | 湿度     |
| &emsp;pressure    | object | C        | 气压     |
| &emsp;temperature | object | C        | 温度     |
| &emsp;timestamp   | object | R        | 时间戳   |
| &emsp;&emsp;value | float  | R        | 数值     |

***注意：*** 不同类型的设备，data 字段返回的属性种类不一样，具体参考 [规范说明](/main/specification)

示例：

```json
{
    "total": 22,
    "data": [
        {
            "battery": {"value": 70},
            "humidity": {"value": 22.2},
            "pressure": {"value": 103.29},
            "temperature": {"value": 20.8},
            "timestamp": {"value": 1574565267}
        },
        {
            "battery": {"value": 70},
            "humidity": {"value": 22.2},
            "pressure": {"value": 103.29},
            "temperature": {"value": 20.8},
            "timestamp": {"value": 1574565267}
        }
    ]
}
```

### 1.5 设备历史事件

- **接口说明：** 设备历史事件
- **接口方法：** GET
- **接口地址：** /v1/apis/devices/events

#### 1.5.1 请求头

| 参数名称            | 参数值 | 描述                                                           |
| :------------------ | :----- | :------------------------------------------------------------- |
| &emsp;Authorization | token  | 格式为 "Bearer YourToken"，注意 Bearer 与 token 之间有一个空格 |

#### 1.5.2 请求参数

| 参数名称         | 类型   | 出现要求 | 描述                                            |
| :--------------- | :----- | :------- | :---------------------------------------------- |
| &emsp;mac        | string | R        | 设备mac                                         |
| &emsp;start_time | int    | R        | 开始时间                                        |
| &emsp;end_time   | int    | R        | 结束时间                                        |
| &emsp;timestamp  | int    | R        | 毫秒级时间戳(13位) 20s内有效,同一个请求不可重复 |
| &emsp;offset     | int    | O        | 偏移量                                          |
| &emsp;limit      | int    | O        | 最大返回数量 不得超过200条                      |

请求示例：

```sh
https://apis.cleargrass.com/v1/apis/devices/events?mac=582D3446029C&end_time=1573527615&limit=200&start_time=1573354814&timestamp=1596527411689
```

#### 1.5.3 返回结果

| 参数名称                 | 类型   | 出现要求 | 描述                           |
| :----------------------- | :----- | :------- | :----------------------------- |
| total                    | int    | R        | 总条数                         |
| events                   | object | R        | 数据正文                       |
| &emsp;data               | object | C        | 设备数据                       |
| &emsp;&emsp;battery      | object | C        | 电量                           |
| &emsp;&emsp;humidity     | object | C        | 湿度                           |
| &emsp;&emsp;pressure     | object | C        | 气压                           |
| &emsp;&emsp;temperature  | object | C        | 温度                           |
| &emsp;&emsp;timestamp    | object | R        | 时间戳                         |
| &emsp;&emsp;&emsp;value  | float  | R        | 数值                           |
| &emsp;&emsp;&emsp;level  | float  | C        | 等级                           |
| &emsp;&emsp;&emsp;status | float  | C        | 状态（1-触发，2-解除）         |
| &emsp;alert_config       | object | C        | 触发条件                       |
| &emsp;&emsp;metric_name  | string | R        | 事件属性（温度、湿度、气压等） |
| &emsp;&emsp;operator     | string | R        | 操作符（大于gt、小于lt）       |
| &emsp;&emsp;threshold    | float  | R        | 阈值                           |

***注意：*** 不同类型的设备，events 的 data 字段返回的属性种类不一样，events 的 alert_config.metric_name 支持的事件类型也不一样，具体参考 [规范说明](/main/specification)

示例：

```json
{
    "total":20,
    "events":[
        {
            "data": {
                "battery": {"value": 70},
                "humidity": {"value": 22.2},
                "pressure": {"value": 103.29},
                "temperature": {"value": 20.8},
                "timestamp": {"value": 1574565267}
            },
            "alert_config": {
                "metric_name": "temperature",
                "operator": "gt",
                "threshold": 15,
            }
        },
        {
            "data": {
                "battery": {"value": 70},
                "humidity": {"value": 22.2},
                "pressure": {"value": 103.29},
                "temperature": {"value": 20.8},
                "timestamp": {"value": 1574565267}
            },
            "alert_config": {
                "metric_name": "temperature",
                "operator": "gt",
                "threshold": 16,
            }
        }
    ]
}
```

### 1.6 修改设备配置

- **接口说明：** 修改设备配置
- **接口方法：** PUT
- **接口地址：** /v1/apis/devices/settings

#### 1.6.1 请求头

| 参数名称      | 参数值           | 描述                                                           |
| :------------ | :--------------- | :------------------------------------------------------------- |
| Authorization | token            | 格式为 "Bearer YourToken"，注意 Bearer 与 token 之间有一个空格 |
| Content-Type  | application/json | 固定值                                                         |

#### 1.6.2 请求参数

| 参数名称         | 类型     | 出现要求 | 描述                                            |
| :--------------- | :------- | :------- | :---------------------------------------------- |
| mac              | []string | R        | mac地址                                         |
| report_interval  | int      | R        | 上报周期(秒)最小为10s,且为采集周期的整数倍      |
| collect_interval | int      | R        | 采集周期(秒)                                    |
| timestamp        | int      | R        | 毫秒级时间戳(13位) 20s内有效,同一个请求不可重复 |

请求示例：

```sh
https://apis.cleargrass.com/v1/apis/devices/settings

{
    "mac": ["582D34460442"],
    "report_interval": 10,
    "collect_interval": 5,
    "timestamp": 1596527411689
}
```

### 1.7 分组列表

- **接口说明：** 分组列表
- **接口方法：** GET
- **接口地址：** /v1/apis/devices/groups

#### 1.7.1 请求头

| 参数名称      | 参数值           | 描述                                                           |
| :------------ | :--------------- | :------------------------------------------------------------- |
| Authorization | token            | 格式为 "Bearer YourToken"，注意 Bearer 与 token 之间有一个空格 |
| Content-Type  | application/json | 固定值                                                         |

#### 1.7.2 请求参数

| 参数名称  | 类型 | 出现要求 | 描述                                            |
| :-------- | :--- | :------- | :---------------------------------------------- |
| timestamp | int  | R        | 毫秒级时间戳(13位) 20s内有效,同一个请求不可重复 |

请求示例：

```sh
https://apis.cleargrass.com/v1/apis/devices/groups?timestamp=1602468406039
```

#### 1.7.3 返回结果

参数名称						    |类型		|出现要求	    |描述  
:----						    |:---		|:------	|:---
total				            |int		|R			|分组总数
groups				            |[]object	|R			|分组列表
&emsp;id				        |int	    |R			|分组id
&emsp;name				        |string		|R			|分组名称

示例：

```json
{
    "total": 4,
    "groups": [
        {
            "id": 1381,
            "name": "卡卡罗特"
        },
        {
            "id": 1382,
            "name": "贝吉塔"
        },
        {
            "id": -11,
            "name": "plus"
        },
        {
            "id": -12,
            "name": "devPlatform"
        }
    ]
}
```