# Qingping Air Monitor Lite Access Instructions

- [Qingping Air Monitor Lite Access Instructions](#qingping-air-monitor-lite-access-instructions)
  - [1. Open API and Data Push Setting](#1-open-api-and-data-push-setting)
    - [1.1 Account Registration](#11-account-registration)
    - [1.2 Apply the App Key and App Secret of the Open API](#12-apply-the-app-key-and-app-secret-of-the-open-api)
    - [1.3 Register Webhooks（Optional）](#13-register-webhooksoptional)
    - [1.4 Register MQTT Information（Optional）](#14-register-mqtt-informationoptional)
  - [2. Description of Capability Support](#2-description-of-capability-support)

## 1. Open API and Data Push Setting

### 1.1 Account Registration

The Developer Platform only supports email or phone account now, please use email or phone to register on our platforms (Qingping IoT or Qingping+)

- If you need Qingping IoT, please register account here [Qingping IoT](https://qingpingiot.com/)
- If you need Qingping+, please refer to [Qingping](https://www.qingping.co/plus), download the application in the store.
- Please use the account of Qingping IoT to log in the developer platform, the website is [Qingping Developer Platform](https://developers.qingping.co/)

### 1.2 Apply the App Key and App Secret of the Open API

On  will add a Unix timestamp, you can apply the App Key and App Secret on the developer information management page.
The App Key and App Secret are used for getting the Access Token which is necessary when calling the Open API. The detail about the Open API, please refer to [Open API Specification](/main/openApi)

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
| MQTT Broker List   | MQTT Broker address (IP:Port)                                                                                                                                                                                                                                         |
| MQTT Client Prefix | The prefix of the client ID in MQTT Connection (Qingping Developer Platform will add a microsecond Unix timestamp after the prefix to avoid repetition, so the length of the prefix should not be longer than 8 bytes)                                                |
| Topic Data         | Used to push the near-real-time data of the devices after the devices reported them                                                                                                                                                                                   |
| Topic Event        | Used to push the events of the devices, includes online, offline, low power, alerts and so on. Different products support different events, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note) |

------

## 2. Description of Capability Support

| Capability                           | Supported or not | Description                                                                                                                                                                                 |
| ------------------------------------ | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Device Binding API                   | No               | Please using Qingping+ do this                                                                                                                                                              |
| Device Unbinding API                 | No               | Please using Qingping+ do this                                                                                                                                                              |
| Data Report Interval Setting API     | No               | The device reports sensor data every 15 minutes by default, will support to modify this soon                                                                                                |
| Device List API                      | Yes              | Get the device list of your account, refer to [Open API Specification - 1.3 Device List](/main/openApi#13-device-list)                                                                      |
| Device Data API                      | Yes              | Get the device data of your account, refer to [Open API Specification - 1.4 Device History Data](/main/openApi#14-device-history-data)                                                      |
| Device Event API                     | No               | Will be supported soon                                                                                                                                                                      |
| Webhook Setting                      | Yes              | Register the Webhooks, Qingping Developer Platform will push the device data and events to you in near-real-time, refer to [Webhook / MQTT Push Specification](/main/webhook)               |
| MQTT Setting                         | Yes              | Register MQTT information，Qingping Developer Platform will push the device data and events to you in near-real-time, refer to [Webhook / MQTT Push Specification](/main/webhook)           |
| Device Privatization (MQTT firmware) | Yes              | The device of MQTT firmware is supported to connect to your private cloud directly by configure the MQTT broker settings, refer to [Qingping Air Monitor Privatization](/main/snow_private) |
