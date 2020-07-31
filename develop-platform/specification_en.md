# Specification

- [Specification](#specification)
  - [1. Open API Specification](#1-open-api-specification)
    - [1.1 Communication Protocol](#11-communication-protocol)
    - [1.2 Request Method](#12-request-method)
    - [1.3 Format Specification](#13-format-specification)
    - [1.4 Response Code of Open API Specification](#14-response-code-of-open-api-specification)
    - [1.5 Other Specification](#15-other-specification)
    - [1.6 Oauth 2.0 Access Token](#16-oauth-20-access-token)
  - [2. Products List and Support Note](#2-products-list-and-support-note)
    - [2.1 Products List](#21-products-list)
    - [2.2 Device Property List and Support Specification of Every Product](#22-device-property-list-and-support-specification-of-every-product)
    - [2.3 Device Event List and Support Specification of Every Product](#23-device-event-list-and-support-specification-of-every-product)

## 1. Open API Specification

### 1.1 Communication Protocol

```markdown
    Protocol: HTTPS
    Address: apis.cleargrass.com
```

### 1.2 Request Method

```markdown
    Supported: GET POST DELETE PUT
```

### 1.3 Format Specification

Parameters requirement in the requests:

| Symbol | Description                                                             |
| ------ | ----------------------------------------------------------------------- |
| R      | This parameter must be supplied（Required）                             |
| O      | This parameter may be supplied（Optional）                              |
| C      | This parameter will be supplied under certain conditions（Conditional） |

### 1.4 Response Code of Open API Specification

| Status | Description                                      |
| ------ | ------------------------------------------------ |
| 200    | Success                                          |
| 400    | Invalid request, parameter error or format error |
| 401    | Authorization missing in header                  |
| 403    | No permission                                    |
| 404    | Target not found                                 |
| 409    | Request conflict                                 |
| 408    | Request expired, please request again            |
| 500    | Server internal error                            |
| 503    | Service not available temporarily                |

### 1.5 Other Specification

1. Response content will be put in the body part, in Json formate. The header type is "Content-Type: application/json".
2. When the response code is not 200, format of error information is like this：

```json
{
    "msg": "xx"
}
```

### 1.6 Oauth 2.0 Access Token

Every request need an Access Token in the header. How to get the Access Token, please refer to [接口授权](/main/oauthApi)

------

## 2. Products List and Support Note

### 2.1 Products List

| Product Code | Product Name                     |
| ------------ | -------------------------------- |
| 1001         | Qingping Temp & RH Monitor Pro S |
| 1101         | Qingping Temp & RH Barometer     |
| 1201         | Qingping Air Monitor             |

### 2.2 Device Property List and Support Specification of Every Product

| Property         | Description                          | Field Label | Value     | Property Unit / Level Specification                                             | Qingping Temp & RH Monitor Pro S | Qingping Temp & RH Barometer | Qingping Air Monitor |
| ---------------- | ------------------------------------ | ----------- | --------- | ------------------------------------------------------------------------------- | :------------------------------: | :--------------------------: | :------------------: |
| battery          | device battery                       | value       | number    | %                                                                               |            &#x0221A;             |          &#x0221A;           |      &#x0221A;       |
|                  |                                      | status      | discharge | not plug in power                                                               |                                  |                              |      &#x0221A;       |
|                  |                                      | status      | charging  | plug in power                                                                   |                                  |                              |      &#x0221A;       |
| signal           | device signal                        | value       | number    | negative number, the larger the value, the stronger the signal, such as:-50>-90 |            &#x0221A;             |          &#x0221A;           |                      |
| timestamp        | time of the message                  | value       | number    | s                                                                               |            &#x0221A;             |          &#x0221A;           |      &#x0221A;       |
| temperature      | value of temperature sensor          | value       | number    | C                                                                               |            &#x0221A;             |          &#x0221A;           |      &#x0221A;       |
|                  |                                      | status      | abnormal  | sensor abnormal                                                                 |                                  |          &#x0221A;           |
|                  |                                      | status      | preparing | sensor in the preparation stage                                                 |                                  |                              |      &#x0221A;       |
|                  |                                      | status      | sampling  | sensor in sampling stage                                                        |                                  |          &#x0221A;           |
| prob_temperature | value of external temperature sensor | value       | number    | C                                                                               |            &#x0221A;             |                              |                      |
| humidity         | value of humidity sensor             | value       | number    | %                                                                               |            &#x0221A;             |          &#x0221A;           |      &#x0221A;       |
|                  |                                      | status      | abnormal  | sensor abnormal                                                                 |                                  |                              |                      | &#x0221A; |
|                  |                                      | status      | preparing | sensor in the preparation stage                                                 |                                  |          &#x0221A;           |
|                  |                                      | status      | sampling  | sensor in sampling stage                                                        |                                  |                              |      &#x0221A;       |
| pressure         | value of pressure sensor             | value       | number    | kpa                                                                             |                                  |          &#x0221A;           |                      |
| pm10             | PM1.0                                | value       | number    | μg/m³                                                                           |                                  |                              |      &#x0221A;       |
| pm50             | PM5.0                                | value       | number    | μg/m³                                                                           |                                  |                              |      &#x0221A;       |
| pm25             | PM2.5                                | value       | number    | μg/m³                                                                           |                                  |                              |      &#x0221A;       |
|                  |                                      | status      | abnormal  | sensor abnormal                                                                 |                                  |                              |                      | &#x0221A; |
|                  |                                      | status      | preparing | sensor in the preparation stage                                                 |                                  |          &#x0221A;           |
|                  |                                      | status      | idle      | sensor in the idle stage                                                        |                                  |          &#x0221A;           |
|                  |                                      | status      | sampling  | sensor in sampling stage                                                        |                                  |                              |      &#x0221A;       |
| pm100            | PM10                                 | value       | number    | μg/m³                                                                           |                                  |                              |                      |
| co2              | Co2                                  | value       | number    | ppb                                                                             |                                  |                              |      &#x0221A;       |
| tvoc             | value of TVOC sensor                 | value       | number    | ppb                                                                             |                                  |                              |      &#x0221A;       |
|                  |                                      | status      | abnormal  | sensor abnormal                                                                 |                                  |                              |                      | &#x0221A; |
|                  |                                      | status      | preparing | sensor in the preparation stage                                                 |                                  |                              |      &#x0221A;       |
|                  |                                      | status      | initing   | sensor in initialization stage                                                  |                                  |                              |                      | &#x0221A; |
|                  |                                      | status      | sampling  | sensor in sampling stage                                                        |                                  |                              |                      | &#x0221A; |
| radon            | value of radon sensor                | value       | number    | pCi/L                                                                           |                                  |                              |                      |

***Explain:*** &#x0221A; indicates that this property is supported by the product

### 2.3 Device Event List and Support Specification of Every Product

| Property         | Property Description                 | Operator | Threshold | Description                                                                                                                                                                       | Qingping Temp & RH Monitor Pro S | Qingping Temp & RH Barometer | Qingping Air Monitor |
| ---------------- | ------------------------------------ | -------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------: | :--------------------------: | :------------------: |
| battery          | low power event                      | lt       | 15        | battery is less than 15%                                                                                                                                                          |            &#x0221A;             |          &#x0221A;           |                      |
| timestamp        | offline event                        | gt       |           | the device has been offline for 2 report intervals (2 report interval is subjective, you can handle the offline event by yourself according to the last device data you received) |            &#x0221A;             |          &#x0221A;           |                      |
| temperature      | temperature alert                    | gt/lt    | number    | temperature is bigger or less than the threshold                                                                                                                                  |            &#x0221A;             |          &#x0221A;           |                      |
| humidity         | humidity alert                       | gt/lt    | number    | humidity is bigger or less than the threshold                                                                                                                                     |            &#x0221A;             |          &#x0221A;           |                      |
| pressure         | pressure alert                       | gt/lt    | number    | pressure is bigger or less than the threshold                                                                                                                                     |                                  |          &#x0221A;           |                      |
| prob_temperature | temperature alert of external sensor | gt/lt    | number    | temperature is bigger or less than the threshold                                                                                                                                  |            &#x0221A;             |                              |                      |

***Explain:*** &#x0221A; indicates that this property is supported by the product

拿到以后少一格电；不久又满格了；最后一格电量使用很久
虚电
电量曲线采集和拟合

8.15号 magpie
