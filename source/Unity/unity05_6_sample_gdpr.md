## GDPR
`GDPR《一般数据保护法案》`是欧盟出台的数据保护方案，如果您的产品会面向欧盟用户，我们提供如下方案确保`UPSDK`遵守`GDPR`规范。

### GDPR 推荐用例
#### 方案一
推荐根据自己游戏的画面风格自定义GDPR的授权弹框界面，保证产品体验最佳。
采用此方案时，仅需要将授权结果同步给UPSDK后再初始化UPSDK。

示例代码：
```csharp
{
    // 旧的初始化代码
    // PolyADSDK.initPolyAdSDK (xxxx);

    // 新的初始化代码
    UPConstant.UPAccessPrivacyInfoStatusEnum result = Polymer.UPSDK.getAccessPrivacyInfoStatus();
    if (result == UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusUnkown
        || result == UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusFailed) {
        // 如果没有询问过授权，先定位用户是否是欧盟地区
        // isEuropeanUserCallback异步回调对象
        UPSDK.isEuropeanUnionUser (new Action<bool, string>(isEuropeanUserCallback));
    } else {
        // 假定发行地区是海外
        UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);
    }

    // .....
}

private void isEuropeanUserCallback(bool result, string msg) {
    // result: true 表示欧盟地区用户，否则非欧盟地区用户
    if (result) {
        // 欧盟地区用户，调用自定义授权询问方法
        // callYourOwnGDPRDialog()，是我们假定的方法
        // yourOwnGDPRCallback，是我们假定的授权回调方法
        // 请根据实b际代码替换
        callYourOwnGDPRDialog(yourOwnGDPRCallback);

    } else {
        // 非欧盟地区用户，直接初始化SDK
        // 假定发行地区是海外
        UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);
    }
}

private void yourOwnGDPRCallback(bool result) {
    // 如果result : true 表示用户接受授权，false拒绝授权
    // 请参考以下代码完成UPSDK的授权同步与初始化
    if (result) {
        UPSDK.updateAccessPrivacyInfoStatus(UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusAccepted);
    } else {
        UPSDK.updateAccessPrivacyInfoStatus(UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined);
    }
 
    // 调用updateAccessPrivacyInfoStatus()之后再初始化UPSDK
    // 假定发行地区是海外
    UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);
}
```

#### 方案二
如果采用UPSDK提供的标准授权处理机制，请参考以下代码修改UPSDK的初始化流程。

示例代码：

```csharp

{
    // .....

    // 旧的初始化代码
    // PolyADSDK.initPolyAdSDK (xxxx);

    // 新的初始化代码
    UPConstant.UPAccessPrivacyInfoStatusEnum result = Polymer.UPSDK.getAccessPrivacyInfoStatus();
    if (result == UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusUnkown
        || result == UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusFailed) {
        // 如果没有询问过授权，先定位用户是否是欧盟地区
        // isEuropeanUserCallback异步回调对象
        UPSDK.isEuropeanUnionUser (new Action<bool, string>(isEuropeanUserCallback));
    } else {
        // 假定发行地区是海外
        UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);
    }

    // .....
}

private void isEuropeanUserCallback(bool result, string msg) {
    // result: true 表示欧盟地区用户，否则非欧盟地区用户
    if (result) {
        // 欧盟地区用户，进行授权询问
        UPSDK.notifyAccessPrivacyInfoStatus (new Action<UPConstant.UPAccessPrivacyInfoStatusEnum, string> (accessPrivacyInforCallback));
    } else {
        // 非欧盟地区用户，直接初始化SDK
        // 假定发行地区是海外
        UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);
    }
}

private void accessPrivacyInforCallback(UPConstant.UPAccessPrivacyInfoStatusEnum result, string msg) {
    // result 用户授权的结果
    // 不论结果如何，都要初始化sdk
    // 假定发行地区是海外
    UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);
    // 打印日志
    Debug.Log ("===> accessPrivacyInforCallback Event result: " + result + "," + msg);
}
```

### GDPR API介绍

#### 1.notifyAccessPrivacyInfoStatus
弹出授权窗口，向用户说明重要数据收集的情况并询问用户是否同意授权，如果用户拒绝授权将放弃相关数据的收集。请在初始化UPSDK之前调用。

```csharp
public static void notifyAccessPrivacyInfoStatus(Action<UPConstant.UPAccessPrivacyInfoStatusEnum, string> callback)
```
示例代码：

```csharp
public void onBtnNotifyAccessStatus_Click() {
    Polymer.UPSDK.notifyAccessPrivacyInfoStatus (new Action<UPConstant.UPAccessPrivacyInfoStatusEnum, string>(accessPrivacyInforCallback));
}

private void accessPrivacyInforCallback(UPConstant.UPAccessPrivacyInfoStatusEnum result, string msg) {
     Debug.Log ("===> accessPrivacyInforCallback Event result: " + result + "," + msg);
}
```


#### 2.updateAccessPrivacyInfoStatus
外部进行GDPR授权时，调用此方法将用户授权结果同步到UPSDK，UPSDK将不再进行授权弹窗管理。请在初始化UPSDK之前调用。

```csharp
public static void updateAccessPrivacyInfoStatus(UPConstant.UPAccessPrivacyInfoStatusEnum enumValue)
```

示例代码：

```csharp

public void onBtnUpdateAccessStatus_Click() {
    Polymer.UPSDK.updateAccessPrivacyInfoStatus(UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined);
}
```


#### 3.getAccessPrivacyInfoStatus
获取用户授权结果，可以在初始化UPSDK之前调用。

```csharp
public static UPConstant.UPAccessPrivacyInfoStatusEnum getAccessPrivacyInfoStatus()
```

示例代码：
```csharp
public void onBtnGetAccessStatus_Click() {
    UPConstant.UPAccessPrivacyInfoStatusEnum e = Polymer.UPSDK.getAccessPrivacyInfoStatus();
    Debug.Log ("==> getAccessPrivacyInfoStatus :" + e);
}
```

#### 4.isEuropeanUnionUser
判断用户是否属于欧盟地区，异步返回结果。
可以在初始化UPSDK之前调用。
```csharp
public static void isEuropeanUnionUser(Action<bool, string> callback)
```
示例代码：
```csharp
public void onBtnIsEuropeanUnionUser_Click() {
    Polymer.UPSDK.isEuropeanUnionUser (new Action<bool, string>(isEuropeanUserCallback));
}

private void isEuropeanUserCallback(bool result, string msg) {
    Debug.Log ("===> isEuropeanUserCallback Event result: " + result + "," + msg);
}
```
