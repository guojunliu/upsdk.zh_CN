## GDPR
`GDPR《一般数据保护法案》`是欧盟出台的数据保护方案，如果您的产品会面向欧盟用户，我们提供如下方案确保`UPSDK`遵守`GDPR`规范。

`UPSDK`在`3.0.03`版本支持欧盟`GDPR`规范，发行区域包含欧盟或涵盖欧盟用户的开发者必须处理此逻辑。

### GDPR 推荐用例
#### 方案一
推荐根据自己游戏的画面风格自定义GDPR的授权弹框界面，保证产品体验最佳。
采用此方案时，仅需要将授权结果同步给UPSDK后再初始化UPSDK。

示例代码：
```objective-c
{
    // 旧的初始化代码
    // [UPSDK initSDK];
    
    // 判断是否为第一次运行
    UPAccessPrivacyInfoStatus result1 = [UPSDK getCurrentAccessPrivacyInfoStatus];
    if (result1 == UPAccessPrivacyInfoStatusNone) {
        // 是第一次运行
        // 查询用户归属
        [UPSDK checkIsEuropeanUnionUser:^(BOOL isEuropeanUnion) {
            if (isEuropeanUnion) {
                // 是欧盟用户
                // 请注意 [Test yourOwnMethod: completion]是伪代码,请根据实际需求进行修改
                [Test yourOwnMethod:nil completion:^(BOOL isAccepted) {
                     // isAccepted为YES表示用户同意使用隐私信息，为NO表示用户不同意使用隐私信息
                     // 更新GDPR状态,获取用户信息授权
                    NSLog(@"CP可自由在这里写入自己的逻辑，接下来只需要更新一下GDPR状态即可");
                    if (isAccepted) {
                        // 用户同意使用隐私信息：
                        [UPSDK updateAccessPrivacyInfoStatus:UPAccessPrivacyInfoStatusAccepted];
                    }
                    else {
                        // 用户拒绝使用隐私信息：
                        [UPSDK updateAccessPrivacyInfoStatus:UPAccessPrivacyInfoStatusDenied];
                    }
                    // 初始化SDK
                    // 假设发行地区为海外，则如下所示：
                    [UPSDK initSDK:UPSDKGlobalZoneForeign];
                }];
            }
            else {
                // 不是欧盟用户
                // 初始化SDK
                // 假设发行地区为海外，则如下所示：
                [UPSDK initSDK:UPSDKGlobalZoneForeign];
            }
        }];
    }
    else {
        // 不是第一次运行
        // 假设发行地区为海外，则如下所示：
        [UPSDK initSDK:UPSDKGlobalZoneForeign];
    }
}
```
#### 方案二
如果采用UPSDK提供的标准授权处理机制，请参考以下代码修改UPSDK的初始化流程。

示例代码：
```objective-c
{
    // 旧的初始化代码
    // [UPSDK initSDK];

    // 判断是否为第一次运行
    UPAccessPrivacyInfoStatus result = [UPSDK getCurrentAccessPrivacyInfoStatus];
    if (result == UPAccessPrivacyInfoStatusNone) {
        // 是第一次运行
        // 查询用户归属
        [UPSDK checkIsEuropeanUnionUser:^(BOOL isEuropeanUnion) {
            if (isEuropeanUnion) {
                // 是欧盟用户
                [UPSDK requestAuthorizationWithAlert:nil completion:^(BOOL isAccepted) {
                    // isAccepted为YES表示用户同意使用隐私信息，为NO表示用户不同意使用隐私信息
                    // 初始化SDK
                    // 假设发行地区为海外，则如下所示：
                    [UPSDK initSDK:UPSDKGlobalZoneForeign];
                }];
            }
            else {
                // 不是欧盟用户
                // 初始化SDK
                // 假设发行地区为海外，则如下所示：
                [UPSDK initSDK:UPSDKGlobalZoneForeign];
            }
        }];
    }
    else {
        // 不是第一次运行
        // 假设发行地区为海外，则如下所示：
        [UPSDK initSDK:UPSDKGlobalZoneForeign];
    }
}
```
### GDPR API介绍
#### 一、开发者可以自行获取GDPR状态的

 针对可以自行获取GDPR状态的的开发者，UPSDK提供了设置GDPR状态的API`(此API需要在SDK初始化之前调用)`，即
 
```objective-c
 /**
 更新访问用户隐私信息状态
 
 @param status 访问用户隐私信息状态，不能传UPAccessPrivacyInfoStatusNone
 */
+ (void)updateAccessPrivacyInfoStatus:(UPAccessPrivacyInfoStatus)status;
```

