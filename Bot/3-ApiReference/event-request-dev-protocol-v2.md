## 事件请求开发协议

版本：2.0.0

### 大纲

* [简介](#1-简介)
* [Event Request](#2-Event-Request)
  * [HTTP-Header](#21-HTTP-Header)
  * [Request Body Syntax](#22-Request-Body-Syntax)
    * [Event Object](#221-Event-Object)
* [Event 命名规范](#3-Event-命名规范)
  * [Event Name](#31-Event-Name)
  * [ROSAI Events](#32-ROSAI-Events)

### 1. 简介

本文是对在[_Roobo开放平台_](https://ros.ai)上事件请求开发协议的详细描述。在当有某种事件发生时产生的，将事件转发给云端的AI策略引擎进行计算。

### 2. Event Request

#### 2.1 HTTP Header

```
POST / HTTP/1.1
Content-Type : application/json;charset=UTF-8
Host : your.application.endpoint
Content-Length :
Accept : application/json
Accept-Charset : utf-8
```

#### 2.2 Request Body Syntax

_Request_ 的整体协议定义如下所示：

| Name | Type | Description | Required |
| --- | --- | --- | --- |
| agentId | String | Access Key | Required |
| token | String | Token | Required |
| sessionId | String | 会话id，如果有clientIId，一般同clientId即可 | Required |
| events | [[]Event](#221-Event-Object) | 事件列表 | Required |
| clientId | String | 设备id | Optional |
| lang | Lang | 语种，默认"zh" | Optional |
| contexts | Context 对象 | 上文 | Optional |
| location | Location 对象 | 地理位置 | Optional |

```
{
    "agentId": "Your Access Key",
    "token": "Your Token",
    "sessionId": "sn1234567890",
    "clientId": "sn1234567890",
    "lang": "zh",
    "contexts": [
        {
            "service": "OrderCoffee",
            "context": "coffee",
            "parameters": {
                "contactId": {
                    "orgin":null,
                    "normType":"String",
                    "norm":"1486539416366"
                }
            }
        }
    ],
    "location": {
        "address": {
            "country": "中国",
            "province": "北京",
            "city": "北京",
            "detail": "北京朝阳区北苑家园121号"
        },
        "longitude": 39.9197477,
        "latitude": 116.432956
    },
    "events": [
        {
            "name": "事件名",
            "params": {
                // 事件参数
            }
        }
    ],
}
```

##### 2.2.1 Event Object

__Event__ 向所请求的CloudApp提供了当前的发生的事件信息，包含事件名和事件参数

| Name | Type | Description | Required |
| --- | --- | --- | --- |
| name | string | 事件名 | true |
| params | Map | 事件参数Key:Value结构，（Value只支持String,Int,Float,Bool） | true |

### 3 Event 命名规范

#### 3.1 Event Name

四段式命名规则，中间使用"."号分隔，每段命名规则同C变量。
{$developer}.{$source}.{$module}.{$eventname}

| Name | Description |
| --- | --- |
| {$developer}  | 开发商，例如ROSAI |
| {$source} | 事件来源，枚举类型[local, cloud] |
| {$module} | 模块，例如：audio_player |
| {$eventname} | 具体事件名，例如：PlaybackStarted |

#### 3.2 ROSAI Events
```
// 图像人脸检测事件
{
    "name":"ROSAI.local.image.human_face_detection",
    "params":{
        "yaw":-6.595666885375977,
        "pitch":17.07350730895996,
        "roll":-1.699707269668579
    }
}
```
