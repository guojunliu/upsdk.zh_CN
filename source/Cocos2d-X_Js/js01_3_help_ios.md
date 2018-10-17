## IOS Xcode接入帮助

本接入文档以cocos2d-X 3.16为蓝本，如果您使用Cocos2d-X其它版本，不同之处请灵活处理。

### 1 下载SDK包
首先从 [UPSDK下载页](http://ads-sdk-doc.haloapps.com/docs/show/13 "SDK下载页面") 下载UPSDK JavaScriptPlugin包，解压后的目录包含如下三个文件：
- `UPSDK.framework` 这是UPSDK的主包，请务必添加到当前工程中
- `UPSDK.bundle` UPSDK主包需要访问的外部文件资源，请务必添加到当前工程中
- `UpltvJsBridge` 此目录包含一些*.js源码文件，用于桥接当前Cocos2d-X JavaScript工程与UPSDK广告接口调用
</br>

### 2 XCode工程添加UPSDK主包与源文件
请将UPSDK.framework、UPSDK.bundle以及UpltvJsBridge文件夹同时添加到你的Xcode工程目录下。

UpltvJsBridge文件夹包含有Object-C++源码及*.js脚本文件，如下图如示：

![](http://docs.upltv.com/uploads/201805/5b02724dc9bb5_5b02724d.png)

请将`UPLTV.js`、`UPLTVIos.js`引进到当前工程中，建议添加到`src -> upltv`路径下，如下图所示：

![](http://docs.upltv.com/uploads/201805/5b02734555c95_5b027345.png)

将`UPAdsBrigeJs.h`和`UPAdsBrigeJs.mm`引进到当前工程里，建议添加到`ios->upltv`目录下，如下图所示：

![](http://docs.upltv.com/uploads/201805/5b02736e0f931_5b02736e.png)

### 3 加入第三方依赖库
UPSDK运行时会依赖第三方广告联盟，所以需要手动将这些联盟的依赖库文件导入到你的项目中。为了保证你能正确添加第三方依赖包，请从这里[下载UPSDK联盟包](http://ads-sdk-doc.haloapps.com/docs/show/13 "SDK第三方包下载") 。

UPSDK当前依赖第三方广告联盟如下：

![添加所有第三方SDK包](http://docs.upltv.com/uploads/201709/59afafb9143e9_59afafb9.png "添加所有第三方SDK包")

> 每个联盟对应一个文件夹，且命名都以广告联盟的名字为前缀，以版本号为后缀，因此很好识别。文件夹中除了包括广告联盟的依赖库或源码文件外，还有可能包括配套的资源文件，因此添加广告联盟时依赖库与对应的配套的资源要一同添加。

虽然并不要求将全部的第三方联盟广告添加到当前项目中，但为了扩大收益，我们仍然建议在你的项目中尽量多地添加以上的联盟广告的依赖库及其配套的资源包。在实际操作中，如有疑问请联系我们的技术支持人员。

当你完成第三方联盟广告添加后，在XCode工程中检查下`Linked Frameworks Libraries`是否正确引入了相应的库文件，避免不必要的失误。

假设在当前项目中接入Applovin、Unity、Vungle、Tapjoy四家广告，需要先将这四家广告的库文件(依次是AppLovinSDK.framework,UnityAds.framework,VungleSDK.framework,Tapjoy.framework四个静态库)添加到工程里，成功添加后的效果图如下：
![](http://docs.upltv.com/uploads/201804/5acc6644c33a5_5acc6644.png)

在以上假设中，由于Tapjoy广告需要访问它一些外部资源文件，因此需要将它配套的资源文件也添加到项目中，可在TARGETS → General → Copy Bundle Resources查看，正确添加后的效果如下：
<br>
![](http://docs.upltv.com/uploads/201804/5acc70803fec8_5acc7080.png)


### 4 加入系统依赖库
在TARGETS → General → Linked Frameworks Libraries中添加依赖库
- `QuartzCore.framework`
- `MediaPlayer.framework`
- `libsqlite3.tbd`
- `libz.tbd`
- `CoreMedia.framework`
- `CoreGraphics.framework`
- `CFNetwork.framework`
- `WebKit.framework` (Optional)
- `WatchConnectivity.framework`	(Optional)
- `SystemConfiguration.framework`
- `StoreKit.framework`
- `Social.framework`
- `MessageUI.framework`
- `JavaScriptCore.framework`	(Optional)
- `EventKit.framework`
- `CoreTelephony.framework`
- `AVFoundation.framework`
- `AudioToolbox.framework`
- `AdSupport.framework`
- `GLKit.framework`
- `CoreMotion.framework`
- `libxml2.tbd`
- `libc++.tbd`
- `SafariServices.framework`
- `CoreLocation.framework`
- `EventKitUI.framework`
- `MobileCoreServices.framework`
- `GameController.framework`
<br>

### 工程配置
#### 1 添加分类编译符

- 在`TARGETS` → `Build Setting` → `Linking` → `Other Linker Flags` 中加入 `-ObjC` 如下图所示
![](http://docs.upltv.com/uploads/201804/5ae28e70be73c_5ae28e70.png)

#### 2 在info.plist中加入以下节点，以兼容http模式

```objective-c
&lt;key>NSAppTransportSecurity&lt;/key>
&lt;dict>
	&lt;key>NSAllowsArbitraryLoads&lt;/key>
	&lt;true/>
&lt;/dict>
```

#### 3 在info.plist中加入以下节点，用来获取权限（如使用AdColony联盟必须加，未使用AdColony联盟可以不添加）
```objective-c
&lt;key>NSCalendarsUsageDescription&lt;/key>
&lt;string>Some ad content may create a calendar event.&lt;/string>
&lt;key>NSCameraUsageDescription&lt;/key>
&lt;string>Some ad content may access camera to take picture.&lt;/string>
&lt;key>NSPhotoLibraryUsageDescription&lt;/key>
&lt;string>Some ad content may require access to the photo library.&lt;/string>
```

注：根据不同语言和不同使用场景，使用者可以适当调整获取权限的描述文字。
<br>

#### 4 Bitcode
支持Bitcode，请使用者根据需求选择是否使用Bitcode（Domob不支持Bitcode）

#### 5 cocos项目配置
如果您使用的是cocos创建的项目，请检查项目的`Version`是否已经填写，如下图：
（因为cocos创建出来的项目Version默认是没有填写的，如不填写会影响后续使用）

![cocos项目Version字段配置](http://docs.upltv.com/uploads/201709/59afb01ec7612_59afb01e.png "cocos项目Version字段配置")
<br>

### 6 查看版本号
在*AvidlyAdsSDKVersion.h*文件中，可以直接查看当前SDK的版本号。

```objective-c
//sdk版本号
#define AvidlyAdsSDKVERSION  @"3003"
```
> AvidlyAdsSDKVERSION，3003表示当前的版本号数字编号。

