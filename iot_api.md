
# 青萍开放平台 v1.0.0

## 1 规范说明

### 1.1 通信协议

    协议: HTTPS
    域名: apis.cleargrass.com

### 1.2 请求方法
    支持 GET POST DELETE PUT

### 1.3 注册账号(获取平台秘钥)
    请联系工作人员

### 1.4 格式说明
元素出现要求说明：

符号				|说明
:----:			    |:---
R				    |报文中该元素必须出现（Required）
O				    |报文中该元素可选出现（Optional）
C				    |报文中该元素在一定条件下出现（Conditional）

### 1.5 报文规范说明

1. 报文规范仅针对交易请求数据进行描述；  

2. 报文规范中请求报文的内容为Https请求报文中RequestData值的明文内容；

3. 报文规范分为请求报文和响应报文。请求报文描述由发起方，响应报文由报文接收方响应。

### 1.6 请求报文结构
接口只接收两类参数 **RequestData** 和 **SignData** ，其中RequestData为请求接口业务参数，SignData为签名必须参数。

#### 1.6.1 参数说明
**RequestData（请求内容）：** 其明文为每次请求的具体参数，采用 Parms 或 JSON 格式

**SignData（签名内容）：** 请求参数（明文）的HMAC加密字符串，用于校验RequestData是否合法。


#### 1.6.2 校验流程：
服务端接收到请求后首先对除signature字段外的其他所有请求参数进行UrlEncode 得到待签名的字符串SignStr，然后aceess对应的secretKey为秘钥对SignStr进行HMAC-MD5签名得sign，将签名值转为大写形式，比较服务端生成的sign与请求参数中的signature，如对比通过，视为合法请求，否则视为非法请求。

**HMAC-MD5签名示例：**

```
描述:
例如 请求中包含如下参数  
offset = 10
limit = 10
timestamp = 1573527615
access = "e8399e28046811eabff1525400a3644c"
secret = "6ecd1be782333777b7f4b5ae09448cf8"

UrlEncode(会根据key的ASCII码进行排序) 后得到的字符串signStr为
access=e8399e28046811eabff1525400a3644&limit=10&offset=10&timestamp=1573527615

以secret为秘钥 signStr通过HMAC-MD5得到的hash值的16进制表示为 9b6ea4dda122b7b5b8f3ea6e72a7c744,将hash值转为大写，即为签名值

```

Golang版：

```
/* HMAC-MD5签名 */

func Sign (params map[string]string,secret string) string {
    p := url.Values{}
	for k, v := range params {
		p.Add(k, v)
	}

	str := p.Encode()
	h := hmac.New(md5.New, []byte(secret))
	h.Write([]byte(str))
	sign := hex.EncodeToString(h.Sum(nil))
	sign = strings.ToUpper(sign)

	return sign
}

```


### 1.7 响应报文结构
#### 1.7.1 结构说明
所有接口响应均采用JSON格式，如无特殊说明，每次请求的返回值中，都包含下列字段：

参数名称						|类型		|出现要求	|描述  
:----						    |:---		|:------	|:---	
code						    |int		|R			|响应码，代码定义请见“附录A 响应吗说明”
msg							    |string		|R			|响应描述
timestamp					    |int		|R			|响应时间
data						    |object		|R			|每个接口特有的参数，详见每个接口定义


#### 1.7.2 响应报文示例

```
{
    "code":0,
    "msg":"success",
    "timestamp":1573483266,
    "data":{
        "device_id":"A10086",
    }
}
```


## 2. 接口定义

### 2.1 设备列表
- **接口说明：** 设备列表
- **接口方法：** GET
- **接口地址：** /v1/portal/apis/devices

#### 2.1.1 请求参数
  
参数名称						|类型		|出现要求	|描述  
:----						    |:---		|:------	|:---
&emsp;offset				    |int		|O			|偏移量
&emsp;limit				        |int		|O			|最大返回数据条数 不得超过20	
&emsp;access				    |string		|R			|accessKey 身份标识
&emsp;signature				    |string		|R			|签名值
&emsp;timestamp				    |int		|R			|时间戳


