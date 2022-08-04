## TUICallingEvent API 简介

TUICallingEvent API 是音视频通话组件的**事件接口**。

<h2 id="TUICallingEvent"> 事件列表</h2>

| EVENT | 描述 |
|-----|-----|
| [TUICallingEvent.ERROR]() | SDK 内部发生了错误|
| [TUICallingEvent.SDK_READY]() | SDK 进入 ready 状态时收到该回调|
| [TUICallingEvent.KICKED_OUT]() | 重复登陆，收到该回调说明被踢出房间|
| [TUICallingEvent.USER_ACCEPT]() | 如果有用户接听，那么会收到此回调 |
| [TUICallingEvent.USER_ENTER]() | 如果有用户同意进入通话，那么会收到此回调|
| [TUICallingEvent.USER_LEAVE]() | 如果有用户同意离开通话，那么会收到此回调|
| [TUICallingEvent.REJECT]() | 用户拒绝通话 |
| [TUICallingEvent.NO_RESP]() | 邀请用户无应答|
| [TUICallingEvent.LINE_BUSY]() | 邀请方忙线|
| [TUICallingEvent.CALLING_TIMEOUT]() | 作为被邀请方会收到，收到该回调说明本次通话超时未应答|
| [TUICallingEvent.USER_VIDEO_AVAILABLE]() | 远端用户开启/关闭了摄像头, 会收到该回调|
| [TUICallingEvent.USER_AUDIO_AVAILABLE]() | 远端用户开启/关闭了麦克风, 会收到该回调|
| [TUICallingEvent.USER_VOICE_VOLUME]() | 远端用户说话音量调整, 会收到该回调|
| [TUICallingEvent.GROUP_CALL_INVITEE_LIST_UPDATE]() | 群聊更新邀请列表收到该回调|
| [TUICallingEvent.INVITED]() | 被邀请进行通话|
| [TUICallingEvent.CALLING_CANCEL]() | 作为被邀请方会收到，收到该回调说明本次通话被取消了|
| [TUICallingEvent.CALLING_END]() | 收到该回调说明本次通话结束了|
| [TUICallingEvent.DEVICED_UPDATED]() | 设备列表更新收到该回调 |
| [TUICallingEvent.CALL_TYPE_CHANGED]() | 通话类型切换收到该回调 |

<h2 id="TUICallingEvent"> 事件详情</h2>

### ERROR
SDK 内部发生了错误。
```javascript
let onError = function(error) {
  console.log(error)
};
trtcCalling.on(TRTCCalling.EVENT.ERROR, onError);
```

### SDK_READY
SDK 进入 ready 状态时收到该回调。
> v1.0.0 及其之后版本，新增此事件。
```javascript
let onSDKReady = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.SDK_READY, onSDKReady);
```

### KICKED_OUT
重复登陆，收到该回调说明被踢出房间。
```javascript
let handleOnKickedOut = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.KICKED_OUT, handleOnKickedOut);
```

### USER_ACCEPT
如果有用户接听，那么会收到此回调。
```javascript
let handleUserAccept = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.USER_ACCEPT, handleUserAccept);
```

### USER_ENTER
如果有用户同意进入通话，那么会收到此回调。
```javascript
let handleUserEnter = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.USER_ENTER, handleUserEnter);
```

### USER_LEAVE
如果有用户同意离开通话，那么会收到此回调。
```javascript
let handleUserLeave = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.USER_LEAVE, handleUserLeave);
```

### REJECT
用户拒绝通话。
```javascript
let handleInviteeReject = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.REJECT, handleInviteeReject);
```

### NO_RESP
邀请用户无应答。
- 在C2C通话中，只有发起方会收到无人应答的回调 例如 A 邀请 B、C 进入通话，B不应答，A可以收到该回调，但C不行
- 在IM群组通话中，所有被邀请人均能收到该回调 例如 A 邀请 B、C 进入通话，B不应答，A、C均能收到该回调
```javascript
let handleNoResponse = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.NO_RESP, handleNoResponse);
```

### LINE_BUSY
邀请方忙线。
```javascript
let handleLineBusy = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.LINE_BUSY, handleLineBusy);
```

### CALLING_TIMEOUT
作为被邀请方会收到，收到该回调说明本次通话超时未应答。
```javascript
let handleCallingTimeout = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.CALLING_TIMEOUT, handleCallingTimeout);
```

### USER_VIDEO_AVAILABLE
远端用户开启/关闭了摄像头, 会收到该回调。
```javascript
let handleUserVideoChange = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.USER_VIDEO_AVAILABLE, handleUserVideoChange);
```


### USER_AUDIO_AVAILABLE
远端用户开启/关闭了麦克风, 会收到该回调。
```javascript
let handleUserAudioChange = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.USER_AUDIO_AVAILABLE, handleUserAudioChange);

```

### USER_VOICE_VOLUME
远端用户说话音量调整, 会收到该回调
```javascript
let handleUserVoiceVolumeChange = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.USER_VOICE_VOLUME, handleUserVoiceVolumeChange);
```

### GROUP_CALL_INVITEE_LIST_UPDATE
群聊更新邀请列表收到该回调。
> v1.0.0 及其之后版本，新增此事件。
```javascript
let handleGroupInviteeListUpdate = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.GROUP_CALL_INVITEE_LIST_UPDATE, handleGroupInviteeListUpdate);
```

### INVITED
被邀请进行通话。
```javascript
let handleNewInvitationReceived = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.INVITED, handleNewInvitationReceived);
```

### CALLING_CANCEL
作为被邀请方会收到，收到该回调说明本次通话被取消了。
```javascript
let handleCallingCancel = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.CALLING_CANCEL, handleCallingCancel);
```


### CALLING_END
收到该回调说明本次通话结束了。
```javascript
let handleCallingEnd = function(event) {
  console.log(event)
};
trtcCalling.on(TRTCCalling.EVENT.CALLING_END, handleCallingEnd);
```

### DEVICED_UPDATED
设备列表更新收到该回调。
> v1.0.8 及其之后版本，新增此事件。
```javascript
let handleDeviceUpdated = function({ microphoneList, cameraList, currentMicrophoneID, currentCameraID }) {
  console.log(microphoneList, cameraList, currentMicrophoneID, currentCameraID)
};
trtcCalling.on(TRTCCalling.EVENT.DEVICED_UPDATED, handleDeviceUpdated);
```

### CALL_TYPE_CHANGED
通话类型切换收到该回调。
> v1.0.8 及其之后版本，新增此事件。
```javascript
let handleCallTypeChanged = function({ oldCallType, newCallType }) {
  console.log(oldCallType, newCallType)
};
trtcCalling.on(TRTCCalling.EVENT.CALL_TYPE_CHANGED, handleDeviceUpdated);
```