## API说明
### UPSDK Unity Plugin类引用
```csharp
using Polymer.PolyADSDK ;
```

### UPSDK API
版本3003，我们修改了Unity Plugin的访问类为`UPSDK.cs`，但依然兼容了`PolyADSDK.cs`所有的方法与回调接口。
因此对于3003及更高的版本，建议使用UPSDK类调用Unity Plugin的所有API与回调接口。


```csharp
/*
* 获取当前插件的版本号
*/
public static string getVersionOfPlugin();

/*
* 获取当前插件所支持平台的版本号
* 仅支持ios与android平台，运行时根据编译环境自动匹
*/
public static string getVersionOfPlatform();

/*
* 初始化 UPSDK
* 2030以前的版本，请用此方法初始化
*/
public static string initPolyAdSDK();

/*
* 初始化 UPSDK
* @param zone:0，海外；1，中国大陆；2，自动根据ip判断用户类型
* 如果使用常量类型，自3003开始请通过UPConstant类调用常量：SDKZONE_FOREIGN,SDKZONE_CHINA,SDKZONE_AUTO；
* 3003以前的版本仍然通过PolyADSDK类调用以上常量。
* 2030及以后的版本，请用此方法初始化
*/
public static string initPolyAdSDK(int zone);

/**
* 初始化avidly的聚合广告abtest配置
*
* @param gameAccountId        用户在游戏中的帐号id（必填）
* @param completeTask         是否完成了游戏中的新手任务
* @param isPaid               是否付费用户，0则未付费
* @param promotionChannelName 推广渠道，没有可以传 ""
* @param gender               "M", "F"，未知可以传""
* @param age                  未知可以传-1
* @param tags                 标签，没有可以传 null
*/
public static void initAbtConfigJson(string gameAccountId, bool completeTask, int isPaid, string promotionChannelName,  string gender, int age, string[] tags) ;

/*
* 获取abtest配置
* 返回结果为Json字符串，可能为null
* 调用此API前，请调用initAbtConfigJson()完成abtest配置的初始化
*/
public static string getAbtConfig(string placementId);
 
 /*
 * 判断插屏广告是否可以显示
 */
public static bool isInterstitialReady(string cpPlaceId);

/*
* 判断激励视屏是否可以加载播放
* 请不要在代码中持续不断地调用此方法来实现某个特定目标，如更新UI按钮的状态。对于这种情况，请用setRewardVideoLoadCallback()设置回调监听。
*/
public static bool isRewardReady();

/*
* 显示激励视屏，参数cpCustomId为CP自定义标识，不能为空(广告无法显示)
*/
public static void showRewardAd(string cpCustomId);

/*
* 显示插屏广告
* 参数cpPlaceId为广告位标识，设置错误会无法显示广告
* 2037开始，替换拼写错误方法：showIntersitialAd()
*/
public static void showInterstitialAd(string cpPlaceId);

/*
* 在屏幕顶部显示长条型横幅（Banner）广告
* 参数 cpPlaceId为广告位标识，设置错误会无法显示广告
*/
public static void showBannerAdAtTop(string cpPlaceId);

/*
* 在屏幕底部显示长条型横幅（Banner）广告
*/
public static void showBannerAdAtBottom(string cpPlaceId);

/*
 * 隐藏当前顶部的Banner广告
 * 无须区分广告位
 * 再次展示时，需要调用showBannerAdAtTop()
 * from 2037开始支持
 */
public static void hideBannerAdAtTop();

/*
 * 隐藏当前底部的Banner广告
 * 无须区分广告位
 * 再次展示时，需要调用showBannerAdAtBottom()
 * from 2037开始支持
 */
public static void hideBannerAdAtBottom();

/*
* 删除某条横幅（Banner）广告
*/
public static void removeBannerAdAt(string cpPlaceId);

/*
* 设置Android平台 manifest.xml中所定义的packagename
* 2031以后，此方法被废弃，UPSDK初始化后不需要再调用
*/
public static void setManifestPackageName(string packagename);

/*
* 显示退出广告（对于Android平台有效）
*/
public static void onBackPressed();

/*
 * upltv激励视屏广告加载回调接口
 * @param success 第一个参数，加载成功后的回调
 * @param fail 第二个参数，加载失败后的回调
 * 
 * 回调参数类型：Action&lt;string,string>
 * 第一个为cpPlaceId，广告位，可能为空或null；第二个参数为描述信息，可能为空或null
 * supported from 2028
 */
public static void setRewardVideoLoadCallback(Action&lt;string,string> success, Action&lt;string, string> fail);

/*
 * upltv插屏广告加载回调接口
 * @param cpPlaceId: 第一个参数，插屏广告位标识符，不能为空或null
 * @param success 第二个参数，加载成功后的回调
 * @param fail 第三个参数，加载失败后的回调
 * 
 * 回调参数类型：Action&lt;string,string>
 * 第一个为cpPlaceId，广告位，可能为空或null；第二个参数为描述信息，可能为空或null
 * supported from 2028
 */
public static void setIntersitialLoadCallback(string cpPlaceId, Action&lt;string,string> success, Action&lt;string, string> fail)

/*
 * 用于展示upltv的激励视屏广告调试界面
 * supported from 2028
 */
public static void showRewardDebugView();

/*
 * 用于展示avidly的插屏广告调试界面
 * supported from 2028
 */
public static void showInterstitialDebugView();

/**
* 满足需求：不希望在初始化自动加载广告，且要求根据游戏自主选择合适的时机进行广告加载
* 运行条件：当sdk默认禁用广告自动加载的功能，且upltv后台云配也关闭此功能时
* 如果以上条件不成立，即使调用以下方法，SDK也会自动忽略
* supported from 3002
*/
public static void loadAvidlyAdsByManual() ;

/**
* 对Iphonex手机，顶部Banner被状态栏遮挡时，可以通过调节顶部Banner的偏移值，解决此问题
* @param padding: 顶部Banner的偏移值，如32，则状态样会向下偏移32像素
* Android平台不支持此功能
* supported from 3002
*/
public static void setTopBannerForIphonex(int padding);

/**
 * 对Android 华为P20手机，顶部Banner被状态栏遮挡时，可以通过调节顶部Banner的偏移值，解决此问题
 * @param padding: 顶部Banner的偏移值，如75，则状态样会向下偏移75像素
 * supported from 3004
 */
 public void setTopBannerForHuaWeiP20(int padding);

/**
* 弹出授权窗口，向用户说明重要数据收集的情况并询问用户是否同意授权
* 如果用户拒绝授权将放弃相关数据的收集
* 可以在初始UPSDK之前调用
* @param callback
* Version 3003 and above support this method
*/
public static void notifyAccessPrivacyInfoStatus(Action&lt;UPConstant.UPAccessPrivacyInfoStatusEnum, string> callback);

/**
* 外部进行GDPR授权时，将用户授权结果同步到UPSDK时，调用此方法
* 可以在初始UPSDK之前调用
* @param enumValue
* Version 3003 and above support this method
*/
public static void updateAccessPrivacyInfoStatus(UPConstant.UPAccessPrivacyInfoStatusEnum enumValue);

/**
* 获取用户授权结果，可以在初始UPSDK之前调用
* return UPConstant.UPAccessPrivacyInfoStatusEnum
* Version 3003 and above support this method
*/
public static UPConstant.UPAccessPrivacyInfoStatusEnum getAccessPrivacyInfoStatus();

/**
* 判断用户是否属于欧盟地区
* 异步回调，可以在初始UPSDK之前调用
* Version 3003 and above support this method
*/
public static void isEuropeanUnionUser(Action&lt;bool, string> callback);
```
### 需要特殊说明的方法

#### 1. 仅支持条型横幅广告

在插件模式下，**仅支持条型横幅广告**，而且显示位置固定在当前窗口的**顶部或底部**！

#### 2. **setManifestPackageName**(string packagename)

此API仅适用于Android，对于2031以下的版本非常重要。当AndroidManifest中,Application所设置的package name与实际打包的名字不一样时，请务必通过此API向SDK设置**AndroidManifest**所对应的**package name**!!!

#### 3. 退出广告

当用户即将退出当前游戏时，调用**onBackPressed()**，可以显示一个广告页面。