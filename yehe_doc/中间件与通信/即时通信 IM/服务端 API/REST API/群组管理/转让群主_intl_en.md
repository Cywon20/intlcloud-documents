## Feature Description
-  This API is used by the app admin to transfer the ownership of a group.
-  For groups that do not have a group owner, an app admin can use this API to specify a member as the group owner.
-  The new group owner must be a member of the group.

## API Calling Description
### Applicable group types

| Group Type ID | Is this RESTful API Supported? |
|-----------|------------|
| Private | Yes. Same as Work (work group for friends) in the new version. |
| Public | Yes. |
| ChatRoom | Yes. Same as Meeting (temporary meeting group) in the new version. |
| AVChatRoom | No. See the notes below. |
|Community | Yes. |

These are the 4 built-in group types in IM. For detailed information, see the [Group System](https://intl.cloud.tencent.com/document/product/1047/33529).

>?AVChatRooms (livestreaming groups) do not support transferring group ownership. Error 10007 will be returned if this operation is performed on a group of this type.

### Sample request URL
```
https://xxxxxx/v4/group_open_http_svc/change_group_owner?sdkappid=88888888&identifier=admin&usersig=xxx&random=99999999&contenttype=json
```


### Request parameters
The list below contains only the parameters commonly used when calling this API and their descriptions. For more parameters, see [RESTful API Overview](https://intl.cloud.tencent.com/document/product/1047/34620).

| Parameter | Description |
| ------------------ | ------------------------------------ |
| https       | The request protocol is HTTPS, and the request method is POST.       |
| xxxxxx  | The country/region where your SDKAppID is located.<li>China:  `console.tim.qq.com `<li>Singapore:  `adminapisgp.im.qcloud.com `<li>Seoul: `adminapikr.im.qcloud.com`<li>Frankfurt: `adminapiger.im.qcloud.com`<li>India: `adminapiind.im.qcloud.com` |
| v4/group_open_http_svc/change_group_owner | Request API |
| sdkappid | SDKAppID assigned by the IM console when the application is created |
| identifier | The value must be the app admin account. For more information, see [App Admins](https://intl.cloud.tencent.com/document/product/1047/33517). |
| usersig | Signature generated by the app admin account. For details, see [Generating UserSig](https://intl.cloud.tencent.com/document/product/1047/34385). |
| random | A random 32-bit unsigned integer ranging from 0 to 4294967295 |
| contenttype | Request format. The value is always `json`. |

### Maximum calling frequency
The maximum calling frequency is 200 calls per second.
### Sample request packet

In this example, the ownership of a group is transferred, and the new owner of the group after the transfer must be a member of the group.
```
{
    "GroupId": "@TGS#1NVTZEAE4",  // ID of the group to be transferred (required)
    "NewOwner_Account": "peter" // ID of the new group owner (required)
}
```

### Request packet fields

| Field | Type | Required | Description |
|---------|---------|---------|---------|
| GroupId | String | Required | ID of the group to be transferred |
| NewOwner_Account | String | Required | ID of the new group owner |

### Sample response packet body

```
{
    "ActionStatus": "OK",
    "ErrorInfo": "",
    "ErrorCode": 0
}
```

### Response packet fields

| Field | Type | Description |
|---------|---------|---------|
| ActionStatus | String | The processing result of the request. OK: succeeded. FAIL: failed. |
| ErrorCode | Integer | Error code. `0` indicates that the request was successful, and any non-zero value indicates that the request failed. |
| ErrorInfo | String | Detailed error information |

## Error Codes

Unless a network error (such as error 502) occurs, the returned HTTP status code for this API is always 200. The specific error code and details can be found in the response packet fields such as `ErrorCode` and `ErrorInfo`.
For public error codes (60000 to 79999), see [Error Codes](https://intl.cloud.tencent.com/document/product/1047/34348).
The list below contains error codes specific to this API:

| Error Code | Description |
|---------|---------|
| 10002 | A system error occurred. Please try again or contact our technical support. |
| 10004 | A parameter is invalid. Based on the ErrorInfo field in the response packet, check whether the required fields have been specified, or whether the fields are set according to the protocol requirements. |
| 10007 | Insufficient operation permissions. Check whether the operator is the app admin. |
| 10010 | The group does not exist, or once existed but has been disbanded. |
| 10015 | The group ID is invalid. Be sure to use the correct group ID. |

## API Debugging Tool
Use the [online debugging tool for RESTful APIs](https://29294-22989-29805-29810.cdn-go.cn/api-test.html#v4/group_open_http_svc/change_group_owner) to debug this API.

## Reference
Recalling group messages ([v4/group_open_http_svc/group_msg_recall](https://intl.cloud.tencent.com/document/product/1047/34965))
