# 初始化


## 引用SDK
在需要使用的类中引入AvidlyAnalysisSDK.h
```objective-c
# import  &lt;AvidlyAnalysisSDK/AvidlyAnalysisSDK.h>
```

## 初始化SDK

- 初始化方法 （一）
```objective-c
[HolaAnalysis initWithProductId:@"your productId" ChannelId:@"your channelId" AppID:@"your app id"];
```

- 初始化方法 （二）
```objective-c
[HolaAnalysis initWithProductId:@"your productId" ChannelId:@"your channelId" AppID:@"your app id" zone:your zone enum];
```

参数说明
- productId ：分配的产品编号
- channelId ：渠道编号
- appId ：为空或"id" + Apple ID （注意：AppID非空时要带上id，例如：id1128308845）
- zone ：发行区域枚举（中国大陆/海外）

示例代码：
```objective-c
[HolaAnalysis initWithProductId:@"999999" ChannelId:"32407" AppID:@"" zone:0];
```

## GDPR
如果欧盟用户禁止使用用户信息，则在SDK初始化之前调用。
```objective-c
+ (void)disableAccessPrivacyInformation;
```

