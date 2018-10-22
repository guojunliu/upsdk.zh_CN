
## Eclipse 接入帮助

### 一、UPSDK CppPlugin的目录结构
针对Eclipse构建的工程，UPSDK以`*.jar`的方式导入到工程中。UPSDK CppPlugin版本的下载包( [Android-CPPSDK下载](http://docc.upltv.com/docs/show/13 "SDK下载页面"))解压后的目录结构如下：

![ec-1-1](http://docc.upltv.com/uploads/201805/5afd3722e5ab6_5afd3722.png "ec-1-1")


- `Eclipse`
  此目录主要包含Eclipse工程接入所需的广告依赖库文件。
- `cpp`文件夹
  此目录主要包含一些*.cpp源码文件，用于桥接当前Cocos2d-X cpp工程与UPSDK广告接口调用。
- `Android Studio`文件夹
  Eclipse工程请忽略此目录。
- `proguard-project.txt`文件
  混淆配置文件，如果当前工程开启混淆功能，请将此文件中的混淆配置添加到工程混淆所依赖的文件中。
  

### 二、导入UPSDK主包
#### 1.添加UPSDK文件
以3.0.03版本为例，在`upsdk_ads`目录下，有`libs`，`res`以及`assets`三个目录，请分别将目录下的内容复制到您Eclipse主工程的相应的目录下(如果没有libs目录，则在app文件夹下和src同级的目录中创建libs)。
> 对于非*.so文件，复制时出现重名文件时，请慎重操作：**仅在重名的文件是AvidlyAdsSdk原有的文件时可以复盖操作。**非此情况下的文件重名冲突时，可以根据经验自行酌情解决(比如res/values/目录下的文件可以重命名)，当然也可以直接与我们的技术反馈问题寻求解决方案。

效果图如下：

![ec-2-1](http://docc.upltv.com/uploads/201805/5afd39e93c234_5afd39e9.png "ec-2-1")
> `UPAdsSdk_Cpp_3.0.03.jar`仅作为示例参考说明

#### 2.检查Manifest中minSdkVersion与targetSdkVersion
由于UPSDK中部分方法和广告联盟仅支持sdk 15到sdk 26的设备，所以需要修改项目对应编译版本和最小支持版本。打开项目的manifest文件，设置minSdkVersion和targetSdkVersion，如下：
```groovy
 < uses-sdk android:minSdkVersion="16" android:targetSdkVersion="24" />
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

![ec-3-1](http://docc.upltv.com/uploads/201805/5afd45a3b7d61_5afd45a3.png "ec-3-1")
> 特别地，如果您的工程中已经存在与UPSDK所依赖的gms play不同版时，请用更高的版本替换低版本

#### 2.添加可选广告联盟
为了能为您获得最大收益，建议将这些广告联盟需要的组件包都添加到您的项目中。文件夹 `optional_ads` 中是以`*.jar`形式存在的广告联盟依赖文件。请参考**添加UPSDK文件**的方式将其添加到您的工程中，同时AndroidManifest文件中的内容复制到您项目的Manifest文件中。

### 四、加入 Android Support 支持库
广告的正常展示需要 `support` 库的支持，所以请将其引入到您的项目中。命名为`android_support_library`的文件夹中是需要依赖的文件。请将其中*.jar文件依次加入到项目的libs目录中，目录格式应该如下：

![ec-4-1](http://docc.upltv.com/uploads/201805/5afd4483c57c1_5afd4483.png "ec-4-1")


### 五、修改 AndroidManifest.xml 文件
将下载文件夹中的 `AndroidManifest.xml  `文件中的内容复制加入到您项目的对应节点中。

### 六、添加*.cpp文件
#### 1.添加*.cpp文件到当前项目
当前Cocos工程是通过*cpp源文件实现与UPSDK的接口跨平台调用，因此必须将`cpp/upltv`中所有的*.cpp和*.h文件，复制到当前工程中。


Cocos2d-x 3.16的版本可以复制到Classes文件夹中，其它版本若有差异请参考修改，效果如下：

![classes](http://docc.upltv.com/uploads/201804/5acac9170fddc_5acac917.png "classes")
>本文基于cocos2dx-3.16进行编写，如果您的目录与此不符可联系我们的支持人员获得帮助。

### 2、修改jni目录中的android.mk
打开在当前工程jni目录下的android.mk文件，将**UpltvAndroid.cpp，CocosUpLtv.cpp，UpltvBridge.cpp**三个文件添加到**LOCAL_SRC_FILES**路径中。
以Cocos2d-X 3.16为例，添加方式如下，其它版本请酌情修改相对路径:

```groovy
LOCAL_SRC_FILES := hellocpp/main.cpp \
                   ../../Classes/AppDelegate.cpp \
                   ../../Classes/upltv/UpltvAndroid.cpp \
                   ../../Classes/upltv/CocosUpLtv.cpp \
                   ../../Classes/upltv/UpltvBridge.cpp \
                   ../../Classes/HelloWorldScene.cpp
```

### 七、修改 Proguard
如果您的项目开启了混淆功能，请将 `proguard-project.txt` 文件中的内容复制粘贴到当前项目使用的 混淆配置文件中，否则会因为混淆的原因导致程序无法找到类文件出现崩溃异常。

### 八、Demo工程
为帮助您更好的了解广告SDK的接入以及使用，我们在这里提供了一个简单的[Demo工程](https://github.com/AvidlyGit/AdSdkDemo-Studio "Demo工程")。
