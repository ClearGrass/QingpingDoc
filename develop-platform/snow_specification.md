# 青萍空气检测仪对接说明

- [青萍空气检测仪对接说明](#青萍空气检测仪对接说明)
  - [1. 设备界面 - 合作方入口](#1-设备界面---合作方入口)
  - [2. 开放接口及推送配置](#2-开放接口及推送配置)
    - [2.1 帐号注册](#21-帐号注册)
    - [2.2 申请开放接口 APP ID 及 APP Secret](#22-申请开放接口-app-id-及-app-secret)
    - [2.3 注册 Webhook 信息（可选）](#23-注册-webhook-信息可选)
    - [2.4 注册 MQTT 信息（可选）](#24-注册-mqtt-信息可选)
  - [3 设备交互动作](#3-设备交互动作)
    - [3.1 获取动态码](#31-获取动态码)
    - [3.2 绑定设备](#32-绑定设备)
    - [3.3 解绑设备](#33-解绑设备)
    - [3.4 配置设备上报数据间隔（可选）](#34-配置设备上报数据间隔可选)
    - [3.5 获取设备数据](#35-获取设备数据)
    - [3.6 设备 Webhook 推送](#36-设备-webhook-推送)
    - [3.7 设备 MQTT 推送](#37-设备-mqtt-推送)

## 1. 设备界面 - 合作方入口

在设备设置界面，智能联动选项中：

- 可以选择增加单独的合作企业入口，入口中可以展示企业介绍、企业APP信息等；
- 为了快速接入，可以考虑使用智能联动中“其他”标签页下的共用动态码入口；
- 如需要在设置顶层页面定制相关入口，请在平台反馈页面留言

素材准备等详细信息请在平台反馈页面留言，会有相关人员进行具体沟通

## 2. 开放接口及推送配置

### 2.1 帐号注册

- 请注册青萍物联帐号，注册地址为：[青萍物联](https://qingpingiot.com/)
- 请使用青萍物联帐号登陆青萍开发者平台，平台地址为：[青萍开发者平台](https://xxxx/)

### 2.2 申请开放接口 APP ID 及 APP Secret

请在青萍开发者平台，个人中心权限管理页面，申请 APP ID 及 APP Secret，申请成功可以在当前页面看到相关信息。

APP ID 及 APP Secret 用以通过开放接口获取设备相关信息，如：绑定设备、设备列表、设备数据、设备事件等，具体介绍见 [开放接口说明文档](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/open_api.md)

### 2.3 注册 Webhook 信息（可选）

开发平台支持主动推送设备信息到合作企业平台，实时推送设备数据、设备事件，使企业可以快速获取到设备信息，以满足设备联动需求。

请在青萍开发者平台，个人中心 Webhook 页面，设置相应 Webhook 信息：

- 设备数据 Webhook，用于推送设备实时数据（设备根据配置定时上报最新数据）
- 设备事件 Webhook，用于推送设备事件，包括设备上线、设备下线、低电量、设备报警等，不同类型的产品，支持的事件类型不同，具体请参考相关设备介绍页面。

### 2.4 注册 MQTT 信息（可选）

**注意：**MQTT 推送方式与 Webhook 方式功能一致，两者只能选其一

请在青萍开发者平台，个人中心 MQTT 页面，设置相应 MQTT 信息：

- MQTT User Name，MQTT 连接时所使用的用户名
- MQTT User Secret，MQTT 连接时所使用的密钥
- MQTT Broker List，MQTT 服务地址
- MQTT Client Prefix，MQTT 连接时 Client ID 的前缀（平台默认会在此前缀后加一个毫秒Unix time时间戳以防止重复，此前缀长度不要错过8个字节）
- Topic Data，用于推送设备实时数据（设备根据配置定时上报最新数据）
- Topic Event，用于推送设备事件，包括设备上线、设备下线、低电量、设备报警等，不同类型的产品，支持的事件类型不同，具体请参考相关设备介绍页面。


## 3 设备交互动作

### 3.1 获取动态码

请在 “1. 设备界面 - 合作方入口” 界面获取设备上的动态码，动态码有效期为5分钟，只支持使用一次。

### 3.2 绑定设备

调用设备绑定接口，绑定设备，接口说明见 [开放接口说明文档 - 1.1 绑定设备](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/open_api.md)

### 3.3 解绑设备

解除绑定可以通过一下两种方式：

- 可以在设备设置界面操作进行解绑
- 调用设备解绑接口，解除设备绑定，接口说明见 [开放接口说明文档 - 1.2 删除设备](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/open_api.md)

### 3.4 配置设备上报数据间隔（可选）

设备默认1分钟上报一次最新数据，如需更短间隔，请调用修改设备配置接口进行修改，接口说明见 [开放接口说明文档 - 1.6 修改设备配置](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/open_api.md)

### 3.5 获取设备数据

主动获取帐号下设备的历史数据（包括最新数据）信息，接口请参考[开放接口说明文档](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/open_api.md)

### 3.6 设备 Webhook 推送

如果设置了 Webhook 推送地址，平台会实时推送接收到的设备数据或事件，推送数据格式说明请参见 [WebHook / MQTT 推送说明](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/webhook.md)

### 3.7 设备 MQTT 推送

如果设置了 MQTT 推送地址，平台会实时推送接收到的设备数据或事件，推送数据格式说明请参见 [WebHook / MQTT 推送说明](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/webhook.md)
