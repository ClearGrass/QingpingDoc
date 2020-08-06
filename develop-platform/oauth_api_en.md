# OAuth 2.0 Protocol Specification

- [OAuth 2.0 Protocol Specification](#oauth-20-protocol-specification)
  - [1. Apply the App Key and App Secret](#1-apply-the-app-key-and-app-secret)
  - [2. Get Access Token（Client Credentials Grant Type）](#2-get-access-tokenclient-credentials-grant-type)
    - [2.1 Communication Protocol](#21-communication-protocol)
    - [2.2 API Specification](#22-api-specification)
  - [2.3 Access Token Usage](#23-access-token-usage)
  - [2.4 Access Token Effective Time Specification](#24-access-token-effective-time-specification)

Access Token will be used when you call the Open API as a certificate of authorization. You can get the Access Token from the OAuth 2.0 server as follows.

## 1. Apply the App Key and App Secret

Please use Qingping account to log in the Qingping Developer Platform, apply the App Key and App Secret.

------

## 2. Get Access Token（Client Credentials Grant Type）

### 2.1 Communication Protocol

```markdown
    Protocol: HTTPS
    Address: oauth.cleargrass.com
    Endpoint: /oauth2/token
```

### 2.2 API Specification

Parameters for Access Token request of OAuth 2.0 Client Credentials Grant Type are as follows.

| Parameter            | Description              | Value                                                                                                       |
| -------------------- | ------------------------ | ----------------------------------------------------------------------------------------------------------- |
| Method               | HTTP request method      | POST                                                                                                        |
| Content-Type         | Header request parameter | application/x-www-form-urlencoded                                                                           |
| Authorization: Basic | Header request parameter | client_id:client_secret where client_id is App Key and client_secret is App Secret, and encoded with Base64 |
| grant_type           | Body request parameter   | client_credentials                                                                                          |
| scope                | Body request parameter   | device_full_access                                                                                          |

Response is in Json format with these values:

| Parameter     | Description                              | Value   |
| ------------- | ---------------------------------------- | ------- |
| access_token  | Access Token used to make Open API call  | String  |
| refresh_token | Ignore for Client Credentials Grant Type | Empty   |
| token_type    | Token type                               | String  |
| expires_in    | Remaining effective time in seconds）    | Integer |

## 2.3 Access Token Usage

Please put the Access Token in the request header for permission verification.

| Parameter             | Description              | Value        |
| --------------------- | ------------------------ | ------------ |
| Authorization: Bearer | Header request parameter | Access Token |

## 2.4 Access Token Effective Time Specification

Effective time after the Access Token is generated is 2 hours, please request new token before it expires.
