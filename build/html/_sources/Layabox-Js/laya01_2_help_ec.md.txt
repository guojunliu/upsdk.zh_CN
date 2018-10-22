## Android Eclipse接入帮助

### 一、UPSDK JavaScriptPlugin的目录结构
针对Eclipse构建的工程，UPSDK以`*.jar`的方式导入到工程中。UPSDK JavaScriptPlugin版本的下载包( [Android-LayaboxJsSDK下载](http://docc.upltv.com/docs/show/13 "SDK下载页面"))解压后的目录结构如下：

![](http://docc.upltv.com/uploads/201809/5b98f4a1c89a1_5b98f4a1.png)

- `Eclipse`
	此目录主要包含Eclipse工程接入所需的广告依赖库文件。
- `js`文件夹
	此目录主要包含一些*.js源码文件，用于桥接当前`Layabox Js`工程与`UPSDK`广告接口调用。
- `Android Studio`文件夹
	Eclipse工程请忽略此目录。
- `proguard-project.txt`文件
	混淆配置文件，如果当前工程开启混淆功能，请将此文件中的混淆配置添加到工程混淆所依赖的文件中。
	

### 二、导入UPSDK主包
#### 1.添加UPSDK文件
以3.0.05版本为例，在`upsdk_ads`目录下，有`libs`，`res`以及`assets`三个目录，请分别将目录下的内容复制到您Eclipse主工程的相应的目录下(如果没有libs目录，则在app文件夹下和src同级的目录中创建libs)。
> 对于非*.so文件，复制时出现重名文件时，请慎重操作：**仅在重名的文件是AvidlyAdsSdk原有的文件时可以复盖操作。**非此情况下的文件重名冲突时，可以根据经验自行酌情解决(比如res/values/目录下的文件可以重命名)，当然也可以直接与我们的技术反馈问题寻求解决方案。

效果图如下：

![](http://docc.upltv.com/uploads/201809/5b98f614c67d9_5b98f614.png)

> `UPAdsSdk_LayaJs_3.0.05.jar`仅作为示例参考说明

#### 2.检查Manifest中minSdkVersion与targetSdkVersion
由于UPSDK中部分方法和广告联盟仅支持sdk 15到sdk 25的设备，所以需要修改项目对应编译版本和最小支持版本。打开项目的manifest文件，设置minSdkVersion和targetSdkVersion，如下：
```groovy
 < uses-sdk android:minSdkVersion="16" android:targetSdkVersion="25" />
```
#### 3.确保*.so文件对齐
`upsdk_ads`文件夹下的`libs`目录中提供了如下三种cpu类型的so文件：
- armeabi
- armeabi-v7a
- x86

根据您的需要，只要在libs目录下删除当前不需要支持的CPU架构即可。

### 三、添加广告联盟
#### 1.添加Google Ads SDK
google广告联盟是主要的盈利来源，必须接入到项目中，请参考**添加UPSDK文件**的方式将其添加到您的工程中，效果图如下：

![](http://docc.upltv.com/uploads/201809/5b98f679af237_5b98f679.png)

> 特别地，如果您的工程中已经存在与UPSDK所依赖的gms play不同版时，请用更高的版本替换低版本

#### 2.添加可选广告联盟
为了能为您获得最大收益，建议将这些广告联盟需要的组件包都添加到您的项目中。文件夹 `optional_ads` 中是以`*.jar`形式存在的广告联盟依赖文件。请参考**添加UPSDK文件**的方式将其添加到您的工程中，同时AndroidManifest文件中的内容复制到您项目的Manifest文件中。

### 四、加入 Android Support 支持库
广告的正常展示需要 `support` 库的支持，所以请将其引入到您的项目中。命名为`android_support_library`的文件夹中是需要依赖的文件。请将其中*.jar文件依次加入到项目的libs目录中，目录格式应该如下：

![](http://docc.upltv.com/uploads/201809/5b98f6edd1b61_5b98f6ed.png)

### 五、修改 AndroidManifest.xml 文件
将下载文件夹中的 `AndroidManifest.xml  `文件中的内容复制加入到您项目的对应节点中。

### 六、添加*.Js文件
#### 添加*.Js文件到当前项目

当前Layabox工程是通过*.js源文件实现与UPSDK的接口跨平台调用，因此必须将`js/src/upltv`中所有的*.js文件复制到当前工程中。

1)、将`upltv`拷贝到Layabox工程的src文件夹下，只留.js文件。

2)、在Layabox工程的bin文件夹下的index.html中加入`UPLTV.js`,`UPLTVIos.js`的script引用。

如下图所示

![](http://docc.upltv.com/uploads/201809/5b98f2c8af661_5b98f2c8.png)

### 七、修改 Proguard
如果您的项目开启了混淆功能，请将 `proguard-project.txt` 文件中的内容复制粘贴到当前项目使用的 混淆配置文件中，否则会因为混淆的原因导致程序无法找到类文件出现崩溃异常。

