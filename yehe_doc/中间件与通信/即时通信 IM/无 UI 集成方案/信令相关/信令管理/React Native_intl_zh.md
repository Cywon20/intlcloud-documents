## 概述

信令接口是基于 IM 消息提供的一套邀请流程控制的接口，可以实现多种实时场景，例如：

- 直播聊天室中进行上麦、下麦管理。
- 聊天场景中实现类似微信中的音视频通话功能。
- 教育场景中老师邀请同学们举手、发言的流程控制。

## 功能

信令接口支持以下功能：

### 单聊邀请

在使用 [简单收发消息接口](https://intl.cloud.tencent.com/document/product/1047/36359#.E6.94.B6.E5.8F.91.E7.AE.80.E5.8D.95.E6.B6.88.E6.81.AF) 或 [富媒体消息接口](https://intl.cloud.tencent.com/document/product/1047/36359#.E6.94.B6.E5.8F.91.E5.AF.8C.E5.AA.92.E4.BD.93.E6.B6.88.E6.81.AF) 进行单聊的同时，可以使用 [invite](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/invite.html) 信令接口进行点对点呼叫，对方收到邀请通知 [onReceiveNewInvitation](https://comm.qq.com/im/doc/RN/en/Callback/OnReceiveNewInvitation.html) 后可以选择接受、拒绝或等待超时。

### 群聊邀请

首先需通过 [建群](https://intl.cloud.tencent.com/document/product/1047/34328#.E5.88.9B.E5.BB.BA.E7.BE.A4.E7.BB.84)、[加群](https://intl.cloud.tencent.com/document/product/1047/34328#.E5.8A.A0.E5.85.A5.E7.BE.A4.E7.BB.84)、[退群](https://intl.cloud.tencent.com/document/product/1047/34328#.E9.80.80.E5.87.BA.E7.BE.A4.E7.BB.84)、[解散群](https://intl.cloud.tencent.com/document/product/1047/34328#.E8.A7.A3.E6.95.A3.E7.BE.A4.E7.BB.84)以及[群资料](https://intl.cloud.tencent.com/document/product/1047/34328#.E7.BE.A4.E8.B5.84.E6.96.99.E5.92.8C.E7.BE.A4.E8.AE.BE.E7.BD.AE) 和 [群成员](https://intl.cloud.tencent.com/document/product/1047/34328#.E7.BE.A4.E6.88.90.E5.91.98.E7.AE.A1.E7.90.86) 相关接口完成对群组的管理，并监听群内的相关事件回调 [V2TimGroupListener](https://comm.qq.com/im/doc/RN/en/Interface/Listener/V2TimGroupListener.html)。然后群成员可以在群内发起群呼叫邀请 [inviteInGroup](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/invite.htmlInGroup)，被邀请的群成员会收到邀请通知 [onReceiveNewInvitation](https://comm.qq.com/im/doc/RN/en/Callback/OnReceiveNewInvitation.html) 后可以选择接受、拒绝或等待超时。

### 取消邀请

邀请者可以在超时前且被邀请者未处理前取消邀请 [cancel](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/cancel.html)。被邀请者会收到取消通知 [onInvitationCancelled](https://comm.qq.com/im/doc/RN/en/Callback/OnInvitationCancelled.html)，该邀请流程结束。

![](https://main.qcloudimg.com/raw/f482603761219a660d47098c8754ff5a.png)

### 接受邀请

被邀请者收到邀请通知 [onReceiveNewInvitation](https://comm.qq.com/im/doc/RN/en/Callback/OnReceiveNewInvitation.html) 后可以在超时前且邀请者取消前接受邀请 [accept](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/accept.html)，邀请者会收到接受邀请通知 [onInviteeAccepted](https://comm.qq.com/im/doc/RN/en/Callback/OnInviteeAccepted.html)，所有被邀请者处理完后（包括接受、拒绝、超时）该邀请流程结束。

![](https://main.qcloudimg.com/raw/764eae198d01855a4e87f6611b056b28.png)

### 拒绝邀请

被邀请者收到邀请通知 [onReceiveNewInvitation](https://comm.qq.com/im/doc/RN/en/Callback/OnReceiveNewInvitation.html)后可以在超时前且邀请者取消前拒绝邀请 [reject](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/reject.html)，邀请者会收到拒绝邀请通知 [onInviteeRejected](https://comm.qq.com/im/doc/RN/en/Callback/OnInviteeRejected.html)，所有被邀请者处理完后（包括接受、拒绝、超时）该邀请流程结束。

### 邀请超时

若邀请接口的超时时间大于 0，且被邀请者未在超时时间之内响应则邀请超时，邀请者和被邀请者都会收到超时通知 [onInvitationTimeout](https://comm.qq.com/im/doc/RN/en/Callback/OnInvitationTimeout.html)，所有被邀请者处理完后（包括接受、拒绝、超时）该邀请流程结束。若邀请接口的超时时间等于 0，则不会有超时通知。

![](https://main.qcloudimg.com/raw/293e604a66d65413be7b3b1e68b299c5.png)

## 应用场景案例

### 音视频通话

在开源项目 [TRTCFlutterScenesDemo](https://github.com/tencentyun/TRTCFlutterScenesDemo) 中，我们基于 [TRTC 组件](https://intl.cloud.tencent.com/document/product/647/41691) 并对其稍作修改提供了一个适合聊天场景的 1v1 和多人音视频通话的方案，您可以直接基于我们提供的 Demo 进行修改适配。我们以 1v1 视频通话为例介绍下信令接口跟 TRTC SDK 的结合使用。

**1v1 视频通话的流程：**

1. 邀请者根据业务层生成的 roomID 进入该 TRTC 房间，同时调用信令邀请接口 [invite](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/invite.html) 发起音视频通话请求，并把 roomID 放到邀请接口的自定义字段中。
2. 被邀请者收到信令邀请通知 [onReceiveNewInvitation](https://comm.qq.com/im/doc/RN/en/Callback/OnReceiveNewInvitation.html)，并通过自定义数据拿到 roomID，界面开始响铃。
3. 被邀请者处理邀请通知：

- 接受邀请需调用信令 [accept](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/accept.html) 接口，并根据 roomID 进入到 TRTC 房间，并同时调用 openCamera() 函数打开自己本地的摄像头，双方收到 TRTC SDK 的 `onRemoteUserEnterRoom` 回调后记录本次通话的开始时间。
- 拒绝邀请需调用信令 [reject](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/reject.html) 接口结束本次通话。
- 如果被邀请者正在跟其他人通话，则调用信令 [reject](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/reject.html) 接口拒绝本次邀请，并在自定义数据中告诉对方是由于本地线路忙而拒绝。

4. 接听并当双方的音视频通道建立完成后，通话的双方都会接收到 TRTC SDK 的 `onUserVideoAvailable` 的事件通知，表示对方的视频画面已经拿到。此时双方用户均可以调用 TRTC SDK 接口 `startRemoteView` 展示远端的视频画面。远端的声音默认是自动播放的。
5. 通话结束即某一方挂断电话，该用户退出 TRTC 房间。对方收到 TRTC SDK 的 `onRemoteUserLeaveRoom` 回调后计算通话总时长并再次发起一次邀请，此邀请的自定义数据中标明是结束通话并附带通话时长，方便 UI 界面做展示。

**时序图**
![](https://main.qcloudimg.com/raw/868a811fe706f1cbb04014c1058c393d.png)

### 教育场景中老师邀请学生举手发言

该场景为老师先让同学们举手，再从举手的同学中选一个同学进行发言。详细流程如下：

1. 老师调用 [inviteInGroup](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/invite.htmlInGroup) 接口邀请同学们举手，自定义 `data` 中填入“举手操作”，同学们收到 [onReceiveNewInvitation](https://comm.qq.com/im/doc/RN/en/Callback/OnReceiveNewInvitation.html) 回调。
2. 同学们根据 [onReceiveNewInvitation](https://comm.qq.com/im/doc/RN/en/Callback/OnReceiveNewInvitation.html) 中的 `inviteeList` 和 `data` 字段判断被邀请者里有自己且是举手操作，那么调用 [accept](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/accept.html) 接口举手。
3. 如果有学生举手，所有人都可以收到 [onInviteeAccepted](https://comm.qq.com/im/doc/RN/en/Callback/OnInviteeAccepted.html) 回调，判断 `data` 中的字段为“举手操作”，展示举手学生列表。
4. 老师从举手成员列表中邀请某个同学进行发言，调用 [inviteInGroup](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/invite.htmlInGroup) 接口，此时自定义 `data` 中填入“发言操作”，学生们都收到 [onReceiveNewInvitation](https://comm.qq.com/im/doc/RN/en/Callback/OnReceiveNewInvitation.html) 回调。
5. 学生根据 [onReceiveNewInvitation](https://comm.qq.com/im/doc/RN/en/Callback/OnReceiveNewInvitation.html) 回调中的 `inviteeList` 和 `data` 字段判断被邀请者里有自己且是发言操作，则调用 [accept](https://comm.qq.com/im/doc/RN/en/Api/V2TIMSignalingManager/accept.html) 接口发言。
6. 如果有学生发言，所有人都可以收到 [onInviteeAccepted](https://comm.qq.com/im/doc/RN/en/Callback/OnInviteeAccepted.html) 回调，判断 `data` 中的字段为“发言操作”，展示发言成员列表。