请求示例：

```
https://apis.cleargrass.com/v1/portal/apis/devices?access=384c104405bd11ea80cc525400797dcf&signature=2474888B70EB9AD04523998EFC7A2367&timestamp=1573612191
```


#### 2.1.2 返回结果

参数名称						         |类型		|出现要求	|描述  
:----						            |:---		|:------	|:---	
code						            |int		|R			|响应码，代码定义请见“附录A 响应吗说明”
msg						                |string		|R			|&nbsp;
timestamp						        |int		|R			|响应时间
data						            |object		|R			|数据正文
&emsp;total				                |int		|R			|设备总数
&emsp;devices				            |[]object	|R			|设备数据
&emsp;&emsp;device				        |object	    |R			|设备信息
&emsp;&emsp;&emsp;id				    |string		|R			|设备id
&emsp;&emsp;&emsp;name				    |string		|R			|设备名称
&emsp;&emsp;&emsp;mac				    |string		|R			|设备mac地址
&emsp;&emsp;&emsp;sn				    |string		|R			|设备序列号
&emsp;&emsp;&emsp;cg_sn				    |string		|R			|青萍序列号
&emsp;&emsp;&emsp;imei				    |string		|R			|imei
&emsp;&emsp;&emsp;version				|string		|R			|设备版本
&emsp;&emsp;&emsp;created_at			|string		|R			|设备注册时间
&emsp;&emsp;&emsp;status			    |[]string	|R			|设备状态 offline(离线) online(在线) alarm(警报) 
&emsp;&emsp;&emsp;product			    |object		|R			|产品信息
&emsp;&emsp;&emsp;&emsp;name			|string		|C			|产品名称
&emsp;&emsp;&emsp;&emsp;brand			|string		|C			|产品品牌
&emsp;&emsp;&emsp;&emsp;protocol		|string		|C			|产品协议
&emsp;&emsp;data				        |object	    |C			|设备数据
&emsp;&emsp;&emsp;battery				|object		|C			|电量
&emsp;&emsp;&emsp;&emsp;value		    |float		|C			|数值
&emsp;&emsp;&emsp;&emsp;alarm		    |bool		|C			|是否超限
&emsp;&emsp;&emsp;humidity				|object		|C			|湿度
&emsp;&emsp;&emsp;&emsp;value		    |float		|C			|数值
&emsp;&emsp;&emsp;&emsp;alarm		    |bool		|C			|是否超限
&emsp;&emsp;&emsp;pressure				|object		|C			|气压
&emsp;&emsp;&emsp;&emsp;value		    |float		|C			|数值
&emsp;&emsp;&emsp;&emsp;alarm		    |bool		|C			|是否超限
&emsp;&emsp;&emsp;temperature			|object		|C			|温度
&emsp;&emsp;&emsp;&emsp;value		    |float		|C			|数值
&emsp;&emsp;&emsp;&emsp;alarm		    |bool		|C			|是否超限
&emsp;&emsp;&emsp;timestamp			    |object		|C			|时间
&emsp;&emsp;&emsp;&emsp;value		    |float		|C			|数值
&emsp;&emsp;&emsp;&emsp;alarm		    |bool		|C			|是否超限

示例：

