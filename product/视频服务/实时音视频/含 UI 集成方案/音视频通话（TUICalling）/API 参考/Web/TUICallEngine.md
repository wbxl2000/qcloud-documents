## TUICallEngine API 简介

TUICallEngine API 是音视频通话组件的**无 UI 接口**。

<h2 id="TUICallEngine"> API 概览</h2>

| API | 描述 |
|-----|-----|
| [createInstance](#createInstance) | 创建 TUICallEngine 实例（单例模式）|
| [destroyInstance](#destroyInstance) | 销毁 TUICallEngine 实例（单例模式）|
| [on](#on) | 监听事件|
| [off](#off) | 取消监听事件|
| [login](#login) | 登录接口|
| [logout](#logout) | 登出接口|
| [setSelfInfo](#setSelfInfo) | 设置用户昵称和头像|
| [call](#call) | C2C邀请通话|
| [groupCall](#groupCall) | 群聊邀请通话|
| [accept](#accept) | 接听通话 |
| [reject](#reject) | 拒绝通话 |
| [hangup](#hangup) | 结束通话|
| [switchCallingType](#switchCallingType) | 音视频通话切换|
| [startRemoteView](#startRemoteView) | 启动远端画面渲染|
| [stopRemoteView](#stopRemoteView) | 停止远端画面渲染|
| [startLocalView](#startLocalView) | 启动本地画面渲染|
| [stopLocalView](#stopLocalView) | 停止本地画面渲染|
| [openCamera](#opencamera) | 开启摄像头|
| [closeCamara](#closecamara) | 关闭摄像头|
| [openMicrophone](#openMicrophone) | 打开麦克风|
| [closeMicrophone](#closeMicrophone) | 关闭麦克风|
| [setVideoQuality](#setVideoQuality) | 设置视频质量|
| [getDeviceList](#getDeviceList) | 获取设备列表|
| [switchDevice](#switchDevice) | 切换摄像头或麦克风设备|

<h2 id="TUICallEngine"> API 详情</h2>

### createInstance
创建 TUICallEngine 的单例。

```javascript
const tuiCallEngine = TUICallEngine.createInstance({
  SDKAppID: 0, // 接入时需要将0替换为您的云通信应用的 SDKAppID
  tim: tim     // tim 参数适用于业务中已存在 TIM 实例，为保证 TIM 实例唯一性
});
```

### destroyInstance
销毁 TUICallEngine 的单例。

```javascript
let promise = tuiCallEngine.destroyInstance();
promise.then(() => {
  //success
}).catch(error => {
  console.warn('destroyInstance error:', error);
});
```

### on
监听事件。

```javascript
let onError = function(error) {
  console.log(error);
};
tuiCallEngine.on(TUICallEvent.ERROR, onError, this);
```

### off
取消监听事件。
```javascript
let onError = function(error) {
  console.log(error);
};
tuiCallEngine.on(TUICallEvent.ERROR, onError, this);
```

### login
登录接口。
```javascript
let promise = tuiCallEngine.login({userID: 'your userID', userSig: 'your userSig'});
promise.then(() => {
  //success
}).catch(error => {
  console.warn('login error:', error);
});
```

### logout
登出接口。
```javascript
let promise = tuiCallEngine.logout();
promise.then(() => {
  //success
}).catch(error => {
  console.warn('logout error:', error);
});
```

### setSelfInfo
设置用户昵称和头像。
```javascript
let promise = tuiCallEngine.setSelfInfo("username", "avaterlink");
promise.then(() => {
  //success
}).catch(error => {
  console.warn('setSelfInfo error:', error);
});
```

### call
C2C 邀请通话。

```javascript
const offlinePushInfo = {
  title: '',
  description: '您有一个通话请求',
  extension: ''
}
let promise = tuiCallEngine.call({
  userID: 'user1', 
  type: 1, 
  offlinePushInfo: offlinePushInfo
});
promise.then(() => {
  //success
}).catch(error => {
  console.warn('call error:', error);
});
```

### groupCall
IM 群组邀请通话，被邀请方会收到 `EVENT.INVITED` 事件。

```javascript
const offlinePushInfo = {
  title: '',
  description: '您有一个通话请求',
  extension: ''
}
let promise = tuiCallEngine.groupCall({
  userIDList: ['user1', 'user2'], 
  type: 1, 
  groupID: 'IM群组 ID', 
  offlinePushInfo: offlinePushInfo
});
promise.then(() => {
  //success
}).catch(error => {
  console.warn('groupCall error:', error);
});
```

### accept

当您作为被邀请方收到 `TUICallEvent.INVITED` 事件的回调时，可以调用该接口接听来电。

```javascript
tuiCallEngine.on(TUICallEvent.INVITED, ({inviteID, sponsor, inviteData}) => {
  let promise = tuiCallEngine.accept();
   promise.then(() => {
    //success
  }).catch(error => {
    console.warn('accept error:', error);
  });
});
```

### reject

拒绝当前通话，当您作为被叫收到 `TUICallEvent.INVITED ` 的回调时，可以调用该函数拒绝来电。
```javascript
tuiCallEngine.on(TUICallEvent.INVITED, ({inviteID, sponsor, inviteData}) => {
  tuiCallEngine.reject();
});
```

### hangup
挂断当前通话，当您处于通话中，可以调用该函数结束通话。

```javascript
tuiCallEngine.hangup();
```

### switchCallingType
音视频通话切换。

```javascript
let promise = tuiCallEngine.switchCallingType("audio");
promise.then(() => {
  //success
}).catch(error => {
  console.warn('switchCallingType error:', error);
});
```

### startRemoteView
启动远端画面渲染。

```javascript
let promise = tuiCallEngine.startRemoteView({
  userID: 'user1', 
  videoViewDomID: 'video_1',
});
promise.then(() => {
  //success
}).catch(error => {
  console.warn('startRemoteView error:', error);
});
```
### stopRemoteView
停止远端画面渲染

```javascript
tuiCallEngine.stopRemoteView({userID: 'user1'});
```

### startLocalView
启动本地画面渲染

```javascript
let promise = tuiCallEngine.startLocalView({
  userID: 'user1', 
  videoViewDomID: 'video_1'
});
promise.then(() => {
  //success
}).catch(error => {
  console.warn('startLocalView error:', error);
});
```

### stopLocalView
停止本地画面渲染

```javascript
tuiCallEngine.stopLocalView({userID: 'user1'});
```

### openCamera
开启摄像头。
```javascript
let promise = tuiCallEngine.openCamera();
promise.then(() => {
  //success
}).catch(error => {
  console.warn('openCamera error:', error);
});
```
### closeCamara
关闭摄像头
```javascript
let promise = tuiCallEngine.closeCamera();
promise.then(() => {
  //success
}).catch(error => {
  console.warn('closeCamara error:', error);
});
```

### openMicrophone
打开麦克风。
```javascript
let promise = tuiCallEngine.openMicrophone();
promise.then(() => {
  //success
}).catch(error => {
  console.warn('openMicrophone error:', error);
});
```

### closeMicrophone
关闭麦克风。
```javascript
let promise = tuiCallEngine.closeMicrophone();
promise.then(() => {
  //success
}).catch(error => {
  console.warn('closeMicrophone error:', error);
});
```

### setVideoQuality
设置视频质量。
```javascript
const profile = '720p';
tuiCallEngine.setVideoQuality(profile);   
```

### getDeviceList
获取设备列表。
```javascript
let promise = tuiCallEngine.getDeviceList("audio");
promise.then(() => {
  //success
}).catch(error => {
  console.warn('getDeviceList error:', error);
});
```

### switchDevice
切换摄像头或麦克风设备。
```javascript
tuiCallEngine.switchDevice({
  deviceType: 'video', 
  deviceId: cameras[0].deviceId
});
```