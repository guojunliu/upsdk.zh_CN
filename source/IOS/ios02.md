## CocoaPods 接入帮助

如果您使用CocoaPods来管理iOS项目的依赖关系，请按照以下步骤导入UPSDK：

如果您是第一次使用CocoaPods接入此SDK，首先通过使用以下命令将 Avidly Repo 添加到CocoaPods中：

```
pod repo add Avidly https://github.com/guojunliu/AvidlyAdsSDK-SpecsRepo.git
```

如果您是更新此SDK，请使用以下代码更新 Avidly Repo

```
pod repo update Avidly
```

然后在您的项目中的Podfile文件添加依赖

```
platform :ios, '8.0'
use_frameworks!

target ‘YourAppName’ do
  source 'https://github.com/guojunliu/AvidlyAdsSDK-SpecsRepo.git'
  pod 'UPSDK', '~> 3.0.03'
end
```

其中`~> 3.0.03`表示要下载的版本，请选择需要的版本（`建议使用最新版本`）

然后进入您的工程目录，运行以下命令

```
pod install
```

完成


### 工程配置
  
1 在info.plist中加入以下节点，以兼容http模式

```objective-c
&lt;key>NSAppTransportSecurity&lt;/key>
&lt;dict>
	&lt;key>NSAllowsArbitraryLoads&lt;/key>
	&lt;true/>
&lt;/dict>
```

2 在info.plist中加入以下节点，用来获取权限（如使用AdColony联盟必须加，未使用AdColony联盟可以不添加）

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

3 Bitcode

支持Bitcode，请使用者根据需求选择是否使用Bitcode（Domob和Youlan不支持Bitcode）
<br>

### 查看版本号
在*UPSDKVersion.h*文件中，可以直接查看当前SDK的版本号。

```objective-c
//sdk版本号
#define UPSDKVERSION  @"3003"
```
> UPSDKVERSION，3003表示当前的版本号数字编号。