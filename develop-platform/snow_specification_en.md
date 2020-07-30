# Qingping Air Monitor Access Instructions

- [Qingping Air Monitor Access Instructions](#qingping-air-monitor-access-instructions)
  - [1. Partner entrance on device](#1-partner-entrance-on-device)
  - [2. Open API and Data Push Setting](#2-open-api-and-data-push-setting)
    - [2.1 Account Registration](#21-account-registration)
    - [2.2 Apply the App Key and App Secret of the Open API](#22-apply-the-app-key-and-app-secret-of-the-open-api)
    - [2.3 Register Webhooks（Optional）](#23-register-webhooksoptional)
    - [2.4 Register MQTT Information（Optional）](#24-register-mqtt-informationoptional)
  - [3. Description of Capability Support](#3-description-of-capability-support)
    - [3.1 Capability Support List](#31-capability-support-list)

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
- Please use the account of Qingping IoT to log in the developer platform, the website is [ will add a Unix timestamp](https://developers.qingping.co/)

### 2.2 Apply the App Key and App Secret of the Open API

On  will add a Unix timestamp, you can apply the App Key and App Secret on the developer information management page.
The App Key and App Secret are used for getting the Access Token which is necessary when calling the Open API. The detail about the Open API, please refer to [Open API](/main/openApi)

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

### 3.1 Capability Support List

| Capability                       | Supported or not | Description                                                                                                                                                                      |
| -------------------------------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Device Binding API               | Yes              | Bind the device to your account, refer to [开放接口说明文档 - 1.1 绑定设备](/main/openApi#11-绑定设备)                                                                           |
| Device Unbinding API             | Yes              | Unbind device from your account, refer to [开放接口说明文档 - 1.2 删除设备](/main/openApi#12-删除设备) <br> ***Notice: User can also unbind the device on the device***          |
| Data Report Interval Setting API | Yes              | The device reports  设备默认1分钟上报一次最新数据，如需更短间隔，请调用修改设备配置接口进行修改，接口说明见 [开放接口说明文档 - 1.6 修改设备配置](/main/openApi#16-修改设备配置) |
| 获取设备列表接口                 | Yes              | 获取帐号下设备列表信息，接口请参考 [开放接口说明文档 - 1.3 设备列表](/main/openApi#13-设备列表)                                                                                  |
| 获取设备数据接口                 | Yes              | 获取帐号下设备的历史数据（包括最新数据）信息，接口请参考 [开放接口说明文档 - 1.4 设备历史数据](/main/openApi#14-设备历史数据)                                                    |
| 获取设备事件接口                 | No               | 近期会进行支持                                                                                                                                                                   |
| Webhook 推送                     | Yes              | 如果设置了 Webhook 推送地址，平台会实时推送接收到的设备数据或事件，推送数据格式说明请参见 [WebHook / MQTT 推送说明](/main/webhook)                                               |
| MQTT 推送                        | Yes              | 如果设置了 MQTT 推送地址，平台会实时推送接收到的设备数据或事件，推送数据格式说明请参见 [WebHook / MQTT 推送说明](/main/webhook)                                                  |
