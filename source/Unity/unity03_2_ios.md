## IOS 接入帮助
### 一、声明
我们在对Unity 5.x不同版本测试时发现，只有5.1.0以上的版本在Mac平台才能正常运行，因此5.1.0以下版本未能完成适配测试。若你的版本低于5.1.0，建议你升级到更高的版本；如果因故不能升级，可以偿试导入，若有问题可及时向我们的技术人员寻求帮助。

### 二、检查Unity工程Assets目录IOS Frameworks
Unity插件IOS版本被成功导入后，在Assets目录下，检查`PolyADSDK/Plugins/IOS/frameworks`以及`PolyADSDK/Plugins/IOS/resources`两个目录是否存在。如果不存在，说明插件没有正确导入。

成功导入IOS插件后，3003以下的版本，Assets中目录结构如下：

![bbb](http://docc.upltv.com/uploads/201805/5af5744506897_5af57445.png "bbb")

3003及以后的版本，Assets中目录结构如下：

![aaa](http://docc.upltv.com/uploads/201805/5af573a34f61b_5af573a3.png "aaa")

为了保证UPSDK的目录资源的健全，请检查以下几方面的文件是否存在。

#### 1. UPSDK 主包
`PolyADSDK/Plugins/IOS/frameworks/UPSDK.framework`文件是UPSDK 主包的主包文件，如果缺少此文件，或许下载的插件包不对，请尽快与我们的技术支持人员联系，寻求帮助。
> 版本3003开始，原AvidlyAdsSDK.framework改名为UPSDK.framework，相应的bundle文件也改成UPSDK.bundle

#### 2. UPSDK 联盟商依赖库
在`PolyADSDK/Plugins/IOS/frameworks`目录下，除了UPSDK 主包文件外，还会有一系列其它的广告联盟商的依赖库，他们有些以*.framework的形式存在 ，如AdCoony.framework,UnityAds.framework；有些是*.a或*.h等文件形式存在，如OneWaySDK, Domob-SDK。

#### 3. UPSDK 资源包
`PolyADSDK/Plugins/IOS/resources`是UPSDK广告需要依赖的资源文件，如激励广告UI交互的一些图标资源。如果这些资源缺少，会导致运行不正常，影响广告收益。

#### 4. XUPorter
XUPorter是一个开源库，用于Xcode导出时动态修改工程的配置参数，包括添加依赖包与资源文件。如果缺少这个目录的文件，会导致Xcode导出时，无法自动完成工程的适配工作。当然，这时也可以手动导入资源，详细过程在后继内容中会讲解。


### Xcode 配置检查
如果导出的Xcode工程编译出错时，怎么办？请按下面步骤检查所有的配置参数是否正确。

#### 1. Deployent Target设置
在Target->General面板中，设置Deployent Target在7.0或以上，部分第三方依赖的framework对target有要求在7.0或以上。

示例如下：
![ios-target](http://docc.upltv.com/uploads/201706/59535d81cf339_59535d81.png "ios-target")

#### 2. Link Binary with Libraries检查
在Build Phases -> Link Binary with Libraries中检查是否添加了插件中包含的framework库以及.a文件。

正常添加后会增加以下红框中的library文件。
![ios-lib](http://docc.upltv.com/uploads/201706/595361be1b87e_595361be.png "ios-lib")

如果没有发现`PolyADSDK/Plugins/IOS/frameworks`中所包括的库文件，可以通过Xcode工程手动将`PolyADSDK/Plugins/IOS/frameworks`所包括的文件添加到工程中。

**其中：
`libsqlite3.tbd`，`libxml2.tbd`，`libz.tbd`，`libstdc++.tbd`，`WebKit.framework`与`JavaScriptCore.framework`是系统库，必须添加，因为部分第三方广告的表态库对此有依赖。如果发现缺失请手工添加这些系统库。**

#### 3. bundle resources检查
在Build Phases -> Copy Bundle Resources 中检查是否添加了插件中包含的resources的资源。

正常添加后会增加以下红框中的资源文件文件。
![ios-resources](http://docc.upltv.com/uploads/201706/59536339b4e91_59536339.png "ios-resources")

如果没有发现`PolyADSDK/Plugins/IOS/resources`中所包括的资源，可以通过Xcode工程手动将`PolyADSDK/Plugins/IOS/resources`资源添加到工程中。

#### 4. Other Linker Flag检查
在Build Settings -> Linking -> Other Linker Flags 中检查是否添加了`-ObjC`以及`-fobjc-arc`的配置。

正常添加后会增加以下红框中的配置参数，如果没有出现可以手动添加。
![ios-link-set](http://docc.upltv.com/uploads/201706/59536443253fc_59536443.png "ios-link-set")

#### 5. Unity个别版本不兼容的处理

1、对于5.4.2~5.4.0的版本，如果引入的Framework不正常（标红），请手工将引入以下目录的代码库与资源添加到工程中：
`PolyADSDK/Plugins/IOS/frameworks`
 `PolyADSDK/Plugins/IOS/resources`
 
 2、对于5.3.1等部分版本il2cpp-config.h出现noreturn定义错误时，请参考如下修改：
```cpp
 #define NORETURN __declspec(noreturn)
替换为
#define NORETURN __attribute__((noreturn))
```
3、对于5.1.4等版本出现 **does not contain bitcode** 错误时，请将 Enable Bitcode 设置为NO

#### 6. Clean工程，重新编译
做完上述检查后 ，请在Product->Clean之后，再编译！！！

如果错误仍然存在，请与我们的技术支持联系，告诉他们错误信息（截图+日志），寻求帮助。