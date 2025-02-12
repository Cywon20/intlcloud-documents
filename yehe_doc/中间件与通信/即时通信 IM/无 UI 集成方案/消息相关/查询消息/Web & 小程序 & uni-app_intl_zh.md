## 功能描述

根据 messageID 可以调用 [findMessages](https://web.sdk.qcloud.com/im/doc/en/SDK.html#findMessage) 查询本地消息。
1. 只支持查询本地消息，例如接收到的消息或者调用拉取历史消息接口获取到的消息。
2. 不支持查询直播群（AVChatRoom）的消息，因为其消息不会保存在本地。

>! v2.18.0起支持。

**接口**

<dx-codeblock>
:::  js

tim.findMessage(messageID);

:::
</dx-codeblock>

**参数**

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| messageID          | String | 消息 ID |

**返回值**

消息实例 [Message](https://web.sdk.qcloud.com/im/doc/en//Message.html) 或 `null`。

**示例**

<dx-codeblock>
:::  js

// 查找消息，v2.18.0起支持
let message = tim.findMessage('144115217363870632-1647417469-77069006');
if (message) {
  // 读取 message 相关属性，如已读回执信息 readReceiptInfo
}

:::
</dx-codeblock>