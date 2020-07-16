## MQTT 通信协议

### 1.上报数据

#### 1.1主题

```sh
/productName/up/mac
如: /snow/up/582D34460584
```

#### 1.2数据格式

```go
type Attribute struct {
    Value     float32 `json:"value"`     // 数据值
    Unit      string  `json:"unit"`      // 单位
    Level     string  `json:"level"`     // 级别
    Status    string  `json:"status"`    // 状态
}

// 传感器数据有哪些属性填充那些数属性
type SensorDataAttrs struct {
    Battery     *Attribute     `json:"battery"`     // 电池
    Temperature *Attribute     `json:"temperature"` // 温度
    Humidity    *Attribute     `json:"humidity"`    // 湿度
    Pm1         *Attribute     `json:"pm1"`         // pm1
    Pm2         *Attribute     `json:"pm2"`         // pm2.5
    Pm5         *Attribute     `json:"pm5"`         // pm5
    Pm10        *Attribute     `json:"pm10"`        // pm10
    Co2         *Attribute     `json:"co2"`         // co2
    Tvoc        *Attribute     `json:"tvoc"`        // tvoc
    Radon       *Attribute     `json:"radon"`       // 氡
    Timestamp   *Attribute     `json:"timestamp"`   // 此数据的采集时间
}

// 设备配置
type Host struct {
    Host    string      `json:"host"`
    User    string      `json:"user"`
    Password   string   `json:"password"`
}

// 设备配置
type Setting struct {
    ReportInterval  int64           `json:"reportInterval"`
    CollectInterval int64           `json:"collectInterval"`
    AlterSettings   []*AlterSetting `json:"alterSettings"`
}

type AlterSetting struct {
    Metric    string  `json:"metric"`
    Operation string  `json:"operation"`
    Threshold float64 `json:"threshold"`
}

// 此为上报的最终结构
// 如出现网络问题导致堆积,时间取当时应该上报的时间,而不能多组数据使用一样的时间 FIXME
type SensorData struct {
    Mac        string               `json:"mac"`         // 设备mac
    MsgType    string               `json:"msgType"`     // 消息类型
    SensorData []*SensorDataAttrs   `json:"sensorData"`  // 传感器数据
    Settings   *Setting             `json:"settings"`    // 配置
    Version    string               `json:"version"`     // 版本号
    Timestamp  int                  `json:"timestamp"`   // 上报时间
    //Signature  string             `json:"signature"`   // 签名值 TODO(规则未定)
}

```

#### 1.3属性介绍

| 属性代码          | 属性     | 单位 |
| :---------------- | :------- | :--- |
| &emsp;battery     | 电量     | %    |
| &emsp;timestamp   | 时间     | s    |
| &emsp;temperature | 温度     | C    |
| &emsp;humidity    | 湿度     | %    |
| &emsp;pressure    | 气压     | kPa  |
| &emsp;pm10        | pm1      | ppm  |
| &emsp;pm25        | pm2.5    | ppm  |
| &emsp;pm100       | pm10     | ppm  |
| &emsp;co2         | 二氧化碳 | ppm  |
| &emsp;tvoc        | tvoc     | ppm  |
| &emsp;radon       | 氡       | ppm  |

* example上报数据

```json
主题 /snow/data/582D34460584
{
    "mac": "582D34460584",
    "vsersion": "1.2.001",
    "sensorData": [
        {
            "battery": {"value":99},
            "timestamp": {"value":1592192453},
            "temperature": {"value":25.1,"unit":"C"},
            "humidity": {"value":99,"unit":"%"},
            "pm10": {"value":99}
        },
        {
            "battery": {"value":99},
            "timestamp": {"value":1592192453},
            "temperature": {"value":25.1,"unit":"C"},
            "humidity": {"value":99,"unit":"%"},
            "pm10": {"value":99}
        }
    ],
    "timestamp": 1592192453
}
```

* example上报配置

```json
主题 /snow/data/582D34460584
{
    "mac": "582D34460584",
    "vsersion": "1.2.001",
    "settings": {
        "reportInterval": 10,
        "collectInterval": 5,
        "alterSettings": []
    },
    "timestamp": 1592192453
}
```

### 2.下发指令

#### 2.1主题

```sh
 /productName/down/mac 
 如: /snow/down/582D34460584
```

#### 2.2数据结构

```go
type Command struct {
    ID         string            `json:"id"`        // 唯一id
    Mac        string            `json:"mac"`       // 设备mac
    Type       string            `json:"type"`      // 命令类型id
    Desc       string            `json:"desc"`      // 命令描述
    Conf       Setting           `json:"setting"`   // 配置
    ServerHost Host              `json:"host"`      // 服务器地址信息
    Timestamp  int64             `json:"timestamp"` // 时间戳
}
```

#### 2.3.命令编号

| 命令编号  | 描述               | 参数列表                         |
| :-------- | :----------------- | :------------------------------- |
| &emsp;101 | 修改host(切换环境) | host,user,password               |
| &emsp;301 | 修改上报频率       | report_interval,collect_interval |
| &emsp;501 | 查询数据           | report_interval,collect_interval |
| &emsp;502 | 查询配置           |

* example 修改Host(切换环境)

```json
{
    "mac": "582D34460584",
    "type": "101",
    "desc": "change env",
    "host": {
        "host": "127.0.0.1",
        "user": "root",
        "password":"password"
    },
    "timestamp": 1592192453
}
```

* example 修改上报频率

```json
{
    "mac": "582D34460584",
    "type": "301",
    "desc": "change settings",
    "setting": {
        "report_interva;": 60,
        "collect_interval": 10
    },
    "timestamp": 1592192453
}
```
