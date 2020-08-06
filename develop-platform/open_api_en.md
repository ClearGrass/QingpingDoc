# Open API Specification

API default specification please refer to [Specification](/main/specification)  
The support of API SDK please refer to [Platform Access SDK Specification - 1. Open API SDK](/main/ccSdk#1-open-api-sdk)

- [Open API Specification](#open-api-specification)
  - [1. Device API](#1-device-api)
    - [1.1 Device Binding](#11-device-binding)
      - [1.1.1 Request Header](#111-request-header)
      - [1.1.2 Request Parameter](#112-request-parameter)
      - [1.1.3 Response Formate](#113-response-formate)
    - [1.2 Device Unbinding](#12-device-unbinding)
      - [1.2.1 Request Header](#121-request-header)
      - [1.2.2 Request Parameter](#122-request-parameter)
    - [1.3 Device List](#13-device-list)
      - [1.3.1 Request Header](#131-request-header)
      - [1.3.2 Request Parameter](#132-request-parameter)
      - [1.3.3 Response Formate](#133-response-formate)
    - [1.4 Device History Data](#14-device-history-data)
      - [1.4.1 Request Header](#141-request-header)
      - [1.4.2 Request Parameter](#142-request-parameter)
      - [1.4.3 Response Formate](#143-response-formate)
    - [1.5 Device History Event](#15-device-history-event)
      - [1.5.1 Request Header](#151-request-header)
      - [1.5.2 Request Parameter](#152-request-parameter)
      - [1.5.3 Response Formate](#153-response-formate)
    - [1.6 Device Settings Modification](#16-device-settings-modification)
      - [1.6.1 Request Header](#161-request-header)
      - [1.6.2 Request Parameter](#162-request-parameter)

## 1. Device API

### 1.1 Device Binding

- **Description:** Bind device to your account
- **HTTP Method:** POST
- **Endpoint:** /v1/apis/devices

#### 1.1.1 Request Header

| Parameter     | Value            | Description                                                                |
| :------------ | :--------------- | :------------------------------------------------------------------------- |
| Authorization | token            | Formate is "Bearer YourToken", There is a blank between "Bearer" and token |
| Content-Type  | application/json | Fixed value                                                                |

#### 1.1.2 Request Parameter

| Parameter    | Type   | Requirement | Description                                                                                    |
| :----------- | :----- | :---------- | :--------------------------------------------------------------------------------------------- |
| device_token | string | R           | Pairing code                                                                                   |
| product_id   | int    | R           | Product ID, refer to [Specification - 2.1 Products List](/main/specification#21-products-list) |
| timestamp    | int    | R           | Millisecond timestamp, unique for every request and expires in 20 second                       |

Request Demo:

```sh
https://apis.cleargrass.com/v1/apis/devices

{
    "device_token": "143412",
    "product_id": "1001"
}
```

#### 1.1.3 Response Formate

| Parameter         | Type   | Requirement | Description                                                                                    |
| :---------------- | :----- | :---------- | :--------------------------------------------------------------------------------------------- |
| info              | object | R           | Device information                                                                             |
| &emsp;name        | string | R           | Device name                                                                                    |
| &emsp;mac         | string | R           | Device Mac address                                                                             |
| &emsp;version     | string | R           | Device firmware version                                                                        |
| &emsp;created_at  | string | R           | Device register time                                                                           |
| &emsp;product     | object | R           | Product information                                                                            |
| &emsp;&emsp;id    | string | C           | Product ID, refer to [Specification - 2.1 Products List](/main/specification#21-products-list) |
| &emsp;&emsp;desc  | string | C           | Product Description                                                                            |
| data              | object | C           | Device data                                                                                    |
| &emsp;battery     | object | C           | Battery                                                                                        |
| &emsp;humidity    | object | C           | Humidity                                                                                       |
| &emsp;pressure    | object | C           | Pressure                                                                                       |
| &emsp;temperature | object | C           | Temperature                                                                                    |
| &emsp;timestamp   | object | C           | Time                                                                                           |
| &emsp;&emsp;value | float  | C           | Value                                                                                          |

***Notice:*** Different product has different kinds of data sub-parameters, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note)

Demo:

```json
{
    "info":{
        "name": "温湿度气压计",
        "mac": "582D34460442",
        "version": "1.0.1_0049",
        "created_at": 1573034091,
        "product": {"id":"10", "desc":"青萍温湿度气压计"}
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

### 1.2 Device Unbinding

- **Description:** Unbind the device from your account
- **HTTP Method:** DELETE
- **Endpoint:** /v1/apis/devices

#### 1.2.1 Request Header

| Parameter     | Value            | Description                                                                |
| :------------ | :--------------- | :------------------------------------------------------------------------- |
| Authorization | token            | Formate is "Bearer YourToken", There is a blank between "Bearer" and token |
| Content-Type  | application/json | Fixed value                                                                |

#### 1.2.2 Request Parameter

| Parameter | Type     | Requirement | Description                                                              |
| :-------- | :------- | :---------- | :----------------------------------------------------------------------- |
| mac       | []string | R           | Mac address                                                              |
| timestamp | int      | R           | Millisecond timestamp, unique for every request and expires in 20 second |

Request Demo:

```sh
https://apis.cleargrass.com/v1/apis/devices

{
    "mac": ["582D34460442"]
}
```

### 1.3 Device List

- **Description:** Get device list of your account
- **HTTP Method:** GET
- **Endpoint:** /v1/apis/devices

#### 1.3.1 Request Header

| Parameter     | Value | Description                                                                |
| :------------ | :---- | :------------------------------------------------------------------------- |
| Authorization | token | Formate is "Bearer YourToken", There is a blank between "Bearer" and token |

#### 1.3.2 Request Parameter

| Parameter | Type | Requirement | Description                                                              |
| :-------- | :--- | :---------- | :----------------------------------------------------------------------- |
| timestamp | int  | R           | Millisecond timestamp, unique for every request and expires in 20 second |
| offset    | int  | O           | Offset for the list                                                      |
| limit     | int  | O           | Max return number of devices, no bigger than 50                          |

Request Demo:

```sh
https://apis.cleargrass.com/v1/apis/devices?timestamp=1573612191
```

#### 1.3.3 Response Formate

| Parameter                | Type     | Requirement | Description                                                                                    |
| :----------------------- | :------- | :---------- | :--------------------------------------------------------------------------------------------- |
| total                    | int      | R           | Total number of devices                                                                        |
| devices                  | []object | R           | Device data                                                                                    |
| &emsp;info               | object   | R           | Device information                                                                             |
| &emsp;&emsp;name         | string   | R           | Device name                                                                                    |
| &emsp;&emsp;mac          | string   | R           | Device Mac address                                                                             |
| &emsp;&emsp;version      | string   | R           | Device firmware version                                                                        |
| &emsp;&emsp;created_at   | string   | R           | Device register time                                                                           |
| &emsp;&emsp;product      | object   | R           | Product information                                                                            |
| &emsp;&emsp;&emsp;id     | string   | C           | Product ID, refer to [Specification - 2.1 Products List](/main/specification#21-products-list) |
| &emsp;&emsp;&emsp;desc   | string   | C           | Product Description                                                                            |
| &emsp;data               | object   | C           | Device data                                                                                    |
| &emsp;&emsp;battery      | object   | C           | Battery                                                                                        |
| &emsp;&emsp;humidity     | object   | C           | Humidity                                                                                       |
| &emsp;&emsp;pressure     | object   | C           | Pressure                                                                                       |
| &emsp;&emsp;temperature  | object   | C           | Temperature                                                                                    |
| &emsp;&emsp;tvoc         | object   | C           | TVOC                                                                                           |
| &emsp;&emsp;co2          | object   | C           | CO2                                                                                            |
| &emsp;&emsp;pm25         | object   | C           | pm25                                                                                           |
| &emsp;&emsp;timestamp    | object   | C           | Time                                                                                           |
| &emsp;&emsp;&emsp;value  | float    | C           | Value                                                                                          |
| &emsp;&emsp;&emsp;level  | float    | C           | Level                                                                                          |
| &emsp;&emsp;&emsp;status | float    | C           | Status                                                                                         |

***Notice:*** Different product has different kinds of data sub-parameters, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note)

Demo:

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
                "status":["offline"],
                "product": {"id":1001, "desc":"青萍温湿度气压计"}
            }
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
                "status":["offline"],
                "product": {"id":1001, "desc":"青萍温湿度气压计"}
            }
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

### 1.4 Device History Data

- **Description:** History data of device
- **HTTP Method:** GET
- **Endpoint:** /v1/apis/devices/data

#### 1.4.1 Request Header

| Parameter     | Value | Description                                                                |
| :------------ | :---- | :------------------------------------------------------------------------- |
| Authorization | token | Formate is "Bearer YourToken", There is a blank between "Bearer" and token |

#### 1.4.2 Request Parameter

| Parameter  | Type   | Requirement | Description                                                              |
| :--------- | :----- | :---------- | :----------------------------------------------------------------------- |
| mac        | string | R           | Device Mac                                                               |
| start_time | int    | R           | Start time for the data                                                  |
| end_time   | int    | R           | End time for the data                                                    |
| timestamp  | int    | R           | Millisecond timestamp, unique for every request and expires in 20 second |
| offset     | int    | O           | Offset for the data list                                                 |
| limit      | int    | O           | Max number of return data in one request, no bigger than 200             |

Request Demo:

```sh
 https://apis.cleargrass.com/v1/apis/devices/data?mac=582D3446029C&end_time=1573527615&limit=200&start_time=1573354814&timestamp=1573527615
```

#### 1.4.3 Response Formate

| Parameter         | Type   | Requirement | Description                                     |
| :---------------- | :----- | :---------- | :---------------------------------------------- |
| total             | int    | R           | Total number of data items in the target period |
| data              | object | R           | Data item list                                  |
| &emsp;battery     | object | C           | Battery                                         |
| &emsp;humidity    | object | C           | Humidity                                        |
| &emsp;pressure    | object | C           | Pressure                                        |
| &emsp;temperature | object | C           | Temperature                                     |
| &emsp;timestamp   | object | R           | Timestamp for this data                         |
| &emsp;&emsp;value | float  | R           | Value                                           |

***Notice:*** Different product has different kinds of data sub-parameters, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note)

Demo:

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

### 1.5 Device History Event

- **Description:** History event of device
- **HTTP Method:** GET
- **Endpoint:** /v1/apis/devices/events

#### 1.5.1 Request Header

| Parameter           | Value | Description                                                                |
| :------------------ | :---- | :------------------------------------------------------------------------- |
| &emsp;Authorization | token | Formate is "Bearer YourToken", There is a blank between "Bearer" and token |

#### 1.5.2 Request Parameter

| Parameter        | Type   | Requirement | Description                                                              |
| :--------------- | :----- | :---------- | :----------------------------------------------------------------------- |
| &emsp;mac        | string | R           | Device Mac                                                               |
| &emsp;start_time | int    | R           | Start time for the events                                                |
| &emsp;end_time   | int    | R           | End time for the events                                                  |
| &emsp;timestamp  | int    | R           | Millisecond timestamp, unique for every request and expires in 20 second |
| &emsp;offset     | int    | O           | Offset for the event list                                                |
| &emsp;limit      | int    | O           | Max number of return events in one request, no bigger than 200           |

Request Demo:

```sh
https://apis.cleargrass.com/v1/apis/devices/events?mac=582D3446029C&end_time=1573527615&limit=200&start_time=1573354814&timestamp=1573527615
```

#### 1.5.3 Response Formate

| Parameter                | Type   | Requirement | Description                                 |
| :----------------------- | :----- | :---------- | :------------------------------------------ |
| total                    | int    | R           | Total number of events in the target period |
| events                   | object | R           | Event list                                  |
| &emsp;data               | object | C           | Device data                                 |
| &emsp;&emsp;battery      | object | C           | Battery                                     |
| &emsp;&emsp;humidity     | object | C           | Humidity                                    |
| &emsp;&emsp;pressure     | object | C           | Pressure                                    |
| &emsp;&emsp;temperature  | object | C           | Temperature                                 |
| &emsp;&emsp;timestamp    | object | R           | Timestamp for this data                     |
| &emsp;&emsp;&emsp;value  | float  | R           | Value                                       |
| &emsp;&emsp;&emsp;level  | float  | C           | Level                                       |
| &emsp;&emsp;&emsp;status | float  | C           | Status                                      |
| &emsp;alert_config       | object | C           | Alert condition                             |
| &emsp;&emsp;metric_name  | string | R           | Event type                                  |
| &emsp;&emsp;operator     | string | R           | Operator (gt: greater than, lt: less than） |
| &emsp;&emsp;threshold    | float  | R           | User defined threshold                      |

***Notice:*** Different product has different kinds of alert_config.metric_name for events, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note)

Demo:

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

### 1.6 Device Settings Modification

- **Description:** Modify the device settings
- **HTTP Method:** PUT
- **Endpoint:** /v1/apis/devices/settings

#### 1.6.1 Request Header

| Parameter     | Value            | Description                                                                |
| :------------ | :--------------- | :------------------------------------------------------------------------- |
| Authorization | token            | Formate is "Bearer YourToken", There is a blank between "Bearer" and token |
| Content-Type  | application/json | Fixed value                                                                |

#### 1.6.2 Request Parameter

| Parameter        | Type     | Requirement | Description                                                                             |
| :--------------- | :------- | :---------- | :-------------------------------------------------------------------------------------- |
| mac              | []string | R           | Mac address                                                                             |
| report_interval  | int      | R           | Report interval (second), minimum is 10 second, an integer multiple of collect_interval |
| collect_interval | int      | R           | data collection and report interval (second)                                            |
| timestamp        | int      | R           | Millisecond timestamp, unique for every request and expires in 20 second                |

Request Demo:

```sh
https://apis.cleargrass.com/v1/apis/devices/settings

{
    "mac": ["582D34460442"],
    "report_interval": 10,
    "collect_interval": 5
}
```
