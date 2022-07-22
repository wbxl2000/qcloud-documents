## TUICallKit API 简介

TUICallKit API 是音视频通话组件的**含 UI 接口**，使用TUICallKit API，您可以通过简单接口快速实现一个类微信的音视频通话场景，更详细的接入步骤，详见：[快速接入TUICallKit（）]()

<h2 id="TUICallKit">TUICallKit API 概览</h2>


| API | 描述 |
|-----|-----|
| [sharedInstance](#sharedinstance) | 创建 TUICallKit 实例（单例模式）|
| [setSelfInfo](#setSelfInfo) | 设置用户的头像、昵称|
| [call](#call) | 发起 1v1 通话|
| [groupCall](#groupCall) | 发起群组通话|
| [joinInGroupCall](#joinInGroupCall) | 主动加入当前的群组通话中 |
| [setCallingBell](#switchCallMediaType) | 切换通话媒体类型，比如视频通话切音频通话|
| [enableMuteMode](#setRenderView) | 设置显示视频画面的 View 对象 |
| [enableFloatWindow](#startRemoteView) | 设置显示视频画面的 View 对象 |


##  创建实例和事件回调
### sharedInstance
创建 TUICallKit 的单例。
```java
TUICallKit sharedInstance(Context context)
```
### call
拨打电话（1v1通话）

```java
 void call(String userId, TUICallMediaType mediaType)
```

参数如下表所示：

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| userId | String | 目标用户的userId |
| mediaType | TUICallMediaType | 通话的媒体类型，比如视频通话、语音通话 |

### groupCall
发起群组通话，注意：使用群组通话前需要创建IM 群组，如果已经创建，请忽略；

```java
void groupCall(String groupId, List<String> userIdList, TUICallingType callingType)
```

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| groupId | String | 此次群组通话的群 Id. |
| userIdList | List | 目标用户的userId 列表 |
| mediaType | TUICallMediaType | 通话的媒体类型，比如视频通话、语音通话 |

### joinInGroupCall
发起群组通话，注意：使用群组通话前需要创建IM 群组，如果已经创建，请忽略；

```java
void joinInGroupCall(TUIDefine.RoomId roomId, String groupId, TUICallDefine.MediaType callMediaType, TUIDefine.Callback callback);
```

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| groupId | String | 此次群组通话的群 Id. |
| userIdList | List | 目标用户的userId 列表 |
| mediaType | TUICallMediaType | 通话的媒体类型，比如视频通话、语音通话 |


### setCallingBell
设置自定义来电铃音，这里仅限传入本地文件地址，需要确保该文件目录是应用可以访问的。

```java
void setCallingBell(String filePath);
```

### enableMuteMode
开启/关闭静音模式。

```java
void enableMuteMode(boolean enable);
```


### enableFloatWindow
开启/关闭悬浮窗功能，设置为false后，通话界面左上角的悬浮窗按钮会隐藏。

```java
void enableFloatWindow(boolean enable);
```

