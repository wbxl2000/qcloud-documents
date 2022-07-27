## TUICallKit 说明 
TUICalling Android 平台音视频通话组件支持如下两种接入方式：

- TUICallKit：含 UI 交互版本，交互体验”类微信“，支持悬浮窗、自定义铃音、贴耳息屏、离线推送等多个特性，适用于大部分通话场景；
- TUICallEngine：无 UI 交互版本，仅包含通话业务相关的逻辑 API，适用于 UI 定制性较高的场景；

本文以`TUICallKit`的接入为主，如果您需要使用`TUICallEngine`进行接入，可以参考 [TUICallEngine API]()。


## 环境准备
在Android 平台，TUICallKit 的集成需要满足如下要求：

- Android 4.1 (API level 16) 及更高；
- Gradle 3.4.0 及更高；


## 接入TUICallKit
通过集成TUICallKit，您可以通过对方 UserId 直接拨打一个 1v1 通话，也可以在创建一个群组后，通过TUICallKit 来发起一次群组通话，开始接入！


### 步骤一：下载并导入 TUICallKit 组件
点击进入 [TUICalling Github](https://github.com/tencentyun/TUICalling) ，选择克隆/下载代码，然后拷贝 Android 目录下的 tuicallkit 目录到您的工程的 app 同一级目录中，类似下图：
![](https://qcloudimg.tencent-cloud.cn/raw/5184b651c273ff1727065866cc45cd9a.png)

### 步骤二：配置 Android Gradle 文件

1. 在 `setting.gradle` 中完成导入，参考如下：
```java
include ':tuicallkit'
```
2. 在 app 的 build.gradle 文件中添加对 tuicallkit 的依赖：
```java
implementation project(':tuicallkit')
```
> ? tuicallkit 工程内部已经默认依赖：TRTC SDK、IM SDK、TUICallEngine SDK，不需要开发者单独配置。

### 步骤三：配置权限及混淆规则

TUICalling 组件在使用过程中，需要用到诸如相机、麦克风等权限，所以需要您在 app Module 的 AndroidManifest.xml 中配置如下权限：
```java
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />        // 使用场景：悬浮窗、应用在后台时拉起通话界面时需要此权限；
<uses-permission android:name="android.permission.INTERNET" />              
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.BLUETOOTH" />                  // 使用场景：使用蓝牙耳机时需要此权限；
<uses-permission android:name="android.permission.READ_PHONE_STATE" />           // 使用场景：判断是否是系统来电打断时需要此权限；
<uses-permission android:name="android.permission.CAMERA" />
<uses-feature android:name="android.hardware.camera"/>
<uses-feature android:name="android.hardware.camera.autofocus" />
```
备注：Android 6.0以上的 Android 系统需要动态申请相机、麦克风、读取存储权限等

### 步骤四：设置编译混淆规则
因为在我们的 SDK内部使用到了JVM 反射的特性，需要将 SDK 相关类加入不混淆名单，所以需要您在 proguard-rules.pro 文件添加如下内容：
``` bash
-keep class com.tencent.** { *; }
```

### 步骤五：创建并初始化 TUICallKit
在完成上述步骤后，您就可以在您的应用中创建 TUICallKit 实例，并完成鉴权逻辑，这个步骤很关键，请您耐心检查相关参数是否配置正确；
```java
TUICallKit callKit = TUICallKit.sharedInstance(Context).init(int sdkappid, String userId， String userSig);

```
**参数说明**：
- **SDKAppID**：**TRTC 应用ID**，如果您未开通腾讯云 TRTC 服务，可进入 [腾讯云实时音视频控制台](https://console.cloud.tencent.com/trtc/app)，创建一个新的 TRTC 应用后，单击**应用信息**，SDKAppID 信息如下图所示：
![](https://qcloudimg.tencent-cloud.cn/raw/3d6ebfa2a1e4ae5d3af3ecd564fb1463.png)
- **Secretkey**：**TRTC 应用密钥**，和 SDåKAppId 对应，进入 [TRTC 应用管理](https://console.cloud.tencent.com/trtc/app) 后，SecretKey 信息如上图所示。
- **userId**：当前用户的 ID，字符串类型，长度不超过32字节，不支持使用特殊字符，建议使用英文或数字，可结合业务实际账号体系自行设置。
- **userSig**：根据 SDKAppId、userId，Secretkey 等信息计算得到的安全保护签名，您可以单击 [这里](https://console.cloud.tencent.com/trtc/usersigtool) 直接在线生成一个调试的 UserSig，也可以参照我们的 [TUICalling示例工程](https://github.com/tencentyun/TUICalling/blob/main/Android/app/src/main/java/com/tencent/liteav/demo/LoginActivity.java#L74)自行计算，更多信息请参见 [如何计算及使用 UserSig](https://cloud.tencent.com/document/product/647/17275)。

> ! 这个步骤也是目前腾讯云音视频团队收到的反馈最多的步骤，常见的有如下几类，欢迎自查：
> 1. sdkappid 设置错误，国内站sdkappid一般是140开头的10位整数；
> 2. userSig 配置成 Secretkey，userSig是根据sdkappid、userid、secretkey，以及过期时间等参数动态生成的，详见上面的解释；
> 3. userid 都设置成“1”、“123”、“111”等简单字符串，在多人协作时，被后面设置的同学“覆盖”，导致初始失败，建议在调试的时候设置一些辨识度高的userId；



###  步骤六：拨打通话

- **实现1对1视频通话**：
```java
// 发起1对1视频通话，假设userId为：100001；
TUICallKit.sharedInstance(Context).call("100001", TUICallDefine.MediaType.Video);  
```
>? TUICallKit 目前还不支持发起非群组的多人视频通话，如果您有此类需求，欢迎反馈： [TUICalling 需求收集表]()。

- **实现群组视频通话**：
```java
TUICallKit.sharedInstance(Context).groupCall("12345678", ["100001", "100002", "100003"], TUICallDefine.MediaType.Video); 
```
| 参数 | 类型 | 含义 |
|-----|-----|-----|
| groupId | String | 群组 Id，示例：`"12345678"` |
| userIds | Array | 目标用户的userId 列表，示例：`["100001", "100002", "100003"]` |
| mediaType | TUICallMediaType | 通话的媒体类型，示例：`TUICallDefine.MediaType.Video` |

>? 群组的创建详见：[ IM 群组管理](https://cloud.tencent.com/document/product/269/75394#.E5.88.9B.E5.BB.BA.E7.BE.A4.E7.BB.84) ，或者您也可以直接使用 [IM TUIKit](https://cloud.tencent.com/document/product/269/37059)，一站式集成聊天、通话等场景。

###  步骤七：接听通话
在步骤五完成后，收到来电请求后，TUICallKit 组件会自动启动相应的接听界面，不过因为Android 系统权限的原因，分为如下几种情况：
- 您的App 在前台时，当收到邀请时会自动弹出呼叫界面并播放来电铃音；
- 您的App 在后台，但是有授予悬浮窗权限后者后台弹出界面等权限时，仍然会自动弹出呼叫界面并播放来电铃音；
- 您的App 在后台，且没有授予悬浮窗权限后者后台弹出界面等权限时，TUICallKit 会播放来电铃音，提示用户接听或挂断；
- 您的App 进程已经被销毁，只能通过接入[**Andorid 离线推送**]()，通过通知栏消息提示用户接听或挂断；

## 更多特性

### 一. 设置昵称&头像
如果您需要自定义头像或者昵称可以使用如下接口进行更新：
```java
TUICallKit.sharedInstance(context).setSelfInfo("昵称", "头像 URL", callback)；
```
> ! 这里有个常见问题：因为用户隐私限制，非好友之间的通话，被叫的头像更新可能会有延迟，一次通话成功后就会顺利更新，知悉~

### 二. 离线推送

完成以上步骤，就可以实现音视频通话的拨打和接通，但如果您的业务场景需要在 `App 的进程被杀死后`或者`APP 退到后台后`，还可以正常接收到音视频通话请求，就需要为 TUICalling 组件增加离线推送功能，详情见 [**Android 离线推送接入指引**](https://github.com/tencentyun/TUICalling/blob/main/Android/Android%E7%A6%BB%E7%BA%BF%E6%8E%A8%E9%80%81%E6%8E%A5%E5%85%A5%E6%8C%87%E5%BC%95.md)。

### 三. 悬浮窗功能
如果您的业务需要开启悬浮窗功能，您可以在 TUICallKit 组件初始化时调用 `TUICallKit.sharedInstance(context).enableFloatWindow(true)` 开启该功能。

### 四. 通话状态监听
如果您的业务需要 [监听通话的状态](https://cloud.tencent.com/document/product/647/47712#setcallinglistener)，例如通话开始、结束，以及通话过程中的网络质量等等，就可以监听以下事件：
```java
TUICallEngine.sharedInstance(mContext).addObserver(new TUICallObserver() {
    @Override
    public void onCallBegin(TUIDefine.RoomId roomId, TUICallDefine.MediaType callMediaType,
                            TUICallDefine.Role callRole) {
    }
    public void onCallEnd(TUIDefine.RoomId roomId, TUICallDefine.MediaType callMediaType,
                            TUICallDefine.Role callRole, long totalTime) {
    }
    public void onUserNetworkQualityChanged(List<TUICallDefine.NetworkQualityInfo> networkQualityList) {
    }
});
```

### 五. 自定义铃音
如果您需要自定义来电铃音，可以通过如下接口进行设置：
```java
TUICallKit.sharedInstance(context).setCallingBell(filePath);
```



