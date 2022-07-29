## TUICalling常见问题

### Q：TUICalling 是否可以不引入IM SDK，只使用TRTC呢？
A：不可以。

TUIKit 全系组件都使用了腾讯云 IM SDK 做为通信的基础服务，比如通话拨打信令、通话忙线信令等核心逻辑，如果您已经购买有其他 IM 产品，也可以参照 TUICalling 逻辑进行适配；


### Q：TUICalling 是否支持离线推送呢？
A：支持。

Android离线推送采用腾讯云的`TUIOfflinePush`组件，详情请见：[TUIOfflinePush](https://cloud.tencent.com/document/product/269/44516)；

在 [TUICalling](https://github.com/tencentyun/TUICalling/tree/main/Android) 组件中，我们也接入了该组件，具体的体验方式可参考：[Android离线推送接入指引](https://github.com/tencentyun/TUICalling/blob/main/Android/Android%E7%A6%BB%E7%BA%BF%E6%8E%A8%E9%80%81%E6%8E%A5%E5%85%A5%E6%8C%87%E5%BC%95.md)。
