## 手动下载SDK接入


### **下载SDK包**
首先从[IOS-SDK](http://docs.avidly.com/docs/show/67 "IOS-SDK")下载页 下载最新的 SDK 包，解压后的目录包含如下两个文件：

- PlayableAdsSDK.framework
- PlayableAdsSDK.bundle

PlayableAdsSDK.framework是SDK的静态库，PlayableAdsSDK.bundle主要包含一些静态库需要访问的外部文件资源，如图片资源。 
<br><br>
### **Xcode工程添加SDK包**
请将PlayableAdsSDK.framework与PlayableAdsSDK.bundle两个文件同时添加到你的Xcode工程目录下：<br>
![](http://docs.avidly.com/uploads/201707/59686429e44b7_59686429.png)
<br><br>
### **加入系统依赖库**

在TARGETS → Build Phases → Link Binary With Libraries中添加依赖库：<br>
- <font color=#DC143C>`libsqlite3.tbd `</font><br>
- <font color=#DC143C>`libz.tbd `</font><br>
- <font color=#DC143C>`AVFoundation.framework `</font> <br>
- <font color=#DC143C>`SystemConfiguration.framework `</font><br>
- <font color=#DC143C>`StoreKit.framework `</font><br>
- <font color=#DC143C>`CoreTelephony.framework `</font><br>
- <font color=#DC143C>`MobileCoreServices.framework `</font><br>
- <font color=#DC143C>`CoreMedia.framework `</font><br>
- <font color=#DC143C>`AdSupport.framework `</font><br>
<br>

![](http://cnimg.dataverse.cn.s3.cn-north-1.amazonaws.com.cn/playable-ads-demo/playable-0013.png)
<br><br>
### **工程配置**
#### **1 添加分类编译符**
在<font color=#DC143C>`TARGETS `</font> → <font color=#DC143C>`Build Settings `</font> → <font color=#DC143C>`Linking `</font> → <font color=#DC143C>`Other Linker Flags `</font> 中加入 <font color=#DC143C>`-ObjC `</font>、<font color=#DC143C>`-fobjc-arc `</font>、<font color=#DC143C>`-all_load `</font> 如下图所示：<br><br>
![](http://cnimg.dataverse.cn.s3.cn-north-1.amazonaws.com.cn/playable-ads-demo/playable-0012.png)
<br>
#### **2 在info.plist中加入以下节点，以兼容http模式**<br>

	<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSAllowsArbitraryLoads</key>
		<true/>
	</dict>

<br>

#### **3 Bitcode**
支持Bitcode，请使用者根据需求选择是否使用Bitcode。
<br>
### **查看版本号**
在PlaybleAdsTools.h文件中，调用以下方法可获取当前SDK的版本号。
```objective-c
/*
 * 获取版本号
 */
+ (NSString *_Nullable)getVersion;
```
<br><br>
