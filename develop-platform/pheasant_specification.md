# 青萍温湿度气压计对接说明

- [青萍温湿度气压计对接说明](#青萍温湿度气压计对接说明)
  - [1. 开放接口及推送配置](#1-开放接口及推送配置)
    - [1.1 帐号注册](#11-帐号注册)
    - [1.2 申请开放接口 APP ID 及 APP Secret](#12-申请开放接口-app-id-及-app-secret)
    - [1.3 注册 Webhook 信息（可选）](#13-注册-webhook-信息可选)
    - [1.4 注册 MQTT 信息（可选）](#14-注册-mqtt-信息可选)
  - [2 设备交互动作](#2-设备交互动作)
    - [2.1 绑定或解绑设备](#21-绑定或解绑设备)
    - [2.2 配置设备上报数据间隔](#22-配置设备上报数据间隔)
    - [2.3 获取设备列表](#23-获取设备列表)
    - [2.4 获取设备数据](#24-获取设备数据)
    - [2.5 获取设备事件](#25-获取设备事件)
    - [2.6 设备 Webhook 推送](#26-设备-webhook-推送)
    - [2.7 设备 MQTT 推送](#27-设备-mqtt-推送)

## 1. 开放接口及推送配置

### 1.1 帐号注册

- 请注册青萍物联帐号，注册地址为：[青萍物联](https://qingpingiot.com/)
- 请使用青萍物联帐号登陆青萍开发者平台，平台地址为：[青萍开发者平台](https://xxxx/)

### 1.2 申请开放接口 APP ID 及 APP Secret

请在青萍开发者平台，个人中心权限管理页面，申请 APP ID 及 APP Secret，申请成功可以在当前页面看到相关信息。

APP ID 及 APP Secret 用以通过开放接口获取设备相关信息，如：设备列表、设备数据、设备事件等，具体介绍见 [开放接口说明文档](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/open_api.md)

### 1.3 注册 Webhook 信息（可选）

开发平台支持主动推送设备信息到合作企业平台，实时推送设备数据、设备事件，使企业可以快速获取到设备信息，以满足设备联动需求。

请在青萍开发者平台，个人中心 Webhook 页面，设置相应 Webhook 信息：

- 设备数据 Webhook，用于推送设备实时数据（设备根据配置定时上报最新数据）
- 设备事件 Webhook，用于推送设备事件，包括设备上线、设备下线、低电量、设备报警等，不同类型的产品，支持的事件类型不同，具体请参考相关设备介绍页面。

### 1.4 注册 MQTT 信息（可选）

**注意：**MQTT 推送方式与 Webhook 方式功能一致，两者只能选其一

请在青萍开发者平台，个人中心 MQTT 页面，设置相应 MQTT 信息：

- MQTT User Name，MQTT 连接时所使用的用户名
- MQTT User Secret，MQTT 连接时所使用的密钥
- MQTT Broker List，MQTT 服务地址
- MQTT Client Prefix，MQTT 连接时 Client ID 的前缀（平台默认会在此前缀后加一个毫秒Unix time时间戳以防止重复，此前缀长度不要错过8个字节）
- Topic Data，用于推送设备实时数据（设备根据配置定时上报最新数据）
- Topic Event，用于推送设备事件，包括设备上线、设备下线、低电量、设备报警等，不同类型的产品，支持的事件类型不同，具体请参考相关设备介绍页面。

## 2 设备交互动作

### 2.1 绑定或解绑设备

青萍温湿度气压计目前只支持在[青萍物联](https://qingpingiot.com/)平台进行此种操作。后续会进行支持。

### 2.2 配置设备上报数据间隔

青萍温湿度气压计目前只支持在[青萍物联](https://qingpingiot.com/)平台进行此种操作。后续会进行支持。

### 2.3 获取设备列表

获取帐号下设备列表信息，接口请参考[开放接口说明文档](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/open_api.md)

### 2.4 获取设备数据

主动获取帐号下设备的历史数据（包括最新数据）信息，接口请参考[开放接口说明文档](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/open_api.md)

### 2.5 获取设备事件

主动获取帐号下设备的历史事件信息，接口请参考[开放接口说明文档](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/open_api.md)

### 2.6 设备 Webhook 推送

如果设置了 Webhook 推送地址，平台会实时推送接收到的设备数据或事件，推送数据格式说明请参见 [WebHook / MQTT 推送说明](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/webhook.md)

### 2.7 设备 MQTT 推送

如果设置了 MQTT 推送地址，平台会实时推送接收到的设备数据或事件，推送数据格式说明请参见 [WebHook / MQTT 推送说明](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/webhook.md)
