## TUICallKit 说明 
TUICalling iOS 平台音视频通话组件支持如下两种接入方式：

- TUICallKit：含 UI 交互版本，交互体验”类微信“，支持悬浮窗、自定义铃音、贴耳息屏、离线推送等多个特性，适用于大部分通话场景；
- TUICallEngine：无 UI 交互版本，仅包含通话业务相关的逻辑 API，适用于 UI 定制性较高的场景；

本文以`TUICallKit`的接入为主，如果您需要使用`TUICallEngine`进行接入，可以参考 [TUICallEngine API]()。


## 环境准备
在iOS 平台，TUICallKit 的集成需要满足如下要求：

- iOS 9.0 (API level 16) 及更高；


## 接入TUICallKit
通过集成TUICallKit，您可以通过对方 UserId 直接拨打一个 1v1 通话，也可以在创建一个群组后，通过TUICallKit 来发起一次群组通话，开始接入！


### 步骤一：下载并导入 TUICallKit 组件
**通过 cocoapods 导入组件**，具体步骤如下：


### 步骤二：配置权限

使用音视频功能，需要授权麦克风和摄像头的使用权限。在 App 的 Info.plist 中添加以下两项，分别对应麦克风和摄像头在系统弹出授权对话框时的提示信息。

```
<key>NSCameraUsageDescription</key>
<string>CallingApp需要访问您的相机权限，开启后录制的视频才会有画面</string>
<key>NSMicrophoneUsageDescription</key>
<string>CallingApp需要访问您的麦克风权限，开启后录制的视频才会有声音</string>
```
![](https://qcloudimg.tencent-cloud.cn/raw/7f91f3f8defa5a0650f08b8acf960219.png)


### 步骤三：创建并初始化组件

<dx-codeblock>
:::  Objective-C
```
// 1.组件登录
[TUILogin login:userID userID:groupID userSig:groupID succ:^{
        NSLog(@"login success");
 } fail:^(int code, NSString *msg) {
        NSLog(@"login failed, code: %d, error: %@", code, msg);
 }];
 
 // 2.初始化TUICalling实例
[TUICallKit shareInstance];
```
:::
::: Swift
```
// 2.初始化TUICalling实例
TUILogin.login(Int32(SDKAPPID), userID: user, userSig: userSig) {
         print("login success")
 } fail: { (code, err) in
         print("login failed, code: \(code), error: \(message ?? "nil")")
 }
 
// 2.初始化TUICallKit实例
TUICallKit.shareInstance()
```
:::
</dx-codeblock>

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



###  步骤四：拨打通话

- **实现1对1视频通话**：
- <dx-codeblock>
:::  Objective-C
```
// 发起1对1视频通话，假设userId为：100001；
[[TUICallKit shareInstance] call: @"100001" callMediaType: TUICallMediaTypeVideo];
```
```
:::
::: Swift
```
// 2.初始化TUICalling实例
TUICallKit.shareInstance().call(userId: "100001", callType: .video)
```
:::
</dx-codeblock>
>? TUICallKit 目前还不支持发起非群组的多人视频通话，如果您有此类需求，欢迎反馈： [TUICalling 需求收集表]()。

- **实现群组视频通话**：
- - <dx-codeblock>
:::  Objective-C
```
[[TUICallKit shareInstance] groupCall: @[@"100001", @"100002", @"100003"] groupId: @"12345678"  callMediaType: TUICallMediaTypeVideo];
```
```
:::
::: Swift
```
TUICallKit.shareInstance().groupCall(userIds: ["100001", "100002", "100003"], groupId:"12345678", callMediaType: .video)
```
:::
</dx-codeblock>

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| groupId | String | 群组 Id，示例：`@"12345678"` |
| userIds | Array | 目标用户的userId 列表，示例：`@[@"100001", @"100002", @"100003"]` |
| type | TUICallMediaType | 通话的媒体类型，示例：`TUICallMediaTypeVideo` |

>? 群组的创建详见：[ IM 群组管理](https://cloud.tencent.com/document/product/269/75394#.E5.88.9B.E5.BB.BA.E7.BE.A4.E7.BB.84) ，或者您也可以直接使用 [IM TUIKit](https://cloud.tencent.com/document/product/269/37059)，一站式集成聊天、通话等场景。

###  步骤五：接听通话
在步骤三完成后，收到来电请求后，TUICallKit 组件会自动启动相应的接听界面。

## 更多特性

### 一. 设置昵称&头像
如果您需要自定义头像或者昵称可以使用如下接口进行更新：
- <dx-codeblock>
:::  Objective-C
```
 [[TUICallKit shareInstance]setSelfInfo:@"昵称" avatar:@"头像url" succ:^{
            NSLog(@"login success");
  } fail:^(int code, NSString *errMsg) {
            NSLog(@"login failed, code: %d, error: %@", code, errMsg);
  }];
```
```
:::
::: Swift
```
 TUICallKit.shareInstance().setSelfInfo(nickname: "昵称", avatar: "头像url") {
            print("login success")
  } fail: {(code, desc) in
            print("login failed, code: \(code), error: \(desc ?? "nil")")
  }
```
:::
</dx-codeblock>
> ! 这里有个常见问题：因为用户隐私限制，非好友之间的通话，被叫的头像更新可能会有延迟，一次通话成功后就会顺利更新，知悉~

### 二. 离线推送

完成以上步骤，就可以实现视频通话的拨打和接通，但如果您的业务场景需要在 `App 的进程被杀死后`或者 `App 退到后台后`，还可以正常接收到音视频通话请求，就需要为 TUICalling 组件增加离线推送功能，详情见 [**离线推送（iOS）**](https://cloud.tencent.com/document/product/269/44517)。

### 三. 悬浮窗功能
如果您的业务需要开启 [悬浮窗功能](https://cloud.tencent.com/document/product/647/47748#enableFloatWindow)，您可以在 TUICallKit 组件初始化时调用` TUICallKit.shareInstance().enableFloatWindow(enable: true))`开启该功能。

>? 目前组件仅支持应用内悬浮窗（最小化退到上一层界面）。。

### 四. 通话状态监听
如果您的业务需要 [监听通话的状态](https://cloud.tencent.com/document/product/647/47712#setcallinglistener)，例如通话开始、结束，以及通话过程中的网络质量等等，就可以监听以下事件：
<dx-codeblock>
:::  Objective-C Objectivec
[[TUICallEngine shareInstance] addObserver:self];

- (void)onCallBegin:(nonnull TUIRoomId *)roomId callMediaType:(TUICallMediaType)callMediaType callRole:(TUICallRole)callRole {
    
}

- (void)onCallEnd:(nonnull TUIRoomId *)roomId callMediaType:(TUICallMediaType)callMediaType callRole:(TUICallRole)callRole totalTime:(float)totalTime {
   
}

- (void)onUserNetworkQualityChanged:(nonnull NSArray<TUINetworkQualityInfo *> *)networkQualityList {
    
}
:::
::: Swift Swift
TUICallEngine.shareInstance().add(self)

public func onCallBegin(roomId: TUIRoomId, callMediaType: TUICallMediaType, callRole: TUICallRole) {
        
 }
public func onCallEnd(roomId: TUIRoomId, callMediaType: TUICallMediaType, callRole: TUICallRole, totalTime: Float) {
        
 }
public func onUserNetworkQualityChanged(networkQualityList: [TUINetworkQualityInfo]) {
        
 }
:::
</dx-codeblock>

### 五. 自定义铃音
调用 [TUICalling#setCallingBell](https://cloud.tencent.com/document/product/647/47748#setCallingBell) 即可。


