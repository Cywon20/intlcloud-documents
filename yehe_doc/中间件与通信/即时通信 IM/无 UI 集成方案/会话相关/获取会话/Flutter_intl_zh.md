## 功能描述
IM SDK 提供获取会话的接口，可以获取指定的单个、多个会话的 `V2TimConversation` 对象信息。

### 获取指定的单个会话
您可以调用 `getConversation`([dart](https://comm.qq.com/im/doc/flutter/en/SDKAPI/Api/V2TIMConversationManager/getConversation.html)) 获取单个会话的信息，它是一个 `V2TimConversation` 对象。

示例代码如下：


```dart
V2TimValueCallback<V2TimConversation> conv = await conversationManager.getConversation(conversationID: "conversationID");
```


### 获取指定的多个会话

`getConversationList`([dart](https://comm.qq.com/im/doc/flutter/en/SDKAPI/Api/V2TIMConversationManager/getConversationList.html)) 获取指定的会话列表，列表中存储的是 `V2TimConversation` 对象。

示例代码如下：


```dart
V2TimValueCallback<V2TimConversationResult> convList = await conversationManager.getConversationList(nextSeq: '', count: 10);
```



