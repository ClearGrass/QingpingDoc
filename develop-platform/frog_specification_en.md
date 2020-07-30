# Qingping Temp & RH Monitor Pro S Access Instructions

- [Qingping Temp & RH Monitor Pro S Access Instructions](#qingping-temp--rh-monitor-pro-s-access-instructions)
  - [1. Open API and Data Push Setting](#1-open-api-and-data-push-setting)
    - [1.1 Account Registration](#11-account-registration)
    - [1.2 Apply the App Key and App Secret of the Open API](#12-apply-the-app-key-and-app-secret-of-the-open-api)
    - [1.3 Register Webhooks（Optional）](#13-register-webhooksoptional)
    - [1.4 Register MQTT Information（Optional）](#14-register-mqtt-informationoptional)
  - [2. Description of Capability Support](#2-description-of-capability-support)

## 1. Open API and Data Push Setting

### 1.1 Account Registration

- Please register Qingping IoT account, the website is [Qingping IoT](https://qingpingiot.com/)
- Please use the account of Qingping IoT to log in the developer platform, the website is [Qingping Developer Platform](https://developers.qingping.co/)

### 1.2 Apply the App Key and App Secret of the Open API

On  will add a Unix timestamp, you can apply the App Key and App Secret on the developer information management page.
The App Key and App Secret are used for getting the Access Token which is necessary when calling the Open API. The detail about the Open API, please refer to [Open API](/main/openApi)

### 1.3 Register Webhooks（Optional）

By using webhooks,  will add a Unix timestamp can push the information of your devices to your platform. The data and events the devices report are supported now, so you can get these in near-real-time and do something more.

Please register the webhooks on the developer information management page:

| Webhook      | Description                                                                                                                                                                                                                                                           |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Device Data  | Used to push the near-real-time data of the devices after the devices reported them                                                                                                                                                                                   |
| Device Event | Used to push the events of the devices, includes online, offline, low power, alerts and so on. Different products support different events, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note) |

### 1.4 Register MQTT Information（Optional）

**Notice:** The purposes of MQTT and Webhooks are the same, you should choose one of them.

Please register MQTT information on the developer information management page:

| Item               | Description                                                                                                                                                                                                                                                           |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MQTT User Name     | User Name for MQTT Connection                                                                                                                                                                                                                                         |
| MQTT User Secret   | User Secret for MQTT Connection                                                                                                                                                                                                                                       |
| MQTT Broker List   | MQTT Broker address                                                                                                                                                                                                                                                   |
| MQTT Client Prefix | The prefix of the client ID in MQTT Connection (Qingping Developer Platform will add a microsecond Unix timestamp after the prefix to avoid repetition, so the length of the prefix should not be longer than 8 bytes)                                                |
| Topic Data         | Used to push the near-real-time data of the devices after the devices reported them                                                                                                                                                                                   |
| Topic Event        | Used to push the events of the devices, includes online, offline, low power, alerts and so on. Different products support different events, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note) |

------

## 2. Description of Capability Support

| Capability                       | Supported or not | Description                                                                                                                                                              |
| -------------------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Device Binding API               | No               | Only support binding device on [Qingping IoT](https://qingpingiot.com/)                                                                                                  |
| Device Unbinding API             | No               | Only support unbinding device on [Qingping IoT](https://qingpingiot.com/)                                                                                                |
| Data Report Interval Setting API | No               | Only support device setting on [Qingping IoT](https://qingpingiot.com/)                                                                                                  |
| Device List API                  | Yes              | Get the device list of your account, refer to [开放接口说明文档 - 1.3 设备列表](/main/openApi#13-设备列表)                                                               |
| Device Data API                  | Yes              | Get the device data of your account, refer to [开放接口说明文档 - 1.4 设备历史数据](/main/openApi#14-设备历史数据)                                                       |
| Device Event API                 | Yes              | Get the device events of your account, refer to [开放接口说明文档 - 1.5 设备历史事件](/main/openApi#15-设备历史事件)                                                     |
| Webhook Setting                  | Yes              | Register the Webhooks, Qingping Developer Platform will push the device data and events to you in near-real-time, refer to  [WebHook / MQTT 推送说明](/main/webhook)     |
| MQTT Setting                     | Yes              | Register MQTT information，Qingping Developer Platform will push the device data and events to you in near-real-time, refer to  [WebHook / MQTT 推送说明](/main/webhook) |
