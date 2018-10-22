
## GDPR
`GDPR《一般数据保护法案》`是欧盟出台的数据保护方案，如果您的产品会面向欧盟用户，我们提供如下方案确保`UPSDK`遵守`GDPR`规范。

`UPSDK`在`3.0.03`版本支持欧盟`GDPR`规范，发行区域包含欧盟或涵盖欧盟用户的开发者必须处理此逻辑。

### GDPR 推荐用例
#### 方案一
推荐根据自己游戏的画面风格自定义GDPR的授权弹框界面，保证产品体验最佳。
采用此方案时，仅需要将授权结果同步给UPSDK后再初始化UPSDK。

示例代码：
```csharp
{
    // 旧的初始化代码
    // AvidlyAdsSdk.init(xxxx);

    // 新的初始化代码
    AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum result=UPAdsSdk.getAccessPrivacyInfoStatus(MainActivity.this);
    if (result ==  AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusUnkown
        || result ==  AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined) {
        // 如果没有询问过授权，先定位用户是否是欧盟地区
        // isEuropeanUserCallback异步回调对象
        UPAdsSdk.isEuropeanUnionUser (MainActivity.this,isEuropeanUserCallback);
    } else {
        // 假定发行地区是海外
        UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
    }

    // .....
}

UPAdsSdk.UPEuropeanUnionUserCheckCallBack isEuropeanUserCallback=new UPAdsSdk.UPEuropeanUnionUserCheckCallBack(){
    @Override
    public void isEuropeanUnionUser(boolean result) {
        if (result) {
            // 欧盟地区用户，进行自定义授权询问
            // ......
            // ......
    
            // 请调用你的方法，并根据回调做以下处理
            // 如果用户拒绝，调用以下方法同步到UPSDK
            UPAdsSdk.updateAccessPrivacyInfoStatus(this, AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined);
  
            // 如果用户接受授权，调用以下方法同步到UPSDK
            UPAdsSdk.updateAccessPrivacyInfoStatus(this, AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusAccepted);

            // 调用updateAccessPrivacyInfoStatus()之后再初始化UPSDK
            UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);

        } else {
            // 非欧盟地区用户，直接初始化SDK
            // 假定发行地区是海外
            UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
        }
    }
};

```

#### 方案二
如果采用UPSDK提供的标准授权处理机制，请参考以下代码修改UPSDK的初始化流程。

示例代码：

```csharp

{
    // 旧的初始化代码
    // AvidlyAdsSdk.init(xxxx);

    // 新的初始化代码
    AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum result=UPAdsSdk.getAccessPrivacyInfoStatus(MainActivity.this);
    if (result ==  AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusUnkown
        || result ==  AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined) {
        // 如果没有询问过授权，先定位用户是否是欧盟地区
        // isEuropeanUserCallback异步回调对象
        UPAdsSdk.isEuropeanUnionUser (MainActivity.this,isEuropeanUserCallback);
    } else {
        // 假定发行地区是海外
        UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
    }
    // .....
}

UPAdsSdk.UPEuropeanUnionUserCheckCallBack isEuropeanUserCallback = new UPAdsSdk.UPEuropeanUnionUserCheckCallBack(){
    @Override
    public void isEuropeanUnionUser(boolean result) {
        if (result) {
            // result: true 表示欧盟地区用户，否则非欧盟地区用户
            // ......
            // ......

            //弹出系统弹窗询问
             UPAdsSdk.notifyAccessPrivacyInfoStatus(MainActivity.this,myAccessPrivacyStatusInfoCallBack);
        } else {
            // 非欧盟地区用户，直接初始化SDK
            // 假定发行地区是海外
            UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
        }
    }
};

private UPAdsSdk.UPAccessPrivacyInfoStatusCallBack myAccessPrivacyStatusInfoCallBack = new UPAdsSdk.UPAccessPrivacyInfoStatusCallBack() {
    @Override
    public void onAccessPrivacyInfoAccepted() {
        Log.i(TAG, "onAccessPrivacyInfoAccepted:");
        // ......
        // ......
        // 假定发行地区是海外
        UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
    }

    @Override
    public void onAccessPrivacyInfoDefined() {
        Log.i(TAG, "onAccessPrivacyInfoDefined:");
        // ......
        // ......
        // 假定发行地区是海外
        UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
    }

};
```
### GDPR API介绍

#### 1.notifyAccessPrivacyInfoStatus
弹出授权窗口，向用户说明重要数据收集的情况并询问用户是否同意授权，如果用户拒绝授权将放弃相关数据的收集。请在初始化UPSDK之前调用。

```csharp
public static void notifyAccessPrivacyInfoStatus(final Context context, final UPAdsSdk.UPAccessPrivacyInfoStatusCallBack callback)
```
示例代码：

```csharp
UPAdsSdk.notifyAccessPrivacyInfoStatus(MainActivity.this,myAccessPrivacyStatusInfoCallBack);

private UPAdsSdk.UPAccessPrivacyInfoStatusCallBack myAccessPrivacyStatusInfoCallBack =new UPAdsSdk.UPAccessPrivacyInfoStatusCallBack() {
   @Override
   public void onAccessPrivacyInfoAccepted() {
       Log.i(TAG, "onAccessPrivacyInfoAccepted: ");
   }

   @Override
   public void onAccessPrivacyInfoDefined() {
       Log.i(TAG, "onAccessPrivacyInfoDefined");
   }
};
  
```


#### 2.updateAccessPrivacyInfoStatus
外部进行GDPR授权时，调用此方法将用户授权结果同步到UPSDK，UPSDK将不再进行授权弹窗管理。请在初始化UPSDK之前调用。

```csharp
public static void updateAccessPrivacyInfoStatus(final Context context,UPConstant.UPAccessPrivacyInfoStatusEnum enumValue)
```

示例代码：

```csharp
/**
* 同意将设设备信息传递给sdk
*/
UPAdsSdk.updateAccessPrivacyInfoStatus(this, AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusAccepted);

/**
 * 不同意将设设备信息传递给sdk
 */
UPAdsSdk.updateAccessPrivacyInfoStatus(this, AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined);

```


#### 3.getAccessPrivacyInfoStatus
获取用户授权结果，可以在初始化UPSDK之前调用。

```groovy
public static UPAccessPrivacyInfoStatusEnum getAccessPrivacyInfoStatuss(Context context)
```

示例代码：
```csharp
AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum result=UPAdsSdk.getAccessPrivacyInfoStatus(MainActivity.this);
```

#### 4.isEuropeanUnionUser
判断用户是否属于欧盟地区，异步返回结果。
可以在初始化UPSDK之前调用。
```csharp
public static void isEuropeanUnionUser(Context context, UPAdsSdk.UPEuropeanUnionUserCheckCallBack callback)
```
示例代码：
```csharp
 UPAdsSdk.isEuropeanUnionUser(MainActivity.this, new UPAdsSdk.UPEuropeanUnionUserCheckCallBack() {
    @Override
    public void isEuropeanUnionUser(boolean isEurope) {
        if (isEurope) {
            //处于欧盟区域内
        } else {
            //不处于欧盟区域
        }
    }
});
```

