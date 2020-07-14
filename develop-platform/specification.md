# 规范说明

- [规范说明](#规范说明)
  - [1. 接口规范](#1-接口规范)
    - [1.1 通信协议](#11-通信协议)
    - [1.2 请求方法](#12-请求方法)
    - [1.3 格式说明](#13-格式说明)
    - [1.4 请求接口返回状态说明](#14-请求接口返回状态说明)
    - [1.5 Oauth Access Token 获取](#15-oauth-access-token-获取)
    - [1.6 产品列表](#16-产品列表)
    - [1.7 属性列表](#17-属性列表)
    - [1.8 其他说明](#18-其他说明)

## 1. 接口规范

### 1.1 通信协议

```markdown
    协议: HTTPS
    域名: apis.cleargrass.com
```

### 1.2 请求方法

```markdown
    支持方式 GET POST DELETE PUT
```

### 1.3 格式说明

元素出现要求说明：

| 符号 | 说明                                        |
| ---- | ------------------------------------------- |
| R    | 报文中该元素必须出现（Required）            |
| O    | 报文中该元素可选出现（Optional）            |
| C    | 报文中该元素在一定条件下出现（Conditional） |

### 1.4 请求接口返回状态说明

请求返回的HTTP状态码说明如下

| 状态码 | 说明                           |
| ------ | ------------------------------ |
| 200    | 请求处理成功                   |
| 400    | 请求错误，缺失参数或格式不对等 |
| 401    | 缺少头部认证信息               |
| 403    | 请求的资源没有权限             |
| 404    | 请求的资源不存在               |
| 409    | 请求冲突                       |
| 408    | 请求时间过期，请重新请求       |
| 500    | 服务器内部错误                 |
| 503    | 服务暂时不可用                 |

### 1.5 Oauth Access Token 获取

所有请求均需要携带 Access Token 请求头，Access Token 获取方式参见 [接口授权](https://github.com/ClearGrass/QingpingDoc/blob/master/develop-platform/oauth_api.md)

### 1.6 产品列表

| 产品编号 | 名称                 | 支持属性列表                                                   |
| -------- | ------------------ |-------------------------------------------------------------|
| 1001     | 青萍商用温湿度计      | battery,timestamp,temperature,humidity                      |
| 1101     | 青萍温湿度计         | battery,timestamp,temperature,humidity,pressure              |
| 1201     | 青萍空气检测仪        |battery,timestamp,temperature,humidity,pressure,co2,pm25,tvoc|

### 1.7 属性列表

| 属性        | 属性     | 单位  |
| ----------- | -------- | ----- |
| battery     | 电量     | %     |
| timestamp   | 时间     | s     |
| temperature | 温度     | C     |
| humidity    | 湿度     | %     |
| pressure    | 气压     | kpa   |
| pm10        | PM1.0    | μg/m³ |
| pm25        | PM2.5    | μg/m³ |
| pm100       | PM10     | μg/m³ |
| co2         | Co2      | ppb   |
| tvoc        | 挥发物质 | ppb   |
| radon       | 氡       | pCi/L |

### 1.9 其他说明

1. 返回的其他信息以Json形式存放于Body中，头部类型为 "Content-Type: application/json"
2. 返回200以外的状态码时，错误信息格式如下：

```json
{
    "msg": "xx"
}
```
