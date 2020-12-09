# Qingping Air Monitor Privatization Instructions

- [Qingping Air Monitor Privatization Instructions](#qingping-air-monitor-privatization-instructions)
  - [1. Definition](#1-definition)
  - [2. Products](#2-products)
  - [3. Privatization Instructions](#3-privatization-instructions)
    - [3.1 Protocol](#31-protocol)
    - [3.2 Ali IoT Platform](#32-ali-iot-platform)
      - [3.2.1 Parameter](#321-parameter)
      - [3.2.2 Create Product](#322-create-product)
      - [3.2.3 RAM Access Control](#323-ram-access-control)
    - [3.2  Private MQTT Service](#32--private-mqtt-service)
  - [4. Data communication protocol](#4-data-communication-protocol)
    - [4.1 Modify Report Interval Setting](#41-modify-report-interval-setting)
    - [4.2 Report Data Format](#42-report-data-format)
    - [4.3 Response Data Format](#43-response-data-format)

## 1. Definition

Qingping Air Monitor privatization is the monitor connecting to the third part's cloud directly by setting the MQTT configuration.  
***Note:***  Some functions on the monitor depend on the HTTPs API, Such as Weather data, location, time, firmware upgrade. By default, these functions don't support privatization, please contact us if you need this.

## 2. Products

| Product                | Privatization Support | Firmware Version     |
| ---------------------- | --------------------- | -------------------- |
| Qingping Air Monitor   | Yes                   | 3.4.5_0148 and above |
| Qingping Air Monitor S | Yes                   | all                  |

## 3. Privatization Instructions

### 3.1 Protocol

Support: Ali IoT or MQTT

### 3.2 Ali IoT Platform

#### 3.2.1 Parameter

Submit the settings to QingOs platform.

| Parameter     | Description                                                                                                                                            |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| REGION ID     | The region of the Ali Iot Platform                                                                                                                     |
| ACCESS KEY    | Create one user in RAM of Ali Iot Platform and grant permission of “管理物联网平台(IoT)”, then you can get AccessKey and AccessSecret (refer to 3.2.3) |
| ACCESS SECRET | As above                                                                                                                                               |
| PRODUCT KEY   | Create product on Ali Iot Platform and get the ProductKey (refer to 3.2.2)                                                                             |

#### 3.2.2 Create Product

- Create Product: Product Type(Self-definition type), Node type(Direct connecting device), Networking mode(Wi-Fi), Data format(Transparent transmission/Customization), Authorization method(Device key).
- After creation, you can see the ProductKey in the list.

#### 3.2.3 RAM Access Control

- Create one user in RAM of Ali Iot Platform and grant permission of “管理物联网平台(IoT)”.
- Click the user information detail and create the AccessKey in the authorization management, then get AccessKey and AccessSecret.

### 3.2  Private MQTT Service

Provide these information to us for the monitors

| Parameter      | Description                                                   |
| -------------- | ------------------------------------------------------------- |
| host           | MQTT host                                                     |
| port           | MQTT port                                                     |
| username       | MQTT user name                                                |
| password       | MQTT user password                                            |
| clientId       | MQTT client id(should be different for each monitor)          |
| publishTopic   | Topic the monitor use to upload data                          |
| subscribeTopic | Topic which the monitor listen to and get message from server |

## 4. Data communication protocol

### 4.1 Modify Report Interval Setting

Please refer to [QingPing MQTT Communication Protocol - 3.16 Modify Data Report Interval](/main/private/public_mqtt#316-modify-data-report-interval)

### 4.2 Report Data Format

Please refer to [QingPing MQTT Communication Protocol - 3.17 Sensor Data Report](/main/private/public_mqtt#317-sensor-data-report)

### 4.3 Response Data Format

After receiving the data from the device, server must response to the device by sending ack message. The message format please refer to [QingPing MQTT Communication Protocol - 3.18 Server Response For History Data Report](/main/private/public_mqtt#318-server-response-for-history-data-report). Otherwise the device will report the same data in the next circle.
