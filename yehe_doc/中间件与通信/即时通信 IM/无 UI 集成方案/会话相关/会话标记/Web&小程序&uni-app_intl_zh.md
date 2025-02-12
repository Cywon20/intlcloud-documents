
## 功能描述
在某些场景下，您可能需要对会话进行标记，例如 "会话标星"、"会话折叠"、"会话隐藏"、“会话标记未读”等，您可以调用以下接口实现。
> ?
- 该功能仅对旗舰版客户开放，购买 [旗舰版](https://www.tencentcloud.com/document/product/1047/34577) 后可使用。
- 该功能仅v2.22.0及以上版本支持。

## 会话标记

### 标记会话
您可以调用 [markConversation](https://web.sdk.qcloud.com/im/doc/en/SDK.html#markConversation) 接口标记或者取消标记会话。
> ! 当用户标记了会话，SDK 只是简单记录了会话的标记值，并不会改变会话的底层逻辑，比如标记会话为 [CONV_MARK_TYPE_UNREAD](https://web.sdk.qcloud.com/im/doc/en/module-TYPES.html#.CONV_MARK_TYPE_UNREAD)，会话的底层的未读数并不会发生改变。

**接口**

<dx-codeblock>
:::  js

tim.markConversation(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| conversationIDList  | String | 会话 ID 列表 |
| markType   | String | 会话标记类型 |
| enableMark | Boolean | true 设置标记，false 取消标记 |

> ? SDK 提供了四个默认标记（"会话标星"、"会话折叠"、"会话隐藏"、“会话标记未读”），如果已有标记不能满足您的需求，您可以自定义扩展标记，扩展标记需要满足以下两个条件：
1、扩展标记值不能和已有的标记值冲突。
2、自定义标记值必须是 Math.power(2, n)（32 <= n < 64，即 n 必须大于等于 32 并且小于 64），比如自定义 Math.power(2, 32) 标记值表示 "iPhone 在线"。

**返回**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

// 会话标星
let promise = tim.markConversation({
  conversationIDList: ['GROUPtest', 'C2Cexample'],
  markType: TIM.TYPES.CONV_MARK_TYPE_STAR,
  enableMark: true
});
promise.then(function(imResponse) {
  // 会话标星成功
  const { successConversationIDList, failureConversationIDList } = imResponse.data;
  // successConversationIDList - 标星成功的会话 ID 列表
  // 获取会话列表
  const conversationList = tim.getConversationList(successConversationIDList);

  // failureConversationIDList - 标星失败的会话 ID 列表
  failureConversationIDList.forEach((item) => {
    const { conversationID, code, message } = item;
  });
}).catch(function(imError) {
  console.warn('markConversation error:', imError);
});

:::
</dx-codeblock>

### 监听会话标记变更通知
会话被标记或者取消标记后，会话 [Conversation](https://web.sdk.qcloud.com/im/doc/en/Conversation.html) 的 markList 字段会发生变更，您可以通过事件 [CONVERSATION_LIST_UPDATED](https://web.sdk.qcloud.com/im/doc/en/module-EVENT.html#.CONVERSATION_LIST_UPDATED) 监听会话变更通知。

**示例**

<dx-codeblock>
:::  js

let onConversationListUpdated = function(event) {
  console.log(event.data); // 包含 Conversation 实例的数组
};
tim.on(TIM.EVENT.CONVERSATION_LIST_UPDATED, onConversationListUpdated);

:::
</dx-codeblock>

### 拉取指定标记会话
您可以调用 [getConversationList](https://web.sdk.qcloud.com/im/doc/en/SDK.html#getConversationList) 接口拉取指定标记会话。

**示例**

<dx-codeblock>
:::  js

// 获取所有的“标星”会话
let promise = tim.getConversationList({ markType: TIM.TYPES.CONV_MARK_TYPE_STAR });
promise.then(function(imResponse) {
  const conversationList = imResponse.data.conversationList; // 会话列表
});

:::
</dx-codeblock>

<dx-codeblock>
:::  js

// 获取所有折叠的 C2C 会话
let promise = tim.getConversationList({
  markType: TIM.TYPES.CONV_MARK_TYPE_FOLD,
  type: TIM.TYPES.CONV_C2C
});
promise.then(function(imResponse) {
  const conversationList = imResponse.data.conversationList; // 会话列表
});

:::
</dx-codeblock>
