## TUICallKit API 简介

TUICallKit API 是音视频通话组件的**含 UI 接口**，使用TUICallKit API，您可以通过简单接口快速实现一个类微信的音视频通话场景。

<h2 id="TUICallKit">TUICallKit API 概览</h2>


| API | 描述 |
|-----|-----|
| [call](#call) | 发起 1v1 通话|
| [groupCall](#groupCall) | 发起群组通话|
| [setSelfInfo](#setSelfInfo) | 设置用户的头像、昵称|


<h2 id="TUICallEngine"> API 详情</h2>

### call
C2C邀请通话，被邀请方会收到的回调，如果当前处于通话中，可以调用该函数以邀请第三方进入通话

```javascript
 call(params)
```

参数如下表所示：

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| userID | String | 目标用户的userId |
| type | TUICallMediaType | 通话的媒体类型，1-语音通话，2-视频通话|


### groupCall
IM群组邀请通话，被邀请方会收到的回调，如果当前处于通话中，可以继续调用该函数继续邀请他人进入通话，同时正在通话的用户会收到的回调

```javascript
groupCall(params) 
```

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| groupId | String | 此次群组通话的群Id. |
| userIDList | List | 邀请列表 |
| type | TUICallMediaType | 通话的媒体类型，1-语音通话，2-视频通话|


### setSelfInfo
设置用户头像、昵称的接口

```javascript
setSelfInfo(nickName, avatar) ;
```

参数如下表所示：

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| nickName | String | 设置个人昵称 |
| avatar | String | 设置个人昵称 |
