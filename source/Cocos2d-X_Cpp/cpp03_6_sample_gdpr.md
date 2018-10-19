
## GDPR
`GDPR《一般数据保护法案》`是欧盟出台的数据保护方案，如果您的产品会面向欧盟用户，我们提供如下方案确保`UPSDK`遵守`GDPR`规范。

### GDPR 推荐用例
#### 方案一
推荐根据自己游戏的画面风格自定义GDPR的授权弹框界面，保证产品体验最佳。
采用此方案时，仅需要将授权结果同步给UPSDK后再初始化UPSDK。

示例代码：
```csharp
{
    // ...

    UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus result = UpltvBridge::getAccessPrivacyInfoStatus();
    if (result == UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus::UPAccessPrivacyInfoStatusUnkown) {
        // 如果没有询问过授权，先定位用户是否是欧盟地区
        // gdprUeropeanLocationCallback异步回调函数
        UpltvBridge::isEuropeanUnionUser(gdprUeropeanLocationCallback);
    } else {
        // 假定发行地区是海外, 参数传0
        UpltvBridge::initSdkByZone(0);
    }

    // .....
}

void gdprUeropeanLocationCallback(bool isUeropeanUser) {
    // isUeropeanUser: true 表示欧盟地区用户，否则非欧盟地区用户
    if (isUeropeanUser) {
        // 欧盟地区用户，调用自定义授权询问方法
        // callYourOwnGDPRDialog()，是我们假定的方法
        // yourOwnGDPRCallback，是我们假定的授权回调方法
        // 请根据实际代码替换
        callYourOwnGDPRDialog(yourOwnGDPRCallback);

    } else {
        // 非欧盟地区用户，直接初始化SDK
        // 假定发行地区是海外, 参数传0
        UpltvBridge::initSdkByZone(0);
    }
}

private void yourOwnGDPRCallback(bool result) {
    // 如果result : true 表示用户接受授权，false拒绝授权
    // 请参考以下代码完成UPSDK的授权同步与初始化
    if (result) {
        UpltvBridge::updateAccessPrivacyInfoStatus(UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus::UPAccessPrivacyInfoStatusAccepted);
    } else {
        UpltvBridge::updateAccessPrivacyInfoStatus(UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus::UPAccessPrivacyInfoStatusDefined);
    }
 
    // 调用updateAccessPrivacyInfoStatus()之后再初始化UPSDK
    // 假定发行地区是海外, 参数传0
    UpltvBridge::initSdkByZone(0);
}
```


#### 方案二
如果采用UPSDK提供的标准授权处理机制，请参考以下代码修改UPSDK的初始化流程。

示例代码：

```csharp

{
    // .....

    UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus result = UpltvBridge::getAccessPrivacyInfoStatus();
    if (result == UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus::UPAccessPrivacyInfoStatusUnkown) {
        // 如果没有询问过授权，先定位用户是否是欧盟地区
        // gdprUeropeanLocationCallback异步回调函数
        UpltvBridge::isEuropeanUnionUser(gdprUeropeanLocationCallback);
    } else {
        // 假定发行地区是海外
        UpltvBridge::initSdkByZone(0);
    }

    // .....
}

void gdprUeropeanLocationCallback(bool result) {
    // result: true 表示欧盟地区用户，否则非欧盟地区用户
    if (result) {
        // 欧盟地区用户，进行授权询问
        UpltvBridge::notifyAccessPrivacyInfoStatus(gdprNotifyCallback);
    } else {
        // 非欧盟地区用户，直接初始化SDK
        // 假定发行地区是海外
        UpltvBridge::initSdkByZone(0);
    }
}

void gdprNotifyCallback(UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus result, string msg) {
    // result 用户授权的结果
    // 不论结果如何，都要初始化sdk
    // 假定发行地区是海外
    UpltvBridge::initSdkByZone(0);
}
```