```
{
    "code": 0,
    "msg": "success",
    "timestamp": 1573483266,
    "data": {
        "total": 2,
        "devices":[
            {
                "device":{
                    "id": "03654079cb0911e981d5380025e8fb21",
                    "name": "温湿度气压计",
                    "mac": "58:2D:34:46:04:42",
                    "sn": "82869B172C688BED",
                    "cg_sn": "82869B172C688BED",
                    "imei": "82869B172C688BED",
                    "version": "1.0.1_0049",
                    "created_at": 1573034091,
                    "status":["offline"],
                    "product": {
                        "name":"温湿度气压计",
                        "brand":"青萍",
                        "protocol":"NB-IoT"
                    }
                "data":{
                    "battery": {
                        "value": 70,
                        "alarm": false
                    },
                    "humidity": {
                        "value": 22.2,
                        "alarm": false
                    },
                    "pressure": {
                        "value": 103.29,
                        "alarm": false
                    },
                    "temperature": {
                        "value": 20.8,
                        "alarm": false
                    },
                    "timestamp": {
                        "value": 1574565267,
                        "alarm": false
                    }
                }
            },
            {
                "device":{
                    "id": "03654079cb0911e981d5380025e8fb21",
                    "name": "温湿度气压计",
                    "mac": "58:2D:34:46:04:42",
                    "sn": "82869B172C688BED",
                    "version": "1.0.1_0049",
                    "created_at": 1573034091,
                    "status":["online","alarm"],
                    "product": {
                        "name":"温湿度气压计",
                        "brand":"青萍",
                        "protocol":"NB-IoT"
                    }
                "data":{
                    "battery": {
                        "value": 70,
                        "alarm": false
                    },
                    "humidity": {
                        "value": 22.2,
                        "alarm": false
                    },
                    "pressure": {
                        "value": 103.29,
                        "alarm": false
                    },
                    "temperature": {
                        "value": 20.8,
                        "alarm": false
                    },
                    "timestamp": {
                        "value": 1574565267,
                        "alarm": false
                    }
                }
            },
        ]
    }
}
```



### 2.2 设备历史数据
- **接口说明：** 设备列表
- **接口方法：** GET
- **接口地址：** /v1/portal/apis/devices/data

#### 2.2.1 请求参数
  
参数名称						|类型		|出现要求	|描述  
:----						|:---		|:------	|:---	
&emsp;device_id				|string		|R			|设备id
&emsp;start				    |int		|R			|开始时间
&emsp;end				    |int		|R			|结束时间
&emsp;asc				    |bool		|O			|排序 正序or倒叙
&emsp;offset				|int		|O			|偏移量
&emsp;limit				    |int		|O			|最大返回数量 不得超过200条
&emsp;access				|string		|R			|accessKey 身份标识
&emsp;signature				|string		|R			|签名值
&emsp;timestamp				|int		|R			|时间戳


请求示例：

```
 https://apis.cleargrass.com/v1/portal/apis/devices/data?access=384c104405bd11ea80cc525400797dcf&device_id=1CA179E2C998307B880C37D628120E4F&end=1573527615&limit=200&signature=4C9CC52584345CDCEF7298D79ABEC1DA&start=1573354814&timestamp=1573527615
```


#### 2.2.2 返回结果

参数名称						        |类型		|出现要求	|描述  
:----						            |:---		|:------	|:---	
code						            |int		|R			|响应码，代码定义请见“附录A 响应吗说明”
msg						                |string		|R			|&nbsp;
timestamp						        |int		|R			|响应时间
tags				                    |object		|R			|设备标识
&emsp;device_id				            |string		|C			|设备id
data						            |object		|R			|数据正文
&emsp;total				                |int		|R			|数据总数
&emsp;data				                |[]object	|R			|设备数据
&emsp;&emsp;fields				        |object		|R			|传感器数据
&emsp;&emsp;&emsp;battery				|float		|C			|电量 单位%
&emsp;&emsp;&emsp;humidity				|float		|C			|湿度 单位%
&emsp;&emsp;&emsp;pressure				|float		|C			|气压 单位kPa
&emsp;&emsp;&emsp;temperature			|float		|C			|温度 单位C



示例：

```
{
    "code": 0,
    "msg": "success",
    "timestamp": 1573483266,
    "data": {
        "total":200,
        "tags":{
            "device_id":"D7E5B17A4661685C25232F72815920E6",
            "product":"青萍温湿度气压计",
            "sn":"29D726A63B7D01B7",
            "version":"1.0.1_0051"
        }
        "data":[
            {  
                "fields": {
                    "battery":15,
                    "humidity":15.2,
                    "pressure":101.14,
                    "temperature":20
                },
                "timestamp": 1573483266
            },
            {
                "fields": {
                    "battery":15,
                    "humidity":15.2,
                    "pressure":101.14,
                    "temperature":20
                },
                "timestamp": 1573483266
            }
        ]
    }
}
```

