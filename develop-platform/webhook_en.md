# Webhook / MQTT Push Specification

By registering the Webhooks or MQTT information, Qingping Developer Platform can push the newest data or events to your server in near-real-time.

- [Webhook / MQTT Push Specification](#webhook--mqtt-push-specification)
  - [1. Basic Rule Description](#1-basic-rule-description)
    - [1.1 Webhook](#11-webhook)
    - [1.2 MQTT](#12-mqtt)
  - [2. Signature Rules](#2-signature-rules)
  - [3. Webhook Support List](#3-webhook-support-list)
  - [3.1 Device Data Push](#31-device-data-push)
    - [3.1.1 Data Structure Specification](#311-data-structure-specification)
    - [3.1.1 Data Demo](#311-data-demo)
  - [3.2 Device Event Push](#32-device-event-push)
    - [3.2.1 Event Structure Specification](#321-event-structure-specification)
    - [3.2.2 Event Demo](#322-event-demo)

## 1. Basic Rule Description

### 1.1 Webhook

- Data will be put in the body part of every HTTP/HTTPS request. They are in Json format with "Content-Type: application/json" in the request header.
- After getting the Webhook requests, you should make a response in 3 seconds. Otherwise Qingping Developer Platform will determine this request is timeout, this request will be in the delay-push status.
- When the response code from you is 200, Qingping Developer Platform will determine this request is done, otherwise this request will be in the delay-push status.
- In the delay-push status, Qingping Developer Platform will retry the request during 30 minutes at the following intervals before stop trying: 5 minutes, 15 minutes, 30 minutes. (If all requests are failed, the data will be aborted, but you can also get them by calling the Open API)
- If the 10-seconds-timeout ratio of your HTTP/HTTPS server is bigger than 50%, Qingping Developer Platform will fuse this Webhook address for 10 minutes, during this time no requests will be make. You can also get data by calling the Open API.

### 1.2 MQTT

- The data are always a string which is marshaled from the Json object, the Json object is same as the Webhook data part.
- If the 10-seconds-timeout ratio of your MQTT server is bigger than 50%, Qingping Developer Platform will fuse this MQTT address for 10 minutes, during this time no requests will be make. You can also get data by calling the Open API.

------

## 2. Signature Rules

To ensure the authenticity of every push request, Qingping Developer Platform will sign them and posts the signature along other parameters in the Json object. The signature part includes these three parameters:

| Parameter | Type   | Description                                               |
| --------- | ------ | --------------------------------------------------------- |
| timestamp | int    | Unix timestamp                                            |
| token     | string | Randomly generated string with length 36                  |
| signature | string | String with hexadecimal digits generate by HMAC algorithm |

To verify the request is originating from Qingping Developer Platform you need to:

- Concatenate timestamp and token values.
- Encode the resulting string with the HMAC algorithm (using your App Secret on Qingping Developer Platform and SHA256 digest mode).
- Compare the resulting hexdigest to the signature.
- Optionally, you can cache the token value locally and ignore other subsequent request with the same token. This will prevent replay attacks.
- Optionally, you can check if the timestamp is not too far from the current time.

------

## 3. Webhook Support List

## 3.1 Device Data Push

When the device reports data to Qingping Developer Platform, the platform will push the data to your server through the Webhook address.

### 3.1.1 Data Structure Specification

| Parameter         | Type   | Requirement | Description                                                               |
| ----------------- | ------ | ----------- | ------------------------------------------------------------------------- |
| info.mac          | string | R           | Mac address of the device                                                 |
| info.product.id   | int    | R           | Product code of the device                                                |
| info.product.desc | string | R           | Product name of the device                                                |
| info.name         | string | R           | User defined device name                                                  |
| data              | list   | R           | Device data, list formate, refer to "Device Data Parameter Specification" |

Device Data Parameter Specification
***Notice:*** Different product has different kinds of parameters, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note)

| Parameter   | Type   | Requirement | Description                                                                        |
| ----------- | ------ | ----------- | ---------------------------------------------------------------------------------- |
| temperature | object | C           | Temperature information object, refer to "Device Data Sub-Parameter Specification" |
| humidity    | object | C           | Humidity information object, refer to "Device Data Sub-Parameter Specification"    |
| pressure    | object | C           | Pressure information object, refer to "Device Data Sub-Parameter Specification"    |
| battery     | object | C           | Battery information object, refer to "Device Data Sub-Parameter Specification"     |
| timestamp   | object | C           | Unix timestamp object, refer to "Device Data Sub-Parameter Specification"          |

Device Data Sub-Parameter Specification

***Notice:*** Different sensor has different kinds of outputs, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note)

| Parameter | Type   | Requirement | Description        |
| --------- | ------ | ----------- | ------------------ |
| value     | float  | R           | Number from sensor |
| level     | string | C           | Level from sensor  |
| status    | string | C           | Status from sensor |

### 3.1.1 Data Demo

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

## 3.2 Device Event Push

When the device reports events(include alerts) to Qingping Developer Platform, the platform will push the events to your server through the Webhook address.

### 3.2.1 Event Structure Specification

| Parameter         | Type   | Requirement | Description                                                                 |
| ----------------- | ------ | ----------- | --------------------------------------------------------------------------- |
| info.mac          | string | R           | Mac address of the device                                                   |
| info.product.id   | int    | R           | Product code of the device                                                  |
| info.product.desc | string | R           | Product name of the device                                                  |
| info.name         | string | R           | User defined device name                                                    |
| events            | list   | R           | Device event, list formate, refer to "Device Event Parameter Specification" |

Device Event Parameter Specification

| Parameter    | Type   | Requirement | Description                                                                                   |
| ------------ | ------ | ----------- | --------------------------------------------------------------------------------------------- |
| data         | object | R           | Device data triggers this event, is an object, refer to "Device Data Parameter Specification" |
| alert_config | object | C           | Device event, is an object, refer to "Device Event Sub-Parameter Specification"               |
| status       | int    | C           | 事件发生或解除（1）                                                                           |

Device Data Parameter Specification
***Notice:*** Different product has different kinds of parameters, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note)

| Parameter   | Type   | Requirement | Description                                                                        |
| ----------- | ------ | ----------- | ---------------------------------------------------------------------------------- |
| temperature | object | C           | Temperature information object, refer to "Device Data Sub-Parameter Specification" |
| humidity    | object | C           | Humidity information object, refer to "Device Data Sub-Parameter Specification"    |
| pressure    | object | C           | Pressure information object, refer to "Device Data Sub-Parameter Specification"    |
| battery     | object | C           | Battery information object, refer to "Device Data Sub-Parameter Specification"     |
| timestamp   | object | C           | Unix timestamp object, refer to "Device Data Sub-Parameter Specification"          |

Device Data Sub-Parameter Specification

***Notice:*** Different sensor has different kinds of outputs, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note)

| Parameter | Type   | Requirement | Description        |
| --------- | ------ | ----------- | ------------------ |
| value     | float  | R           | Number from sensor |
| level     | string | C           | Level from sensor  |
| status    | string | C           | Status from sensor |

Device Event Sub-Parameter Specification
***Notice:*** Different product has different kinds of alert_config.metric_name for events, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note)

| Parameter   | Type   | Requirement | Description                               |
| ----------- | ------ | ----------- | ----------------------------------------- |
| metric_name | string | R           | Event type                                |
| operator    | string | R           | Operator（gt:greater than, lt:less than） |
| threshold   | int    | R           | User defined target                       |

### 3.2.2 Event Demo

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
