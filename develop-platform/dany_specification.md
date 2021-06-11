# 青萍空气检测仪 Lite 对接说明

- [青萍空气检测仪 Lite 对接说明](#青萍空气检测仪-lite-对接说明)
  - [1. 开放接口及推送配置](#1-开放接口及推送配置)
    - [1.1 帐号注册](#11-帐号注册)
    - [1.2 申请开放接口 App Key 及 App Secret](#12-申请开放接口-app-key-及-app-secret)
    - [1.3 注册 Webhook 信息（可选）](#13-注册-webhook-信息可选)
    - [1.4 注册 MQTT 信息（可选）](#14-注册-mqtt-信息可选)
  - [2. 功能对接支持说明](#2-功能对接支持说明)

## 1. 开放接口及推送配置

### 1.1 帐号注册

开发者平台目前仅支持手机号或邮箱账号登录，在我们平台（青萍物联或青萍+二选一就可以，目前两平台账号相互独立，后续会进行整合）注册时请选用手机或邮箱注册登录

- 如需要注册 青萍物联 帐号，注册地址为：[青萍物联](https://qingpingiot.com/)
- 如需要注册 青萍+ 账号，请参考官网介绍 [青萍官网](https://www.qingping.co/plus)，前往应用商店下载和注册
- 请使用已经注册的手机或邮箱帐号登录青萍开发者平台，平台地址为：[青萍开发者平台](https://developers.qingping.co/)

### 1.2 申请开放接口 App Key 及 App Secret

请在青萍开发者平台，个人中心权限管理页面，申请 App Key 及 App Secret，申请成功可以在当前页面看到相关信息。

App Key 及 App Secret 用以通过开放接口获取设备相关信息，如：绑定设备、设备列表、设备数据、设备事件等，具体介绍见 [开放接口说明文档](/main/openApi)

### 1.3 注册 Webhook 信息（可选）

开发平台支持主动推送设备信息到合作企业平台，实时推送设备数据、设备事件，使企业可以快速获取到设备信息，以满足设备联动需求。

请在青萍开发者平台，个人中心 Webhook 页面，设置相应 Webhook 信息：

| Webhook  | 说明                                                                                                                           |
| -------- | ------------------------------------------------------------------------------------------------------------------------------ |
| 设备数据 | 用于推送设备实时数据（设备根据配置定时上报最新数据）                                                                           |
| 设备事件 | 用于推送设备事件，包括设备上线、设备下线、低电量、设备报警等，不同类型的产品，支持的事件类型不同，具体请参考相关设备对接页面。 |

### 1.4 注册 MQTT 信息（可选）

**注意：**MQTT 推送方式与 Webhook 方式功能一致，两者只能选其一

请在青萍开发者平台，个人中心 MQTT 页面，设置相应 MQTT 信息：

| 项目               | 说明                                                                                                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MQTT User Name     | MQTT 连接时所使用的用户名                                                                                                                                                                         |
| MQTT User Secret   | MQTT 连接时所使用的密钥                                                                                                                                                                           |
| MQTT Broker List   | MQTT 服务地址（IP地址:Port端口号）                                                                                                                                                                |
| MQTT Client Prefix | MQTT 连接时 Client ID 的前缀（平台默认会在此前缀后加一个毫秒Unix time时间戳以防止重复，此前缀长度不要超过8个字节）                                                                                |
| Topic Data         | 用于推送设备实时数据（设备根据配置定时上报最新数据）                                                                                                                                              |
| Topic Event        | 用于推送设备事件，包括设备上线、设备下线、低电量、设备报警等，不同类型的产品，支持的事件类型不同，具体请参考 [规范说明 - 2. 设备列表及属性支持说明](/main/specification#2-设备列表及属性支持说明) |

------

## 2. 功能对接支持说明

| 功能                        | 是否支持 | 说明                                                                                                                               |
| --------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| 设备绑定接口                | 否       | 请在 青萍+ APP 上进行设备绑定相关操作                                                                                              |
| 设备解绑接口                | 否       | 请在 青萍+ APP 上进行设备绑定相关操作                                                                                              |
| 配置设备数据上报间隔接口    | 否       | 设备默认15分钟上报一次最新数据，暂时不支持修改此配置，后续会支持                                                                   |
| 获取设备列表接口            | 是       | 获取帐号下设备列表信息，接口请参考 [开放接口说明文档 - 1.3 设备列表](/main/openApi#13-设备列表)                                    |
| 获取设备数据接口            | 是       | 获取帐号下设备的历史数据（包括最新数据）信息，接口请参考 [开放接口说明文档 - 1.4 设备历史数据](/main/openApi#14-设备历史数据)      |
| 获取设备事件接口            | 否       | 设备暂时不支持事件                                                                                                                 |
| Webhook 推送                | 是       | 如果设置了 Webhook 推送地址，平台会实时推送接收到的设备数据或事件，推送数据格式说明请参见 [Webhook / MQTT 推送说明](/main/webhook) |
| MQTT 推送                   | 是       | 如果设置了 MQTT 推送地址，平台会实时推送接收到的设备数据或事件，推送数据格式说明请参见 [Webhook / MQTT 推送说明](/main/webhook)    |
| 设备私有化（MQTT 协议版本） | 是       | MQTT 固件的检测仪支持配置后直连客户私有云，私有化接入说明请参见 [青萍空气检测仪私有化](/main/private/snow_private)                 |