### 2.3 设备历史事件
- **接口说明：** 设备列表
- **接口方法：** GET
- **接口地址：** /v1/portal/apis/devices/events

#### 2.3.1 请求参数
  
参数名称						|类型		|出现要求	|描述  
:----						|:---		|:------	|:---	
&emsp;device_id				|string		|O			|设备id
&emsp;start				    |int		|R			|开始时间
&emsp;end				    |int		|R			|结束时间
&emsp;offset				|int		|O			|偏移量
&emsp;limit				    |int		|O			|最大返回数量 不得超过50条
&emsp;access				|string		|R			|accessKey 身份标识
&emsp;signature				|string		|R			|签名值
&emsp;timestamp				|int		|R			|时间戳


请求示例：

```
https://apis.cleargrass.com/v1/portal/apis/devices/events?access=384c104405bd11ea80cc525400797dcf&end=1573527615&limit=2&signature=75EA7877F751D4EEA901E7C83659867D&start=1573334814&timestamp=1573527615
```


#### 2.3.2 返回结果

参数名称						        |类型		|出现要求	|描述  
:----						            |:---		|:------	|:---	
code						            |int		|R			|响应码，代码定义请见“附录A 响应吗说明”
msg						                |string		|R			|&nbsp;
timestamp						        |int		|R			|响应时间
data						            |object		|R			|数据正文
&emsp;total				                |int		|R			|数据总数
&emsp;data				                |[]object	|R			|设备数据
&emsp;&emsp;fields				        |object		|R			|传感器数据
&emsp;&emsp;&emsp;battery				|float		|C			|电量 单位%
&emsp;&emsp;&emsp;humidity				|float		|C			|湿度 单位%
&emsp;&emsp;&emsp;pressure				|float		|C			|气压 单位kPa
&emsp;&emsp;&emsp;temperature			|float		|C			|温度 单位C
&emsp;&emsp;tags				        |object		|R			|设备标识
&emsp;&emsp;&emsp;device_id				|string		|C			|设备id
&emsp;&emsp;setting				        |object		|R			|配置项
&emsp;&emsp;&emsp;metric				|string		|C			|数据项 humidity(湿度) pressure(气压) temperature(温度)
&emsp;&emsp;&emsp;operator				|string		|C			|操作符 gt(大于) lt(小于)
&emsp;&emsp;&emsp;threshold				|string		|C			|阈值


示例：

```
{
    "code": 0,
    "data": {
        "total": 32,
        "msg": "success",
        "time": 1573543244017
        "data": [
            {
                "fields": {
                    "battery": 15,
                    "humidity": 43.8,
                    "pressure": 101.42,
                    "temperature": 18.8,
                    "timestamp": 1573526891
                },
                "tags": {
                    "device_id": "90F48B58B1EB3E58AB024B0006D05BE6",
                    "device_name": "Wifi005",
                    "mac": "49:F9:FE:06:7F:E7",
                    "sn": "C71C723322DD5FCC"
                },
                "setting": {
                    "metric": "humidity",
                    "operator": "gt",
                    "threshold": 19
                },
                "timestamp": 1573526891
            },
            {
                "fields": {
                    "battery": 15,
                    "humidity": 43.8,
                    "pressure": 101.42,
                    "temperature": 18.8,
                    "timestamp": 1573526890
                },
                "tags": {
                    "device_id": "90F48B58B1EB3E58AB024B0006D05BE6",
                    "device_name": "Wifi005",
                    "mac": "49:F9:FE:06:7F:E7",
                    "sn": "C71C723322DD5FCC"
                },
                "setting": {
                    "metric": "temperature",
                    "operator": "gt",
                    "threshold": 16
                 },
                "timestamp": 1573526890
            }
        ]
    }
}
```


## 3 附录A 响应码说明

响应码	|说明  
:----	|:---
0		|请求成功
1		|非法请求
2		|参数错误
4		|没有数据
6		|签名错误
