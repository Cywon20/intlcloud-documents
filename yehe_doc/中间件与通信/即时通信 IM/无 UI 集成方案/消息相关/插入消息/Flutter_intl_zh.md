## 功能描述
插入消息只会将消息插入本地数据库，不会发送到服务端。
> ! 如果用户切换手机登录，或程序卸载重装，之前插入的消息会丢失。

该接口主要用于满足向聊天会话中插入一些提示性消息的需求，例如 “您已经退出该群”、“请注意信息安全，不要在群聊中发送任何账号、密码和验证码等私密信息“ 等。这类消息有展示在聊天消息区的需求，但并没有发送给其他人的必要。

### 向单聊插入本地消息

您可以调用 `insertC2CMessageToLocalStorage` ([dart](https://comm.qq.com/im/doc/flutter/en/SDKAPI/Api/V2TIMMessageManager/insertC2CMessageToLocalStorage.html)) 向单聊插入本地消息。目前Flutter端仅支持插入自定义消息。

示例代码如下：


```dart
TencentImSDKPlugin.v2TIMManager.getMessageManager().insertC2CMessageToLocalStorage(data: "", userID: "", sender: "");
```



### 向群聊插入本地消息

您可以调用 `insertGroupMessageToLocalStorage` ([dart](https://comm.qq.com/im/doc/flutter/en/SDKAPI/Api/V2TIMMessageManager/insertGroupMessageToLocalStorage.html)) 向群聊插入本地消息。

示例代码如下：

```dart
TencentImSDKPlugin.v2TIMManager.getMessageManager().insertGroupMessageToLocalStorage(data: "", groupID: "", sender: "");
```

