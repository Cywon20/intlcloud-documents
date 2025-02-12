## Feature Description
Local message search is a feature essential for the application's improved user experience. It allows users to quickly and conveniently find the expected information from massive amounts of complex data. It can also be used as an operations tool to easily and efficiently navigate to extensive content.
>?
>- Only local messages can be searched for, for example, received messages or historical messages obtained after the API is called. 
>- The message search feature is supported only by v5.4.666 or later.
>- The local message search feature is only available on the IM Ultimate edition. To use it, purchase the [Ultimate edition](https://www.tencentcloud.com/document/product/1047/34577#.E5.8D.87.E7.BA.A7.E5.BA.94.E7.94.A8). For more information, see [Pricing](https://intl.cloud.tencent.com/document/product/1047/34350).
)。

## Message Search Class
### Message search parameter class
The message search parameter class is `V2TIMMessageSearchParam` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageSearchParam.html) / [iOS and macOS](https://im.sdk.qcloud.com/doc/en/interfaceV2TIMMessageSearchParam.html)). When searching for messages, the SDK will execute different search logics based on the object settings.

The `V2TIMMessageSearchParam` parameters are as described below:

| Parameter | Definition | Description |
| --- | --- | --- |
| keywordList | Keyword list | It can contain up to five keywords. When the message sender and the message type are not specified, the keyword list must be set; in other cases, it can be left empty. |
| keywordListMatchType | Match type of the keyword list | It can be set to `OR` or `AND`. Valid values: V2TIM_KEYWORD_LIST_MATCH_TYPE_OR (default), V2TIM_KEYWORD_LIST_MATCH_TYPE_AND. |
| senderUserIDList | `userID` used to send a message | It can contain up to five user IDs. |
| messageTypeList | Set of the message types to be searched for | If it is left empty, messages of all the supported types (excluding `V2TIMFaceElem` and `V2TIMGroupTipsElem`) will be searched for. For more information on the values of other types, see `V2TIMElemType` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessage.html#a00455865d1a14191b8c612252bf20a1c) / [iOS and macOS](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#a849af0e4698e8db9f227f9c8e54215b8)). |
| conversationID | Whether to search all the conversations or the specified conversation | If it is left empty, all the conversations will be searched; otherwise, the specified conversation will be searched. |
| searchTimePosition | Start time for the search | It is `0` by default, indicating to search from the current time point. It is a UTC timestamp in seconds. |
| searchTimePeriod | A past period of time starting from the start time | It is `0` by default, indicating not to limit the time range. It is in seconds. `24x60x60` represents a past day. |
| pageIndex | Page number | It is used to display the search result by page. `0` indicates the first page. |
| pageSize | Number of result items per page | It is used to display the search result by page. Set it to `0` if you don't want paged display. If too many result items are pulled at a time, a performance problem may occur. |

### Message search result class
The message search result class is `V2TIMMessageSearchResult` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageSearchResult.html) / [iOS and macOS](https://im.sdk.qcloud.com/doc/en/interfaceV2TIMMessageSearchResult.html)). The parameters are as described below:

| Parameter | Description | Remarks |
| --- | --- | --- |
| totalCount | Total number of the search result items | If the specified conversation is searched, the **total number of messages** that meet the search criteria will be returned.<br>If all the conversations are searched, the **total number of the conversations** to which messages that meet the search criteria belong will be returned. |
| messageSearchResultItems | Match type of the keyword list | If the specified conversation is searched, only the result items in the conversation will be included in the returned result list.<br>If all the conversations are searched, messages that meet the search criteria will be grouped by conversation ID and returned by page. |

Here, `messageSearchResultItems` is a list that contains the `V2TIMMessageSearchResultItem` objects ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageSearchResultItem.html) / [iOS and macOS](https://im.sdk.qcloud.com/doc/en/interfaceV2TIMMessageSearchResultItem.html)). The parameters are as described below:

| Parameter | Definition | Description |
| --- | --- | --- |
| conversationID | Conversation ID | – |
| messageCount | Number of messages | Number of messages that meet the search criteria and are found in the current conversation. |
| messageList | List of messages that meet the search criteria | If the specified conversation is searched, `messageList` is the list of messages in the conversation that meet the search criteria.<br>If all the conversations are searched, `messageList` may display either of the following results:<br>* If the number of matched messages in a conversation is greater than 1, `messageList` is empty, and you can display "{messageCount} relevant records" on the UI.<br>* If the number of matched messages in a conversation is equal to 1, `messageList` is the matched message, and you can display it on the UI and highlight the matched keyword. |


## Searching for the Messages in All the Conversations
When a user enters a keyword in the search box to search for messages, you can call `searchLocalMessages` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageManager.html#a9364c8a0c6a0899b17c0a479b8ca848a) / [iOS and macOS](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#a10878d0bd326b07ec6a605c5695c7de1)) to search for local messages of the IM SDK.

If you want to search all the conversations, you only need to leave `conversationID` in `V2TIMMessageSearchParam` empty or unspecified.

Sample code:
<dx-tabs>
::: Android
```java
List<String> keywordList = new ArrayList<>();
keywordList.add("abc");
keywordList.add("123");
V2TIMMessageSearchParam searchParam = new V2TIMMessageSearchParam();
// Set the keyword for the search
searchParam.setKeywordList(keywordList);
// Search for the data on page 0
searchParam.setPageIndex(0);
// Search for ten data entries per page
searchParam.setPageSize(10);
V2TIMManager.getMessageManager().searchLocalMessages(searchParam, new V2TIMValueCallback<V2TIMMessageSearchResult>() {
  @Override
  public void onSuccess(V2TIMMessageSearchResult v2TIMMessageSearchResult) {
    // Data found successfully
  }

  @Override
  public void onError(int code, String desc) {
    // Failed to find the data
  }
});
```
:::
::: iOS and macOS
```objectivec
V2TIMMessageSearchParam *param = [[V2TIMMessageSearchParam alloc] init];
// Set the keyword for the search
param.keywordList = @[@"abc", @"123"];
param.messageTypeList = nil;
param.conversationID = nil;
param.searchTimePosition = 0;
param.searchTimePeriod = 0;
// Search for the data on page 0
param.pageIndex = 0;
// Search for ten data entries per page
param.pageSize = 10;
[V2TIMManager.sharedInstance searchLocalMessages:param
                                            succ:^(V2TIMMessageSearchResult *searchResult) {
    // Data found successfully. `searchResult` returns the search result.
} fail:^(int code, NSString *desc) {
    // Failed to find the data
}];
```
:::
</dx-tabs>

## Searching for the Messages in the Specified Conversation
When a user enters a keyword in the search box to search for messages, you can call `searchLocalMessages` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageManager.html#a9364c8a0c6a0899b17c0a479b8ca848a) / [iOS and macOS](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#a10878d0bd326b07ec6a605c5695c7de1)) to search for local messages of the IM SDK.


Sample code:
<dx-tabs>
::: Android
```java
List<String> keywordList = new ArrayList<>();
keywordList.add("abc");
keywordList.add("123");
V2TIMMessageSearchParam searchParam = new V2TIMMessageSearchParam();
// Search for one-to-one messages with the `user1` user
searchParam.setConversationID("c2c_user1");
// Set the keyword for the search
searchParam.setKeywordList(keywordList);
// Search for the data on page 0
searchParam.setPageIndex(0);
// Search for ten data entries per page
searchParam.setPageSize(10);
V2TIMManager.getMessageManager().searchLocalMessages(searchParam, new V2TIMValueCallback<V2TIMMessageSearchResult>() {
  @Override
  public void onSuccess(V2TIMMessageSearchResult v2TIMMessageSearchResult) {
    // Data found successfully
  }

  @Override
  public void onError(int code, String desc) {
    // Failed to find the data
  }
});
```
:::
::: iOS and macOS
```objectivec
V2TIMMessageSearchParam *param = [[V2TIMMessageSearchParam alloc] init];
// Set the keyword for the search
param.keywordList = @[@"abc", @"123"];
param.messageTypeList = nil;
// Search for one-to-one messages with the `user1` user
param.conversationID = @"c2c_user1";
param.searchTimePosition = 0;
param.searchTimePeriod = 0;
// Search for the data on page 0
param.pageIndex = 0;
// Search for ten data entries per page
param.pageSize = 10;
[V2TIMManager.sharedInstance searchLocalMessages:param
                                            succ:^(V2TIMMessageSearchResult *searchResult) {
    // Data found successfully. `searchResult` returns the search result.
} fail:^(int code, NSString *desc) {
    // Failed to find the data
}];
```
:::
</dx-tabs>


## Typical Use Cases for the Search
In general IM chat software, a search UI is usually displayed as follows:

| Figure 1. Chat history | Figure 2. More chat history | Figure 3. Messages in the specified conversation |
|---------|---------|---------|
|<img src="https://main.qcloudimg.com/raw/dbd1a42199b1ec31bb620d42558f58bc.png" alt="" style="zoom:30%;" /> | <img src="https://main.qcloudimg.com/raw/97382a01f71299da55c729d8d7cbc56f.png" alt="" style="zoom:30%;" /> | <img src="https://main.qcloudimg.com/raw/1619faf73d148ab0447b1cb38bdef29c.png" alt="" style="zoom:30%;" /> |

The following describes how to implement the above scenarios through IM SDK's search APIs.

### Displaying the latest active conversations
As shown in Figure 1, a list of the latest three conversations to which the messages found belong is displayed at the bottom. The implementation method is as follows:
1. Set the search parameter `V2TIMMessageSearchParam`.
   * Set `conversationID` to `null`, which indicates to search all the conversations.
   * Set `pageIndex` to `0`, which indicates the first page of the conversations to which the messages found belong.
   * Set `pageSize` to `3`, which indicates the number of the latest conversations to be returned. Generally, the latest three conversations will be displayed on the UI.

2. Process the search callback result `V2TIMMessageSearchResult`.
   * `totalCount` indicates the number of all the conversations to which the matched messages belong.
   * The `messageSearchResultItems` list contains the information of the latest three conversations (that is, the `pageSize` parameter). Here, `messageCount` of the `V2TIMMessageSearchResultItem` element indicates the total number of messages found in the current conversation.
   * If the number of messages found is greater than 1, `messageList` is empty, and you can display "4 relevant records" on the UI. Here, `4` is `messageCount`.
   * If the number of messages found is equal to 1, `messageList` is the matched message, and you can display it on the UI and highlight the search keyword, for example, "test" in the above figure.


Sample code:

<dx-tabs>
::: Android
```java
List<String> keywordList = new ArrayList<>();
keywordList.add("test");
V2TIMMessageSearchParam v2TIMMessageSearchParam = new V2TIMMessageSearchParam();
// Setting `conversationID` to `null` is to search for messages in all conversations and the results will be classified by conversation
v2TIMMessageSearchParam.setConversationID(null);
v2TIMMessageSearchParam.setKeywordList(keywordList);
v2TIMMessageSearchParam.setPageSize(3);
v2TIMMessageSearchParam.setPageIndex(0);
V2TIMManager.getMessageManager().searchLocalMessages(v2TIMMessageSearchParam, new V2TIMValueCallback<V2TIMMessageSearchResult>() {
  @Override
  public void onSuccess(V2TIMMessageSearchResult v2TIMMessageSearchResult) {
    // Total number of matched conversations to which messages belong
    int totalCount = v2TIMMessageSearchResult.getTotalCount();
    // Last 3 messages classified by conversation
    List<V2TIMMessageSearchResultItem> resultItemList = v2TIMMessageSearchResult.getMessageSearchResultItems();
    for (V2TIMMessageSearchResultItem resultItem : resultItemList) {
      // Conversation ID
      String conversationID = resultItem.getConversationID();
      // Total number of messages matching the conversation
      int totalMessageCount = resultItem.getMessageCount();
      // List of messages. If `totalMessageCount` is greater than 1, the list is empty. If `totalMessageCount` is equal to 1, the list contains the current message.
      List<V2TIMMessage> v2TIMMessageList = resultItem.getMessageList();
    }
  }

  @Override
  public void onError(int code, String desc) {}
});
```
:::
::: iOS and macOS
```objectivec
V2TIMMessageSearchParam *param = [[V2TIMMessageSearchParam alloc] init];
param.keywordList = @[@"test"];
// Setting `conversationID` to `nil` is to search for messages in all conversations and the results will be classified by conversation
param.conversationID = nil;
param.pageIndex = 0;
param.pageSize = 3;
[V2TIMManager.sharedInstance searchLocalMessages:param succ:^(V2TIMMessageSearchResult *searchResult) {
  // Total number of matched conversations to which messages belong
  NSInteger totalCount = searchResult.totalCount;
  // Last 3 messages classified by conversation
  NSArray<V2TIMMessageSearchResultItem *> *messageSearchResultItems = searchResult.messageSearchResultItems;
  for (V2TIMMessageSearchResultItem *searchItem in messageSearchResultItems) {
    // Conversation ID
    NSString *conversationID = searchItem.conversationID;
    // Total number of messages matching the conversation
    NSUInteger messageCount = searchItem.messageCount;
    // Message list
    NSArray<V2TIMMessage *> *messageList = searchItem.messageList ?: @[];
  }
} fail:^(int code, NSString *desc) {
	// fail
}];
```
:::
</dx-tabs>


### Displaying the list of conversations to which the messages found belong
After the user clicks "More chat history" in Figure 1, the user will be redirected to Figure 2, which displays the list of conversations to which the messages found belong. The search parameter and search result are similar to those as described in the above scenario.

To avoid memory bloat, we strongly recommend you load the list of conversations by page.
For example, to load and display ten conversation results per page, set `V2TIMMessageSearchParam` as follows:
1. For the first call: Set `pageSize` to `10` and `pageIndex` to `0`. Call `searchLocalMessages` to get the message search result, parse and display it on the first page, and get the total number of conversations from `totalCount` in the result callback.
2. Calculate the number of pages: totalPage = (`totalCount` % `pageSize` == 0) ? (`totalCount` / `pageSize`) : (`totalCount` / `pageSize` + 1).
3. For the subsequent call: You can set the `pageIndex` parameter (`pageIndex` < `totalPage`) to query the search result on the specified subsequent page. 

Sample code:
<dx-tabs>
::: Android
```java
......
// Calculate the total number of pages, given that ten messages are displayed per page
int totalPage = (totalCount % 10 == 0) ? (totalCount / 10) : (totalCount / 10 + 1);
......

private void searchConversation(int index) {
  if (index >= totalPage) {
  	return;
  }
  List<String> keywordList = new ArrayList<>();
  keywordList.add("test");
  V2TIMMessageSearchParam v2TIMMessageSearchParam = new V2TIMMessageSearchParam();
  v2TIMMessageSearchParam.setConversationID(null);
  v2TIMMessageSearchParam.setKeywordList(keywordList);
  v2TIMMessageSearchParam.setPageSize(10);
  v2TIMMessageSearchParam.setPageIndex(index);
  V2TIMManager.getMessageManager().searchLocalMessages(v2TIMMessageSearchParam, new V2TIMValueCallback<V2TIMMessageSearchResult>() {
    @Override
    public void onSuccess(V2TIMMessageSearchResult v2TIMMessageSearchResult) {
    // Total number of matched conversations to which messages belong
    int totalCount = v2TIMMessageSearchResult.getTotalCount();
    // Calculate the total number of pages, given that ten messages are displayed per page
    int totalPage = (totalCount % 10 == 0) ? (totalCount / 10) : (totalCount / 10 + 1);
    // Information of messages classified by conversation
    List<V2TIMMessageSearchResultItem> resultItemList = v2TIMMessageSearchResult.getMessageSearchResultItems();
    for (V2TIMMessageSearchResultItem resultItem : resultItemList) {
      // Conversation ID
      String conversationID = resultItem.getConversationID();
      // Total number of messages matching the conversation
      int totalMessageCount = resultItem.getMessageCount();
      // List of messages. If `totalMessageCount` is greater than 1, the list is empty. If `totalMessageCount` is equal to 1, the list contains the current message.
      List<V2TIMMessage> v2TIMMessageList = resultItem.getMessageList();
    }

    @Override
    public void onError(int code, String desc) {}
  });
}

// Load the next page
public void loadMore() {
    searchConversation(++pageIndex);
}
```
:::
::: iOS and macOS
```objectivec
......
// Calculate the total number of pages, given that ten messages are displayed per page
NSInteger totalPage = (totalCount % 10 == 0) ? (totalCount / 10) : (totalCount / 10 + 1);
......

- (void)searchConversation:(NSUInteger)index {
  if (index >= totalPage) {
  	return;
  }
  V2TIMMessageSearchParam *param = [[V2TIMMessageSearchParam alloc] init];
  param.keywordList = @[@"test"];
  param.conversationID = nil;
  param.pageIndex = index;
  param.pageSize = 10;
  [V2TIMManager.sharedInstance searchLocalMessages:param succ:^(V2TIMMessageSearchResult *searchResult) {
    // Total number of matched conversations to which messages belong
    NSUInteger totalCount = searchResult.totalCount;
    // Calculate the total number of pages, given that ten messages are displayed per page
    NSUInteger totalPage = (totalCount % 10 == 0) ? (totalCount / 10) : (totalCount / 10 + 1);
    // Information of messages classified by conversation
    NSArray<V2TIMMessageSearchResultItem *> *messageSearchResultItems = searchResult.messageSearchResultItems;
    for (V2TIMMessageSearchResultItem *searchItem in messageSearchResultItems) {
      // Conversation ID
      NSString *conversationID = searchItem.conversationID;
      // Total number of messages matching the conversation
      NSUInteger totalMessageCount = searchItem.messageCount;
      // List of messages. If `totalMessageCount` is greater than 1, the list is empty. If `totalMessageCount` is equal to 1, the list contains the current message.
      NSArray<V2TIMMessage *> *messageList = searchItem.messageList ?: @[];
    }
  } fail:^(int code, NSString *desc) {
  // fail
  }];
}

// Load the next page
- (void)loadMore {
    [self searchConversation:++pageIndex];
}
```
:::
</dx-tabs>


### Displaying the Messages in the Specified Conversation
Unlike Figure 2, Figure 3 displays the list of messages found in the specified conversation.
To avoid memory bloat, we strongly recommend you load messages by page. For example, you can load and display ten message results per page:
1. Set the search parameter `V2TIMMessageSearchParam` as follows (`pageIndex` can be calculated in the same way as described in the previous section):
   * Set `conversationID` of the search parameter `V2TIMMessageSearchParam` as the ID of the conversation to be searched.
   * For the first call: Set `pageSize` to `10` and `pageIndex` to `0`. Call `searchLocalMessages` to get the message search result, parse and display it on the first page, and get the total number of conversations from `totalCount` in the result callback.
   * Calculate the number of pages: totalPage = (`totalCount` % `pageSize` == 0) ? (`totalCount` / `pageSize`) : (`totalCount` / `pageSize` + 1).
   * For the subsequent call: You can set the `pageIndex` parameter (`pageIndex` < `totalPage`) to query the search result on the specified subsequent page.
  
2. Process the search result `V2TIMMessageSearchResult`:
  * `totalCount` indicates the total number of matched messages in the conversation.
  * The `messageSearchResultItems` list contains only the result items in the conversation. Here, `messageCount` of the `V2TIMMessageSearchResultItem` element is the number of messages on the page, and `messageList` is the list of messages on the page.

Sample code:
<dx-tabs>
::: Android
```java
......
// Calculate the total number of pages, given that ten messages are displayed per page
int totalMessagePage = (totalMessageCount % 10 == 0) ? (totalMessageCount / 10) : (totalMessageCount / 10 + 1);
......

private void searchMessage(int index) {
  if (index >= totalMessagePage) {
  	return;
  }
  List<String> keywordList = new ArrayList<>();
  keywordList.add("test");
  V2TIMMessageSearchParam v2TIMMessageSearchParam = new V2TIMMessageSearchParam();
  v2TIMMessageSearchParam.setConversationID(conversationID);
  v2TIMMessageSearchParam.setKeywordList(keywordList);
  v2TIMMessageSearchParam.setPageSize(10);
  v2TIMMessageSearchParam.setPageIndex(index);
  V2TIMManager.getMessageManager().searchLocalMessages(v2TIMMessageSearchParam, new V2TIMValueCallback<V2TIMMessageSearchResult>() {
    @Override
    public void onSuccess(V2TIMMessageSearchResult v2TIMMessageSearchResult) {
    // Total number of messages matching the conversation
    int totalMessageCount = v2TIMMessageSearchResult.getTotalCount();
    // Calculate the total number of pages, given that ten messages are displayed per page
    int totalMessagePage = (totalMessageCount % 10 == 0) ? (totalMessageCount / 10) : (totalMessageCount / 10 + 1);
    // Information of the messages on the conversation page
    List<V2TIMMessageSearchResultItem> resultItemList = v2TIMMessageSearchResult.getMessageSearchResultItems();
    for (V2TIMMessageSearchResultItem resultItem : resultItemList) {
      // Conversation ID
      String conversationID = resultItem.getConversationID();
      // Number of messages on the current page
      int totalMessageCount = resultItem.getMessageCount();
      // List of messages on the current page
      List<V2TIMMessage> v2TIMMessageList = resultItem.getMessageList();
      }
    }

    @Override
    public void onError(int code, String desc) {
    }
  });
}
// Load the next page
public void loadMore() {
    searchMessage(++pageIndex);
}
```
:::
::: iOS and macOS
```objectivec
......
// Calculate the total number of pages, given that ten messages are displayed per page
NSInteger totalMessagePage = (totalMessageCount % 10 == 0) ? (totalMessageCount / 10) : (totalMessageCount / 10 + 1);
......

- (void)searchMessage:(NSUInteger)index {
    if (index >= totalMessagePage) {
            return;
    }
    V2TIMMessageSearchParam *param = [[V2TIMMessageSearchParam alloc] init];
    param.keywordList = @[@"test"];
    param.conversationID = conversationID;
    param.pageIndex = index;
    param.pageSize = 10;
    [V2TIMManager.sharedInstance searchLocalMessages:param succ:^(V2TIMMessageSearchResult *searchResult) {
        // Total number of messages matching the conversation
        NSUInteger totalMessageCount = searchResult.totalCount;
        // Calculate the total number of pages, given that ten messages are displayed per page
        NSUInteger totalMessagePage = (totalMessageCount % 10 == 0) ? (totalMessageCount / 10) : (totalMessageCount / 10 + 1);
        // Information of the messages on the conversation page
        NSArray<V2TIMMessageSearchResultItem *> *messageSearchResultItems = searchResult.messageSearchResultItems;
        for (V2TIMMessageSearchResultItem *searchItem in messageSearchResultItems) {
            // Conversation ID
            NSString *conversationID = searchItem.conversationID;
            // Number of messages on the current page
            NSUInteger totalMessageCount = searchItem.messageCount;
            // List of messages on the current page
            NSArray<V2TIMMessage *> *messageList = searchItem.messageList ?: @[];
        }
    } fail:^(int code, NSString *desc) {
        // fail
    }];
}

// Load the next page
- (void)loadMore {
    [self searchMessage:++pageIndex];
}
```
:::
</dx-tabs>

## Searching for a Custom Message
In general, if you call the `createCustomMessage(data)` API ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageManager.html#a5c2495d4b7ecd66e5636aeb865c17efd) / [iOS and macOS](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#a7a38c42f63a4e0c9e89f6c56dd0da316)) to create a custom message, the message cannot be found, as the SDK will save it as a binary data stream.

To enable a user to find a custom message, you need to call the `createCustomMessage(data, description, extension)` API ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageManager.html#a313b1ea616f082f535946c83edd2cc7f) / [iOS and macOS](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#a4395ae33520dcf53da3190d56931852d)) to create and send the custom message and put the text to be searched for in the `description` parameter.

If you have configured the offline push feature, after the `description` parameter is set, the custom message will be pushed offline, and the parameter content will be displayed on the notification bar.
If you don't need offline push, you can use the `disablePush` in the `V2TIMOfflinePushInfo` parameter of the `sendMessage` API ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageManager.html#a28e01403acd422e53e999f21ec064795) / [iOS and macOS](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#a681947465d6ab718da40f7f983740a21)).

If you don't want to display the content pushed on the notification bar as the text to be searched for, you can use `desc` in the `V2TIMOfflinePushInfo` parameter to set the push content.


## Searching for a Rich Media Message
Rich media messages include file, image, audio, and video messages.

For a file message, the filename is usually displayed on the UI. If you pass in the `fileName` parameter when calling `createFileMessage` to create a file message, `fileName` will be used as the content to be searched for and match the search keyword. If `fileName` is not set, the SDK will automatically extract the filename from `filePath` as the content to be searched for.
Both `fileName` and `filePath` will be saved in the local database and on the server, which can be searched for after being pulled on another device.

For image, audio, and video messages, there is no such name as `fileName`, and a thumbnail or duration is usually displayed on the UI; in this case, `keywordList` is invalid.
To enable a user to find such messages, you can specify `messageTypeList` as `V2TIM_ELEM_TYPE_IMAGE`/`V2TIM_ELEM_TYPE_SOUND`/`V2TIM_ELEM_TYPE_VIDEO` for categorized search.