其中`UPAccessPrivacyInfoStatus`枚举的值为
```objective-c
typedef NS_ENUM (NSInteger, UPAccessPrivacyInfoStatus) {
    UPAccessPrivacyInfoStatusNone = 0,      //未知
    UPAccessPrivacyInfoStatusAccepted = 1,  //用户同意使用隐私信息
    UPAccessPrivacyInfoStatusDenied = 2,    //用户拒绝使用隐私信息
};
```

注：不能传入`UPAccessPrivacyInfoStatusNone `这个枚举值

demo示例

```objective-c
//------ 用户可以自行获取GDPR状态 ------
//设置GDPR状态
[UPSDK updateAccessPrivacyInfoStatus:UPAccessPrivacyInfoStatusAccepted];
//SDK init
[UPSDK initSDK:UPSDKGlobalZoneForeign];
```

---------

#### 二、开发者无法自行判断用户是否为欧盟用户的

针对此种情况，UPSDK提供了异步查询当前用户是否为欧盟用户的API，即

```objective-c
/**
 查询用户是否是欧盟用户
 
 @param completionBlock 回调 isEuropeanUnion为YES表示是欧盟用户
 */
+ (void)checkIsEuropeanUnionUser:(void (^)(BOOL isEuropeanUnion))completionBlock;
```

其中`isEuropeanUnion`为`YES`表示是欧盟用户,为`NO`表示非欧盟用户

demo示例

```objective-c
//准备查询用户归属
[UPSDK checkIsEuropeanUnionUser:^(BOOL isEuropeanUnion) {
    if (isEuropeanUnion) {
        //欧盟用户 
    }
    else {
        //非欧盟用户
}];
```

---------

#### 三、开发者无法自行向用户请求授权使用隐私信息的

针对此种情况，UPSDK提供了使用Alter向用户请求授权使用隐私信息的API，即

```objective-c
/**
 使用弹窗向用户请求访问隐私信息授权
 
 @param viewController 当前视图控制器
 @param completionBlock 回调，其中isAccepted为YES表示用户同意使用隐私信息，为NO表示用户不同意使用隐私信息
 */
+ (void)requestAuthorizationWithAlert:(UIViewController *)viewController completion:(void (^)(BOOL isAccepted))completionBlock;
```

其中`isAccepted`为`YES`表示用户同意使用隐私信息。为`NO`表示用户拒绝使用隐私信息

demo示例

```objective-c
[UPSDK requestAuthorizationWithAlert:vc completion:^(BOOL isAccepted) {
    if (isAccepted) {
        //同意授权
    }
    else {
        //拒绝授权
    }
}];
```

--------

#### 四、开发者想获知当前GDPR授权状态的

针对此种情况，UPSDK提供了获取当前GDPR授权状态的API，即

```objective-c
/**
 获取当前访问用户隐私信息状态
 
 @return 访问用户隐私信息状态
 */
+ (UPAccessPrivacyInfoStatus)getCurrentAccessPrivacyInfoStatus;
```

其中若返回值为`UPAccessPrivacyInfoStatusAccepted`表示用户同意，`UPAccessPrivacyInfoStatusDenied`表示用户拒绝，`UPAccessPrivacyInfoStatusNone`表示未知或尚未向用户请求授权

--------

#### 五、整体逻辑示例

```objective-c
// 第一步 准备查询用户归属
[UPSDK checkIsEuropeanUnionUser:^(BOOL isEuropeanUnion) {
    if (isEuropeanUnion) {
        // 是欧盟用户
        // 第二步 向用户请求使用隐私信息授权
        [UPSDK requestAuthorizationWithAlert:vc completion:^(BOOL isAccepted) {
            if (isAccepted) {
                // 用户同意使用隐私信息
                // 第三步 向SDK更新GDPR授权状态（同意）
                [UPSDK updateAccessPrivacyInfoStatus:UPAccessPrivacyInfoStatusAccepted];
            }
            else {
                // 用户拒绝使用隐私信息
                // 第三步 向SDK更新GDPR授权状态（拒绝）
                [UPSDK updateAccessPrivacyInfoStatus:UPAccessPrivacyInfoStatusDenied];
            }
            // 第四步  初始化SDK
            [UPSDK initSDK:UPSDKGlobalZoneForeign];
        }];
    }
    else {
        // 非欧盟用户
        // 第四步  初始化SDK
        [UPSDK initSDK:UPSDKGlobalZoneForeign];
    }
}];
```

其中`第一步`和`第二步`均`建议开发者自行实现`以提高用户体验，如无法自行处理的，可以使用SDK提供的对应API处理