# 接口授权

- [接口授权](#接口授权)
  - [1. 获取 App Key 及 App Secret](#1-获取-app-key-及-app-secret)
  - [2. 请求 Access Token（Client Credentials 方式）](#2-请求-access-tokenclient-credentials-方式)
    - [2.1 通信协议](#21-通信协议)
    - [2.2 接口说明](#22-接口说明)
  - [2.3 Access Token 使用说明](#23-access-token-使用说明)
  - [2.4 Access Token 有效期说明](#24-access-token-有效期说明)

开发者请求平台开放接口，均需要在头部携带一个 Access Token ，作为请求的授权凭证，以下为获取 Access Token 的方法说明。

## 1. 获取 App Key 及 App Secret

请使用青萍帐号登陆青萍开发者平台，申请 App Key 及 App Secret，然后可以在平台上查看相关信息。

------

## 2. 请求 Access Token（Client Credentials 方式）

### 2.1 通信协议

```markdown
    协议: HTTPS
    域名: oauth.qingpingcloud.com
    Token 路径: /oauth2/token
```

### 2.2 接口说明

获取 Access Token 采用 OAuth 2.0 的 Client Credentials 方式进行获取，请求参数说明如下：

| 参数                 | 说明            | 值                                                                         |
| -------------------- | --------------- | -------------------------------------------------------------------------- |
| Method               | HTTP 请求方法   | POST                                                                       |
| Content-Type         | Header 请求参数 | application/x-www-form-urlencoded                                          |
| Authorization: Basic | Header 请求参数 | client_id:client_secret 分别对应 App Key 与 App Secret， 需进行 Base64编码 |
| grant_type           | Body 请求参数   | client_credentials                                                         |
| scope                | Body 请求参数   | device_full_access                                                         |

返回结果为Json格式，各项键值对说明如下：

| 参数          | 说明                              | 值           |
| ------------- | --------------------------------- | ------------ |
| access_token  | 调用平台其他接口所需 Access Token | 字符串类型   |
| refresh_token | 可忽略                            | 空字符串类型 |
| token_type    | Token 类型                        | 字符串类型   |
| expires_in    | 剩余有效时间（秒）                | 整型         |

## 2.3 Access Token 使用说明

请将获取到的 Access Token 放入各请求的Header中，用于权限验证。

| 参数                  | 说明            | 值           |
| --------------------- | --------------- | ------------ |
| Authorization: Bearer | Header 请求参数 | Access Token |

## 2.4 Access Token 有效期说明

 Access Token 有效期为获取到开始2小时内，请在有效期结束前获取新的 Token。
