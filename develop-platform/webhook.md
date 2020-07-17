# WebHook / MQTT 推送说明

平台会通过用户设置的各类 WebHooks 或 MQTT 在相应场景下有新数据时，向用户进行推送，使用户在最快时间获取到相应信息

- [WebHook / MQTT 推送说明](#webhook--mqtt-推送说明)
  - [1. 规则说明](#1-规则说明)
    - [1.1 WebHook](#11-webhook)
    - [1.2 MQTT](#12-mqtt)
  - [2. 签名验证](#2-签名验证)
  - [3. WebHook 支持列表](#3-webhook-支持列表)
  - [3.1 设备数据推送](#31-设备数据推送)
    - [3.1.1 数据结构说明](#311-数据结构说明)
    - [3.1.1 数据样例](#311-数据样例)
  - [3.2 设备事件推送](#32-设备事件推送)
    - [3.2.1 事件结构说明](#321-事件结构说明)
    - [3.2.2 数据样例](#322-数据样例)

## 1. 规则说明

### 1.1 WebHook

- 推送的数据位于 HTTP 请求的 Body 中，均为 Json 结构，头部类型为 "Content-Type: application/json"；
- HTTP 请求发出后，请在 3 秒内进行回应，否则平台认为请求超时，该次请求进入延迟推送状态；
- HTTP 请求返回码为 200 时，表示数据推送成功，返回其他状态码，平台判断为推送失败，该次请求进入延迟推送状态；
- 延迟推送会分别在 5、15、30 分钟后尝试重新发起 HTTP 请求进行数据推送，如果期间有成功推送或 3 次均失败，都会终止该次数据后续推送，即丢弃该任务（如果仍然希望获取该丢失数据，可使用开放接口进行获取）
- 若接口 10 秒请求超时占比超过 50%，平台会对对此接口做熔断处理，10分钟后会再次尝试访问，期间数据不再推送，请通过开放接口进行获取

### 1.2 MQTT

- 推送的数据均为 Json 结构序列化后的字符串；
- 若 MQTT 推送 10 秒请求超时占比超过 50%，平台会对对此MQTT地址做熔断处理，10分钟后会再次尝试访问，期间数据不再推送，请通过开放接口进行获取

------

## 2. 签名验证

为了确保请求的安全性，平台对推送的数据进行了签名，放在请求的 Json 结构体中，签名使用的参数包括：

| 参数      | 类型   | 描述                        |
| --------- | ------ | --------------------------- |
| timestamp | int    | Unix 时间戳                 |
| token     | string | 随机字符串                  |
| signature | string | HMAC 编码后的十六进制字符串 |

验证签名方式：

- 拼接 timestamp 和 token，中间无分隔符；
- 使用 HMAC 对拼接后的字符串进行编码（使用您在平台的 App Secret 和 SHA256 签名）；
- 将得到的签名结果与请求体中的 signature 进行对比，以验证请求合法性；
- 建议：可以缓存 token 字段，以避免重放攻击；
- 建议：可以检查 timestamp 字段，以避免过期请求
- 说明：鉴于推送的数据格式多样，内容可能较大，所以选择对部分字段进行签名

------

## 3. WebHook 支持列表

## 3.1 设备数据推送

当设备有新数据时，平台通过此回调地址，将设备数据推送给开发者云服务。

### 3.1.1 数据结构说明

| 参数名称          | 类型   | 出现要求 | 描述                                        |
| ----------------- | ------ | -------- | ------------------------------------------- |
| info.mac          | string | R        | 设备 MAC 地址                               |
| info.product.id   | int    | R        | 设备所属产品类型ID                          |
| info.product.desc | string | R        | 设备所属产品类型描述                        |
| info.name         | string | R        | 设备名称（用户定义的）                      |
| data              | list   | R        | 设备数据（数组，说明如下:设备数据字段说明） |

设备数据字段说明
***注意：*** 不同类型的设备，data 字段返回的属性种类不一样，具体参考 [规范说明 - 2. 设备列表及属性支持说明](/main/specification#2-设备列表及属性支持说明)

| 参数名称    | 类型   | 出现要求 | 描述           |
| ----------- | ------ | -------- | -------------- |
| temperature | object | C        | 温度（结构体） |
| humidity    | object | C        | 湿度           |
| pressure    | object | C        | 气压           |
| battery     | object | C        | 电量           |
| timestamp   | object | C        | Unix 时间戳    |

设备数据子类描述
不同传感器种类输出的数据格式不同，所以该结构里的字段均为可选字段，具体参考 [规范说明 - 2. 设备列表及属性支持说明](/main/specification#2-设备列表及属性支持说明)

| 参数名称 | 类型   | 出现要求 | 描述 |
| -------- | ------ | -------- | ---- |
| value    | float  | R        | 数字 |
| level    | string | C        | 等级 |
| status   | string | C        | 状态 |

### 3.1.1 数据样例

```json
{
    "signature": {
        "signature": "f325c82c5aa8f89bb2af5e10b0605b9a3ff057c1e51db9b460c9ced4797e532e",
        "timestamp": 1594785322,
        "token": "0204e1f7-c64f-11ea-b4e9-00163e2c48b3"
    },
    "payload": {
        "info": {
            "mac": "582D344C046C",
            "sn": "",
            "product": {
                "id": 1101,
                "desc": "青萍温湿度气压计"
            },
            "name": "Qingping Temp RH Barometer",
            "version": "",
            "created_at": 0
        },
        "data": [{
            "timestamp": {
                "value": 1594785339
            },
            "battery": {
                "value": 65
            },
            "temperature": {
                "value": 28.5
            },
            "humidity": {
                "value": 50.5
            },
            "pressure": {
                "value": 99.33
            }
        }]
    }
}
```

## 3.2 设备事件推送

当设备有新数据时，平台通过此回调地址，将设备数据推送给开发者云服务。

### 3.2.1 事件结构说明

| 参数名称          | 类型   | 出现要求 | 描述                                        |
| ----------------- | ------ | -------- | ------------------------------------------- |
| info.mac          | string | R        | 设备 MAC 地址                               |
| info.product.id   | int    | R        | 设备所属产品类型ID                          |
| info.product.desc | string | R        | 设备所属产品类型描述                        |
| info.name         | string | R        | 设备名称（用户定义的）                      |
| events            | list   | R        | 设备事件（数组，说明如下:设备事件字段说明） |

设备事件字段说明

| 参数名称     | 类型   | 出现要求 | 描述                                            |
| ------------ | ------ | -------- | ----------------------------------------------- |
| data         | object | R        | 触发事件的设备数据（说明如下:设备数据字段说明） |
| alert_config | object | C        | 触发的事件（说明如下:设备事件字段描述）         |
| status       | int    | C        | 事件发生或解除（1）                             |

设备数据字段说明
***注意：*** 不同类型的设备，data 字段返回的属性种类不一样，具体参考 [规范说明 - 2. 设备列表及属性支持说明](/main/specification#2-设备列表及属性支持说明)

| 参数名称    | 类型   | 出现要求 | 描述        |
| ----------- | ------ | -------- | ----------- |
| temperature | object | C        | 温度        |
| humidity    | object | C        | 湿度        |
| pressure    | object | C        | 气压        |
| battery     | object | C        | 电量        |
| timestamp   | object | C        | Unix 时间戳 |

设备数据子类描述
不同传感器种类输出的数据格式不同，所以该结构里的字段均为可选字段

| 参数名称 | 类型   | 出现要求 | 描述 |
| -------- | ------ | -------- | ---- |
| value    | float  | R        | 数字 |
| level    | string | C        | 等级 |
| status   | string | C        | 状态 |

设备事件字段描述
***注意：*** 不同类型的设备，events 的 alert_config.metric_name 支持的事件类型不一样，具体参考 [规范说明 - 2. 设备列表及属性支持说明](/main/specification#2-设备列表及属性支持说明)

| 参数名称    | 类型   | 出现要求 | 描述                           |
| ----------- | ------ | -------- | ------------------------------ |
| metric_name | string | R        | 事件属性（温度、湿度、电量等） |
| operator    | string | R        | 操作符（gt-大于、lt-小于）     |
| threshold   | int    | R        | 事件阈值                       |

### 3.2.2 数据样例

```json
{
    "signature": {
        "signature": "92c81f06fdd8e4149bd0db321bb863ebbb8b930c175480a2c9e96f1657ffa94c",
        "timestamp": 1594786965,
        "token": "d580f6e4-c652-11ea-b4e9-00163e2c48b3"
    },
    "payload": {
        "info": {
            "mac": "582D344C046C",
            "sn": "",
            "product": {
                "id": 1101,
                "desc": "青萍温湿度气压计"
            },
            "name": "Qingping Temp RH Barometer",
            "version": "",
            "created_at": 0
        },
        "events": [{
            "data": {
                "timestamp": {
                    "value": 1594714038
                },
                "battery": {
                    "value": 66
                },
                "temperature": {
                    "value": 29.1
                },
                "humidity": {
                    "value": 47.9
                },
                "pressure": {
                    "value": 99.37
                }
            },
            "alert_config": {
                "metric_name": "temperature",
                "operator": "gt",
                "threshold": 29
            },
            "status": 0
        }]
    }
}
```
