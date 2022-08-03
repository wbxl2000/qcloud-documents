## TUICallKit 说明 
TUICalling 小程序平台音视频通话组件支持如下两种接入方式：

- TUICallKit：含 UI 交互版本，交互体验”类微信“，适用于大部分通话场景；
- TUICallEngine：无 UI 交互版本，仅包含通话业务相关的逻辑 API，适用于 UI 定制性较高的场景；

本文以`TUICallKit`的接入为主，如果您需要使用`TUICallEngine`进行接入，可以参考 [TUICallEngine API]()。

## 接入TUICallKit
通过集成TUICallKit，您可以通过对方 UserId 直接拨打一个 1v1 通话。

### 步骤一：开通小程序权限
由于 TUICallKit 所使用的小程序标签有更苛刻的权限要求，因此集成 TUICallKit 的第一步就是要开通小程序的类目和标签使用权限，否则无法使用，这包括如下步骤：
- 小程序推拉流标签不支持个人小程序，只支持企业类小程序。需要在 注册 时填写主体类型为企业
![](https://qcloudimg.tencent-cloud.cn/raw/a30f04a8983066fb9fdf179229d3ee31.png)

- 小程序推拉流标签使用权限暂时只开放给有限 类目。
- 符合类目要求的小程序，需要在 微信公众平台 > 开发 > 开发管理 > 接口设置中自助开通该组件权限，如下图所示：
![](https://main.qcloudimg.com/raw/dc6d3c9102bd81443cb27b9810c8e981.png)

### 步骤二：在小程序控制台配置域名
在 微信公众平台 > 开发 > 开发管理 > 开发设置 > 服务器域名中设置 request 合法域名 和 socket 合法域名，如下图所示：

- request 合法域名
```javascript
https://official.opensso.tencent-cloud.com
https://yun.tim.qq.com
https://cloud.tencent.com
https://webim.tim.qq.com
https://query.tencent-cloud.com
https://web.sdk.qcloud.com
```
- socket 合法域名：
```javascript
wss://wss.im.qcloud.com
wss://wss.tim.qq.com
```
![](https://main.qcloudimg.com/raw/dc6d3c9102bd81443cb27b9810c8e981.png)

### 步骤三：获取 SdkAppId 和签名密钥
如果您还没有腾讯云账号，请注册一个腾讯云账号，并完成 实名认证。然后跳转到 TRTC 管理控制台中的 应用管理 界面。
如果您的应用列表为空，可以单击创建应用创建一个新的应用，之后单击应用信息，打开该应用管理界面。在该界面中寻找快速上手页签，就能够看到如下界面：
![](https://qcloudimg.tencent-cloud.cn/raw/22d68577a7a54cc7cc7df2f078ffbff3.png)

- SDKAppID：TRTC 的应用 ID，用于业务隔离，即不同的 SDKAppID 的通话彼此不能互通。
- Secretkey：TRTC 的应用密钥，需要和 SDKAppID 配对使用，用于签出合法使用 TRTC 服务的鉴权用票据 UserSig，我们会在接下来的步骤五中用到这个 Key。

### 步骤四：下载并导入 TUICallKit 组件
单击进入 Github ，选择克隆/下载代码，然后拷贝 MiniProgram 下的 TUICallKit 和 TUICallEngine 目录到您的工程中。

### 步骤五：创建并初始化 TUI 组件库
1.添加组件到对应页面的 页面配置，例如 pages/index/index.json：
```javascript
// 可参考 MiniProgram/pages/videoCall/videoCall.json 或 MiniProgram/pages/audioCall/audioCall.json
{
    "usingComponents": {
        "TUICallKit": "/TUICallKit/TUICallKit"
    }
}
```

2.添加 JS 逻辑交互，初始化并使用组件，例如 pages/index/index.js：
```javascript
// 可参考 MiniProgram/pages/videoCall/videoCall.js 或 MiniProgram/pages/audioCall/audioCall.js
Page({
    /**
     * 页面的初始数据
     */
 TUICallKit: null,
  data: {
    userIDToSearch: '',
    searchResultShow: false,
    invitee: null,
    config: {
      sdkAppID: 0,
      userID: 0,
      userSig: '',
      type: 1,
      tim: null,// 参数适用于业务中已存在 TIM 实例，为保证 TIM 实例唯一性
    },
  },

    /**
     * 生命周期函数--监听页面加载
     */
    onLoad() {
    const Signature = genTestUserSig(wx.$globalData.userID);
    const config = {
      sdkAppID: wx.$globalData.sdkAppID,
      userID: wx.$globalData.userID,
      userSig: Signature.userSig,
    };
    this.setData(
      {
        config: { ...this.data.config, ...config },
      },
      () => {
        // 将初始化后的TUICallKit实例注册到this.TUICallKit ，this.TUICallKit  可使用TUICallKit所以方法功能。
        this.TUICallKit = this.selectComponent('#TUICallKit-component');
        //初始化TUICallKit
        this.TUICallKit.init();
      },
    );
  },

     /**
     * 生命周期函数--监听页面卸载
     */
    onUnload() {
    this.TUICallKit.destroyed();
    },

    call: function() {
        // 注：初始化完成后调用call方法
        // type 1：语音通话，2：视频通话。
        this.TUICallKit.call({ userID: this.data.userId, type:wx.$TUICallEngine.MEDIA_TYPE.VIDEO})
    },

    call() { 
        // 注：初始化完成后调用call方法
        // type 1：语音通话，2：视频通话。
     this.TUICallKit.call({ userID: this.data.invitee.userID, type: 2 });
          }
    })
```

3.在 WXML 模板 中添加一个 TUICallKit 组件，例如示例代码中的 pages/index/index.wxml：
```javascript
// 可参考 MiniProgram/pages/videoCall/videoCall.wxml 或 MiniProgram/pages/audioCall/audioCall.wxml
  <TUICallKit
    id="TUICallKit-component"
    config="{{config}}"
  ></TUICallKit>
```

### 步骤六：用 JS 代码动态设置 Config 参数
在您的项目中添加 javascript 的代码，用于设置 wxml 文件中的 {{config}} 变量。这部分工作可参考 MiniProgram/pages/videoCall/videoCall.js 或 MiniProgram/pages/audioCall/audioCall.js中的示例代码，如下所示：
```javascript
// 引入 userSig 生成函数
import { genTestUserSig } from '../../debug/GenerateTestUserSig';
Page({
    /**
     * 页面的初始数据
     */
    data: {
        config: {
            sdkAppID: 0, // 替换为步骤三中获取的 SDKAppID
            userID: userID,   // 填写当前用的 userID
            userSig: genTestUserSig(userID), // 通过 genTestUserSig(userID) 生成
            tim: null,   // 如果您的项目中未集成。
        }
    }
})
```
这里详细介绍一下 config 中的几个参数：
sdkAppID：在步骤三中您已经获取到，这里不再赘述。
userId：当前用户的 ID，字符串类型，只允许包含英文字母（a-z 和 A-Z）、数字（0-9）、连词符（-）和下划线（_）。
userSig：使用步骤三中获取的 SecretKey 对 sdkAppID、userId 等信息进行加密，就可以得到 UserSig，它是一个鉴权用的票据，用于腾讯云识别当前用户是否能够使用 TRTC 的服务，更多信息请参见 如何计算及使用 UserSig。

### 步骤七：发起视频通话请求
实现1对1视频通话
在 JS 逻辑交互例如 pages/index/index.js 中填写如下代码，就可以实现一对一视频通话。
```javascript
// 发起1对1视频通话，假设 userId为: 1111, type 1：语音通话，2：视频通话。
this.TUICallKit.call({ userID: '1111', type: 2 });
```
实现事件通知监听
在 TUICallKit 组件内部 MiniProgram/TUICallKit/TUICallKit.js 内部已经默认实现远端用户接听、远端用户挂断的处理逻辑。业务侧也可以进行主动监听，并做相应的业务处理。在 JS 逻辑交互，例如 pages/index/index.js 中填写如下代码。
```javascript
// 可参考 MiniProgram/pages/videoCall/videoCall.js 或 MiniProgram/pages/audioCall/audioCall.js 里 onLoad 生命周期里的逻辑
// ...
onLoad: function() {
    // ...
    this.TUICallKit = this.selectComponent('#TUICallKit-component');
    this.TUICallKit.init();
    // 用户接听
      wx.$TUICallEngine.on(EVENT.USER_ACCEPT, this.handleUserAccept, this);
       // 用户拒绝通话
      wx.$TUICallEngine.on(EVENT.REJECT, this.handleInviteeReject, this);
     // 用户无响应
      wx.$TUICallEngine.on(EVENT.NO_RESP, this.handleNoResponse, this);
    // 更多事件可参考组件内部事件处理逻辑 MiniProgram/TUICallKit/TUICallKit.js _addTSignalingEvent方法
},

handleUserAccept: function(event) {
    console.log(`远端${event.data.userID}已接通电话`);
},
handleInviteeReject: function(event) {
    console.log(`远端${event.data.invitee}拒绝电话`);
},
handleNoResponse: function(event) {
    console.log(`远端${event.data.timeoutUserList[0]}无应答`);
},
// ...
onUnload: function() {
    wx.$TUICallEngine.off(EVENT.USER_ACCEPT, this.handleUserAccept);
    wx.$TUICallEngine.off(EVENT.REJECT, this.handleInviteeReject);
    wx.$TUICallEngine.off(EVENT.NO_RESP, this.handleNoResponse);
    this.TUICallKit.destroyed()
},
// ...
```

