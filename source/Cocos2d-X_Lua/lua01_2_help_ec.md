## Android Eclipse接入帮助


### 一、UPSDK LuaPlugin的目录结构
针对Eclipse构建的工程，UPSDK以`*.jar`的方式导入到工程中。UPSDK LuaPlugin版本的下载包( [Android-LuaSDK下载](http://docc.upltv.com/docs/show/13 "SDK下载页面"))解压后的目录结构如下：


![ec-1-1](http://docc.upltv.com/uploads/201805/5afe9bd143673_5afe9bd1.png "ec-1-1")
- `Eclipse`
  此目录主要包含Eclipse工程接入所需的广告依赖库文件。
- `lua`文件夹
  此目录主要主要包含Demo源码文件。
- `Android Studio`文件夹
  Eclipse工程请忽略此目录。
- `proguard-project.txt`文件
  混淆配置文件，如果当前工程开启混淆功能，请将此文件中的混淆配置添加到工程混淆所依赖的文件中。
  

### 二、导入UPSDK主包
#### 1.添加UPSDK文件
以3.0.03版本为例，在`upsdk_ads`目录下，有`libs`，`res`以及`assets`三个目录，请分别将目录下的内容复制到您Eclipse主工程的相应的目录下(如果没有libs目录，则在app文件夹下和src同级的目录中创建libs)。
> 对于非*.so文件，复制时出现重名文件时，请慎重操作：**仅在重名的文件是AvidlyAdsSdk原有的文件时可以复盖操作。**非此情况下的文件重名冲突时，可以根据经验自行酌情解决(比如res/values/目录下的文件可以重命名)，当然也可以直接与我们的技术反馈问题寻求解决方案。

效果图如下：

![ec-2-1](http://docc.upltv.com/uploads/201805/5afe9cf8d18fd_5afe9cf8.png "ec-2-1")
> `UPAdsSdk_Lua_3.0.03.jar`仅作为示例参考说明

#### 2.检查Manifest中minSdkVersion与targetSdkVersion
由于UPSDK中部分方法和广告联盟仅支持sdk 15到sdk 25的设备，所以需要修改项目对应编译版本和最小支持版本。打开项目的manifest文件，设置minSdkVersion和targetSdkVersion，如下：
```groovy
 < uses-sdk android:minSdkVersion="16" android:targetSdkVersion="25" />
```
#### 3.确保*.so文件对齐
`avidly_ads`文件夹下的`libs`目录中提供了如下三种cpu类型的so文件：
- armeabi
- armeabi-v7a
- x86

根据您的需要，只要在libs目录下删除当前不需要支持的CPU架构即可。

### 三、添加广告联盟
#### 1.添加Google Ads SDK
google广告联盟是主要的盈利来源，必须接入到项目中，请参考**添加UPSDK文件**的方式将其添加到您的工程中，效果图如下：

![ec-3-1](http://docc.upltv.com/uploads/201805/5afea0b542f2f_5afea0b5.png "ec-3-1")
> 特别地，如果您的工程中已经 n存在与UPSDK所依赖的gms play不同版时，请用更高的版本替换低版本

#### 2.添加可选广告联盟
为了能为您获得最大收益，建议将这些广告联盟需要的组件包都添加到您的项目中。文件夹 `optional_ads` 中是以`*.jar`形式存在的广告联盟依赖文件。请参考**添加UPSDK文件**的方式将其添加到您的工程中，同时AndroidManifest文件中的内容复制到您项目的Manifest文件中。

### 四、加入 Android Support 支持库
广告的正常展示需要 `support` 库的支持，所以请将其引入到您的项目中。命名为`android_support_library`的文件夹中是需要依赖的文件。请将其中*.jar文件依次加入到项目的libs目录中，目录格式应该如下：

![ec-4-1](http://docc.upltv.com/uploads/201805/5afea104016ef_5afea104.png "ec-4-1")

### 五、修改 AndroidManifest.xml 文件
将下载文件夹中的 `AndroidManifest.xml  `文件中的内容复制加入到您项目的对应节点中。

### 六、修改.mk文件

### 1、修改jni目录中的application.mk
打开在当前工程jni目录下的application.mk文件，添加如下信息:
```groovy
APP_SHORT_COMMANDS := true
NDK_TOOLCHAIN_VERSION=4.9
APP_PLATFORM=android-14
```

### 七、修改 Proguard
如果您的项目开启了混淆功能，请将 `proguard-project.txt` 文件中的内容复制粘贴到当前项目使用的 混淆配置文件中，否则会因为混淆的原因导致程序无法找到类文件出现崩溃异常。

### 八、Demo工程
为帮助您更好的了解广告SDK的接入以及使用，我们在这里提供了一个简单的[Demo工程](https://github.com/AvidlyGit/AdSdkDemo-Studio "Demo工程")。
