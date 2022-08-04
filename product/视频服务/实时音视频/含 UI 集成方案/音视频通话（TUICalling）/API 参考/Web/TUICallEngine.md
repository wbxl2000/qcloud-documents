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
trtcCalling.createInstance(context);
```

### destroyInstance
销毁 TUICallEngine 的单例。
```javascript
let promise = trtcCalling.destroyInstance();
promise.then(() => {
  //success
}).catch(error => {
  console.warn('destroyInstance error:', error)
});
```

### on
监听事件。
```javascript
let onError = function(error) {
  console.log(error);
};
trtcCalling.on(TRTCCalling.EVENT.ERROR, onError, this);
```

### off
取消监听事件。
```javascript
let onError = function(error) {
  console.log(error);
};
trtcCalling.on(TRTCCalling.EVENT.ERROR, onError, this);
```

### login
登录接口。
```javascript
let promise = trtcCalling.login({userID: 'your userID', userSig: 'your userSig'});
promise.then(() => {
  //success
}).catch(error => {
  console.warn('login error:', error)
});
```

### logout
登出接口。
```javascript
let promise = trtcCalling.logout();
promise.then(() => {
  //success
}).catch(error => {
  console.warn('logout error:', error)
});
```

### setSelfInfo
设置用户昵称和头像。
```javascript
let promise = trtcCalling.setSelfInfo("username", "avaterlink");
promise.then(() => {
  //success
}).catch(error => {
  console.warn('setSelfInfo error:', error)
});
```

### call
C2C邀请通话。

```javascript
const offlinePushInfo = {
  title: '',
  description: '您有一个通话请求',
  extension: ''
}
let promise = trtcCalling.call({userID: 'user1', type: 1, offlinePushInfo: offlinePushInfo});
promise.then(() => {
  //success
}).catch(error => {
  console.warn('call error:', error)
});
```

### groupCall
IM群组邀请通话，被邀请方会收到 `EVENT.INVITED` 事件。

```javascript
const offlinePushInfo = {
  title: '',
  description: '您有一个通话请求',
  extension: ''
}
let promise = trtcCalling.groupCall({userIDList: ['user1', 'user2'], type: 1, groupID: 'IM群组 ID', offlinePushInfo: offlinePushInfo});
promise.then(() => {
  //success
}).catch(error => {
  console.warn('groupCall error:', error)
});
```

### accept

当您作为被邀请方收到 `EVENT.INVITED` 事件的回调时，可以调用该接口接听来电。

```javascript
trtcCalling.on(TrtcCalling.EVENT.INVITED, ({inviteID, sponsor, inviteData}) => {
  let promise = trtcCalling.accept();
   promise.then(() => {
    //success
  }).catch(error => {
    console.warn('accept error:', error);
  });
});
```

### reject

拒绝当前通话，当您作为被叫收到 `EVENT.INVITED ` 的回调时，可以调用该函数拒绝来电。
```javascript
trtcCalling.on(TrtcCalling.EVENT.INVITED, ({inviteID, sponsor, inviteData}) => {
  trtcCalling.reject();
});
```

### hangup
挂断当前通话，当您处于通话中，可以调用该函数结束通话。

```javascript
trtcCalling.hangup();
```

### switchCallingType
音视频通话切换。

```javascript
let promise = trtcCalling.switchCallingType("audio");
promise.then(() => {
  //success
}).catch(error => {
  console.warn('switchCallingType error:', error)
});
```

### startRemoteView
启动远端画面渲染。

```javascript
let promise = trtcCalling.startRemoteView({userID: 'user1', videoViewDomID: 'video_1'});
promise.then(() => {
  //success
}).catch(error => {
  console.warn('startRemoteView error:', error)
});
```
### stopRemoteView
停止远端画面渲染

```javascript
// v1.0.0之前
trtcCalling.stopRemoteView({userID: 'user1', videoViewDomID: 'video_1'});
// v1.0.0及其之后
trtcCalling.stopRemoteView({userID: 'user1'});
```

### startLocalView
启动本地画面渲染

```javascript
let promise = trtcCalling.startLocalView({userID: 'user1', videoViewDomID: 'video_1'});
promise.then(() => {
  //success
}).catch(error => {
  console.warn('startLocalView error:', error)
});
```

### stopLocalView
停止本地画面渲染

```javascript
// v1.0.0之前
trtcCalling.stopLocalView({userID: 'user1', videoViewDomID: 'video_1'});
// v1.0.0及其之后
trtcCalling.stopLocalView({userID: 'user1'});
```

### openCamera
开启摄像头。
```javascript
trtcCalling.openCamera();
```
### closeCamara
关闭摄像头
```javascript
trtcCalling.closeCamera();
```

### openMicrophone
打开麦克风。
```javascript
trtcCalling.openMicrophone();
```

### closeMicrophone
关闭麦克风。
```javascript
trtcCalling.closeMicrophone();
```

### setVideoQuality
设置视频质量。
```javascript
const profile = '720p';
trtcCalling.setVideoQuality(profile);
```

### getDeviceList
获取设备列表。
```javascript
let promise = trtcCalling.getDeviceList("audio");
promise.then(() => {
  //success
}).catch(error => {
  console.warn('getDeviceList error:', error)
});
```

### switchDevice
切换摄像头或麦克风设备。
```javascript
trtcCalling.switchDevice({deviceType: 'video', deviceId: cameras[0].deviceId});
```