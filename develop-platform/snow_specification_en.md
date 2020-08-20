# Qingping Air Monitor Access Instructions

- [Qingping Air Monitor Access Instructions](#qingping-air-monitor-access-instructions)
  - [1. Partner entrance on device](#1-partner-entrance-on-device)
  - [2. Open API and Data Push Setting](#2-open-api-and-data-push-setting)
    - [2.1 Account Registration](#21-account-registration)
    - [2.2 Apply the App Key and App Secret of the Open API](#22-apply-the-app-key-and-app-secret-of-the-open-api)
    - [2.3 Register Webhooks（Optional）](#23-register-webhooksoptional)
    - [2.4 Register MQTT Information（Optional）](#24-register-mqtt-informationoptional)
  - [3. Description of Capability Support](#3-description-of-capability-support)

## 1. Partner entrance on device

If you are not a enterprise user who need API to binding device, please ignore this section.  
In the setting page of the device, "Automation" panel:

- You can decide to add an entrance of your company to show the information of your company and application.
- If you don't need the single entrance, you can also use the dynamic number in the "other" page to bind the device.
- If you want to customize the setting page, please contact us on the feedback page.

------

## 2. Open API and Data Push Setting

### 2.1 Account Registration

- Please register Qingping IoT account, the website is [Qingping IoT](https://qingpingiot.com/)
- Please use the account of Qingping IoT to log in the developer platform, the website is [Qingping Developer Platform](https://developers.qingping.co/)

### 2.2 Apply the App Key and App Secret of the Open API

On  will add a Unix timestamp, you can apply the App Key and App Secret on the developer information management page.
The App Key and App Secret are used for getting the Access Token which is necessary when calling the Open API. The detail about the Open API, please refer to [Open API Specification](/main/openApi)

### 2.3 Register Webhooks（Optional）

By using webhooks,  will add a Unix timestamp can push the information of your devices to your platform. The data and events the devices report are supported now, so you can get these in near-real-time and do something more.

Please register the webhooks on the developer information management page:

| Webhook      | Description                                                                                                                                                                                                                                                           |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Device Data  | Used to push the near-real-time data of the devices after the devices reported them                                                                                                                                                                                   |
| Device Event | Used to push the events of the devices, includes online, offline, low power, alerts and so on. Different products support different events, please refer to [Specification - 2. Products List and Support Note](/main/specification#2-products-list-and-support-note) |

### 2.4 Register MQTT Information（Optional）

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

## 3. Description of Capability Support

| Capability                           | Supported or not | Description                                                                                                                                                                                                                  |
| ------------------------------------ | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Device Binding API                   | Yes              | Bind the device to your account, refer to [Open API Specification - 1.1 Device Binding](/main/openApi#11-device-binding)                                                                                                     |
| Device Unbinding API                 | Yes              | Unbind device from your account, refer to [Open API Specification - 1.2 Device Unbinding](/main/openApi#12-device-unbinding) <br> ***Notice: User can also unbind the device on the device***                                |
| Data Report Interval Setting API     | Yes              | The device reports sensor data every 15 minutes by default, you can use this API to modify this setting, refer to [Open API Specification - 1.6 Device Settings Modification](/main/openApi#16-device-settings-modification) |
| Device List API                      | Yes              | Get the device list of your account, refer to [Open API Specification - 1.3 Device List](/main/openApi#13-device-list)                                                                                                       |
| Device Data API                      | Yes              | Get the device data of your account, refer to [Open API Specification - 1.4 Device History Data](/main/openApi#14-device-history-data)                                                                                       |
| Device Event API                     | No               | Will be supported soon                                                                                                                                                                                                       |
| Webhook Setting                      | Yes              | Register the Webhooks, Qingping Developer Platform will push the device data and events to you in near-real-time, refer to [Webhook / MQTT Push Specification](/main/webhook)                                                |
| MQTT Setting                         | Yes              | Register MQTT information，Qingping Developer Platform will push the device data and events to you in near-real-time, refer to [Webhook / MQTT Push Specification](/main/webhook)                                            |
| Device Privatization (MQTT firmware) | Yes              | The device of MQTT firmware is supported to connect to your private cloud directly by configure the MQTT broker settings, refer to [Qingping Air Monitor Privatization](/main/snow_private)                                  |
