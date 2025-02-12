## 功能说明

App 后台可以通过该回调实时查看用户创建话题的信息，包括： 通知 App 后台有话题创建成功，App 后台可以据此进行数据同步等操作。

## 注意事项

- 要启用回调，必须配置回调 URL，本条回调协议和创建群组之后回调的开关为同一个，配置方法详见 [第三方回调配置](https://intl.cloud.tencent.com/document/product/1047/34520) 文档。
- 回调的方向是即时通信 IM 后台向 App 后台发起 HTTP POST 请求。
- App 后台在收到回调请求之后，务必校验请求 URL 中的参数 SDKAppID 是否是自己的 SDKAppID。
- 其他安全相关事宜请参考 [第三方回调简介：安全考虑](https://intl.cloud.tencent.com/document/product/1047/34354) 文档。

## 可能触发该回调的场景

- App 用户通过客户端创建话题成功
- App 管理员通过 REST API 创建话题成功

## 回调发生时机

话题创建成功之后。

## 接口说明

### 请求 URL 示例

以下示例中 App 配置的回调 URL 为 `https://www.example.com`。
**示例：**

```
https://www.example.com?SdkAppid=$SDKAppID&CallbackCommand=$CallbackCommand&contenttype=json&ClientIP=$ClientIP&OptPlatform=$OptPlatform
```

### 请求参数说明

| 参数 | 说明 |
| --- | --- |
| https | 请求协议为 HTTPS，请求方式为 POST |
| www.example.com | 回调 URL |
| SdkAppid | 创建应用时在即时通信 IM 控制台分配的 SDKAppID |
| CallbackCommand | 固定为 Group.CallbackAfterCreateTopic |
| contenttype | 固定值为 JSON |
| ClientIP | 客户端 IP，格式如：127.0.0.1 |
| OptPlatform | 客户端平台，取值参见 [第三方回调简介：回调协议](https://intl.cloud.tencent.com/document/product/1047/34354) 中 OptPlatform 的参数含义 |

### 请求包示例

```json
 {
    "CallbackCommand": "Group.CallbackAfterCreateTopic", // 回调命令
    "GroupId" : "@TGS#2J4SZEAEL",
    "TopicId"	: "@TGS#_@TGS#cQVLVHIM62CJ@TOPIC#_@TOPIC#cRTE3HIM62C5",
    "Operator_Account": "group_root", // 操作者
    "Owner_Account": "leckie", // 群主
    "Type": "Community", // 群组类型
    "Name": "MyFirstTopic", // 话题的名称
    "UserDefinedDataList": [ //用户新建话题时的自定义字段
        {
            "Key": "UserDefined1",
            "Value": "hello"
        },
        {
            "Key": "UserDefined2",
            "Value": "world"
        }
    ]
}
```

### 请求包字段说明

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| CallbackCommand | String | 回调命令 |
| GroupId | String | 操作的话题所属的群组ID |
| TopicId | string | 操作的话题ID |
| Operator_Account | String | 发起创建话题请求的操作者 UserID |
| Owner_Account | String | 请求创建话题所属群的群主 UserID |
| Type | String | 代表创建话题所属的群组类型，这里为Community |
| Name | String | 请求创建的话题的名称 |
| UserDefinedDataList | Array | 用户创建话题时的自定义字段，这个字段默认是没有的，需要开通，详见 [自定义字段](https://intl.cloud.tencent.com/document/product/1047/33529) |

### 应答包示例

App 后台同步数据后，发送回调应答包。

```json
{
    "ActionStatus": "OK",
    "ErrorInfo": "",
    "ErrorCode": 0 // 忽略回调结果
}
```

### 应答包字段说明

| 字段 | 类型 | 属性 | 	说明 |
| --- | --- | --- | --- |
| ActionStatus | String | 必填 | 请求处理的结果，OK 表示处理成功，FAIL 表示失败 |
| ErrorCode | Integer | 必填 | 错误码，此处填0表示忽略应答结果 |
| ErrorInfo | String | 必填 | 错误信息 |

## 参考
- [第三方回调简介](https://intl.cloud.tencent.com/document/product/1047/34354)
- REST API：[创建话题](https://intl.cloud.tencent.com/document/product/1047/49471)

