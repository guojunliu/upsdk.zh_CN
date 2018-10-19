## SDK初始化
### SDK初始化

仅以Xcode工程作示例讲解，若你使用的是其它工程请参考Xcode工程操作，若有不便敬请谅解。

假定以AppDelegate（实现UIApplicationDelegate）作为IOS工程的主类文件，UPSDK的初始化非常简单。

 1. 在AppDelegate.m添加如下引用

```objective-c
#import   <UPSDK/UPSDK.h>
```

 2. 在App启动入口方法中初始化SDK

```objective-c
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    [UPSDK initSDK];

    // your other code
    
    return YES;
}
```

如果您能确定您应用的发行区域，请使用下边的代码进行初始化

```objective-c
/**
 初始化SDK（根据发行区域）

 @param zone 发行区域
 */
+ (void)initSDK:(UPSDKGlobalZone)zone;
```

其中`UPSDKGlobalZone`为枚举，如下

```objective-c
typedef NS_ENUM(NSUInteger, UPSDKGlobalZone) {
    UPSDKGlobalZoneForeign = 0,     //海外
    UPSDKGlobalZoneDomestic = 1,    //中国大陆
    UPSDKGlobalZoneAuto = 2,        //自动根据ip判断
};
```

如您的应用发行区域为中国大陆，则

```objective-c
[UPSDK initSDK:UPSDKGlobalZoneDomestic];
```

如您的应用发行区域为海外，则

```objective-c
[UPSDK initSDK: UPSDKGlobalZoneForeign];
```

如您的应用发行区域为海外和中国大陆，则

```objective-c
[UPSDK initSDK: UPSDKGlobalZoneAuto];
```
