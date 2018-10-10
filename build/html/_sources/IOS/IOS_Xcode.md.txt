## XCode
### 下载UPSDK包
首先从 [IOS-SDK下载页](http://ads-sdk-doc.haloapps.com/docs/show/13 "SDK下载页面") 下载最新的 SDK 包，解压后的目录包含如下两个文件：

- UPSDK.framework
- UPSDK.bundle

UPSDK.framework是SDK的静态库，UPSDK.bundle主要包含一些静态库需要访问的外部文件资源，如图片资源。
</br>

### Xcode工程添加SDK包
请将UPSDK.framework与UPSDK.bundle两个文件同时添加到你的Xcode工程目录下，比如FrameWork目录：

![SDK包成功引入后的效果](http://docs.upltv.com/uploads/201805/5af568523c6de_5af56852.png "SDK包成功引入后的效果")
</br>

### 加入第三方联盟广告依赖库
UPSDK需要依赖第三广告联盟才能使用，所以需要手动将第三方联盟的依赖的库文件及其配套资源导入到你的项目中。为保证收益最大化，建议尽可能多地添加第三方联盟，为保证收益至少添加我们给出的联盟列表。

为了保证你能正确添加第三方依赖包，请从这里下载最新的联盟包[UPSDK第三方包下载](http://ads-sdk-doc.haloapps.com/docs/show/13 "SDK第三方包下载") 。

当前UPSDK依赖的部分重要广告联盟列表如下：

![添加所有第三方SDK包](http://docs.upltv.com/uploads/201805/5af569379d9bb_5af56937.png "添加所有第三方SDK包")

- **第三方依赖库为可选库，请根据我们的要求选择添加，如不清楚可联系我们**


</br>

### 加入系统依赖库

在TARGETS → General → Linked Frameworks Libraries中添加依赖库

- `QuartzCore.framework`
- `MediaPlayer.framework`
- `libsqlite3.tbd`
- `libz.tbd`
- `CoreMedia.framework`
- `CoreGraphics.framework`
- `CFNetwork.framework`
- `WebKit.framework (Optional)`
- `WatchConnectivity.framework (Optional)`
- `SystemConfiguration.framework`
- `StoreKit.framework`
- `Social.framework`
- `MessageUI.framework`
- `JavaScriptCore.framework (Optional)`
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

如果您使用的是cocos，请额外添加一个依赖库

- `GameController.framework`
<br>

### 工程配置
#### 1 添加分类编译符

- 在`TARGETS` → `Build Setting` → `Linking` → `Other Linker Flags` 中加入 `-ObjC`和`-fobjc-arc` 如下图所示

![分类编译符](http://docs.upltv.com/uploads/201709/59afb0af0f8bd_59afb0af.png "分类编译符")
  
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
支持Bitcode，请使用者根据需求选择是否使用Bitcode（Domob和Youlan不支持Bitcode）

#### 5 cocos项目配置
如果您使用的是cocos创建的项目，请检查项目的`Version`是否已经填写，如下图：
（因为cocos创建出来的项目Version默认是没有填写的，如不填写会影响后续使用）

![cocos项目Version字段配置](http://docs.upltv.com/uploads/201709/59afb01ec7612_59afb01e.png "cocos项目Version字段配置")
<br>

### 查看版本号
在*UPSDKVersion.h*文件中，可以直接查看当前SDK的版本号。

```objective-c
//sdk版本号
#define UPSDKVERSION  @"3003"
```
> UPSDKVERSION，3003表示当前的版本号数字编号。
