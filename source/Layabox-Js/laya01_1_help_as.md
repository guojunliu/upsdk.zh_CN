## Android Studio接入帮助

### 一、UPSDK JavaScriptPlugin的目录结构
针对Android Studio 或 Gradle 构建的工程，UPSDK提倡以`*.aar`的方式导入到项目中。UPSDK JavaScriptPlugin版本的下载包( [Android-LayaboxJsSDK](http://docc.upltv.com/docs/show/13 "SDK下载页面") 下载UPSDK JavaScriptPlugin包)解压后的目录结构如下：

![](http://docc.upltv.com/uploads/201809/5b98ed83ade86_5b98ed83.png)

> 如上图所示，UPSDK JavaScriptPlugin主包命名为`UPAdsSdk_LayaJs_x.x.xx_dex.aar`。

- `Android Studio`
	此目录主要包含Android Studio工程接入所需的广告依赖库文件。
- `js`
	此目录主要包含一些*.js源码文件，用于桥接当前`Layabox Js`工程与`UPSDK`广告接口调用。
- `Eclipse`
	此目录包含一些jar及资源文件，Android Studio工程请忽略此目录。
- `proguard-project.txt`
	混淆配置文件，如果当前工程开启混淆功能，请将此文件中的混淆配置添加到工程混淆所依赖的文件中
	

### 二、导入UPSDK主包
#### 1.添加UPSDK文件
根据上文的介绍，在您下载好的文件目录中找到名为`UPAdsSdk_LayaJs_x.x.xx_dex.aar`的文件，并添加到项目的`libs`目录下（注:如果没有libs目录，则在app文件夹下和src同级的目录中创建libs）。
添加后，工程的效果图如下所示：

![](http://docc.upltv.com/uploads/201809/5b98f0d13705b_5b98f0d1.png)
> `UPAdsSdk_LayaJs_3.0.05_dex`仅作为示例参考说明

为了确保libs目录的aar包能正确被工程引用，请检查`app`目录下的`build.gradle`文件中，是否添加以下配置参数：

```groovy
repositories {
        flatDir {
             dirs 'libs'
        }
}

```
最后将aar添加到dependencies域中


```groovy
dependencies {
// 请将UPAdsSdk_LayaJs_3.0.05_dex替换成实际的文件名
compile(name: 'UPAdsSdk_LayaJs_3.0.05_dex', ext: 'aar')
}
```

#### 2.检查`build.gradle`中的minSdkVersion与targetSdkVersion
UPSDK要求minSdkVersion不能低于15，targetSdkVersion不能高于26，因此请检查`build.gradle`文件中相应的配置是否正确。


推荐的配置如下：

```groovy
 defaultConfig {
        minSdkVersion 16
        targetSdkVersion 26
}
```

#### 3.确保*.so文件对齐
`UPAdsSdk_LayaJs_x.x.xx_dex.aar`中，提供了如下三种cpu类型的so文件：
- armeabi
- armeabi-v7a
- x86

请根据当前工程支持的cup类型，在`gradle.build`文件中添加abiFilters配置，确保so对齐。

如果当前工程仅支持armeabi-v7a，请参考如下修改：

```
defaultConfig {
        ndk {
            abiFilters 'armeabi-v7a'
        }
 }
```

>abiFilters 后面添加的CPU架构，为当前项目实际支持的架构。

### 三、添加广告联盟
对于海外或全球区域发布的产品，Google的Admob广告联盟是主要的盈利来源之一，因此我们强烈建议添加到项目中。

#### 1.添加Google Ads SDK
Admob广告联盟的接入，我们提供两种方式，在网络允许的条件下推荐使用 **【方案一】** 来进行接入。

###### 1.**【方案一】**
在`build.gradle`文件中，通过compile命令从Google的远程仓库下载gms play-service16.0.0的包。
    
	dependencies {
        compile 'com.google.android.gms:play-services-ads:16.0.0'
    }


###### 2.**【方案二】**
如果您的网络不佳，无法从Google的远程仓库下载play-service包，我们也提供了另一种方案作为替代。

首先请将`Android Studio/aar/admob-gms-play-services`目录下aar文件全部复制到工程的`libs`目录下。


参考如下方式修改`build.gradle`文件，将Admob依赖的aar包添加到工程。
```groovy
dependencies {
    compile(name: 'play-services-ads-16.0.0', ext: 'aar')
    compile(name: 'play-services-ads-base-16.0.0', ext: 'aar')
    compile(name: 'play-services-ads-identifier-16.0.0', ext: 'aar')
    compile(name: 'play-services-ads-lite-16.0.0', ext: 'aar')
    compile(name: 'play-services-basement-16.0.0', ext: 'aar')
    compile(name: 'play-services-gass-16.0.0', ext: 'aar')
   }
```

> 特别地，如果您的工程中已经存在与UPSDK所依赖的gms play不同版时，请用更高的版本替换低版本

#### 2.添加其他广告联盟
为了保证您能获得更大收益，请将广告联盟包尽可能多地将其它联盟广告的aar文件添加到您的项目中。
请参考以下方式将`Android Studio/aar`中命名为 `xxx_ads.aar` 的文件添加到当前工程中。
###### 海外区域
我们建议但局限于添加以下联盟的aar文件。
对应的build.gradle如下：
```groovy
dependencies {
    //other ads-libs
    //gson-2.7.jar在android_support_library目录中
    compile(name: 'gson-2.7', ext: 'jar')
    compile(name: 'facebook_ads', ext: 'aar')
    compile(name: 'facebook_exo_player', ext: 'aar')

    compile(name: 'mobvista_ads', ext: 'aar')
    compile(name: 'unity_ads', ext: 'aar')
    compile(name: 'vungle_ads', ext: 'aar')
    compile(name: 'chartboost_ads', ext: 'aar')
    compile(name: 'ironsource_ads', ext: 'aar')
    
    compile(name: 'adcolony_ads', ext: 'aar')
    compile(name: 'applovin_ads', ext: 'aar')
    compile(name: 'playable_ads', ext: 'aar')
    compile(name: 'tapjoy_ads', ext: 'aar')
}
```

###### 中国大陆区域
我们建议但局限于添加以下联盟的aar文件。
对应的build.gradle如下：
```groovy
dependencies {
    //other ads-libs
    //gson-2.7.jar在android_support_library目录中
    compile(name: 'gson-2.7', ext: 'jar')
   compile(name: 'centrixlink_ads', ext: 'aar')

    compile(name: 'mobvista_ads', ext: 'aar')
    compile(name: 'vungle_ads', ext: 'aar')
    compile(name: 'chartboost_ads', ext: 'aar')
    
    compile(name: 'inmobi_ads', ext: 'aar')
    compile(name: 'oneway_ads', ext: 'aar')
    compile(name: 'playable_ads', ext: 'aar')
}
```

###### 全球区域
我们建议请将上述两个区域的aar合并到build.gradle中。

### 四、加入 Android Support 支持库
UPSDK及其广告联盟，需要依赖Android Support 库的支持，所以请根据以下要求正确添加Android Support 支持库。

UPSDK3004开始，因为Admob等联盟升级的原因，我们推荐使用support:xxxx:26.1.0的支持库（含v4与v7)，当然在某种不可抗拒的限制下，也可能使用更低的版本如support:xxxx:25.3.1的版本。

为了方便您将Android Support 库引入到项目中，我们依旧提供两种引入方式，在网络请允许的情况下，建议参考 **【方案一】**。

#### 1.**【方案一】**
从Google仓库远程更新下载support:xxxx:26.1.0，不需要将 `Android Studio/aar/android_support_library`中的aar文件拷贝到libs目录下。
对应的build.gradle如下：

```groovy
dependencies { 
    //core lib
    compile(name: 'UPAdsSdk_LayaJs_3.0.05_dex', ext: 'aar')
    //support libs
    compile 'com.android.support:recyclerview-v7:26.1.0'
    compile 'com.android.support:support-v4:26.1.0'
    compile 'com.android.support:appcompat-v7:26.1.0'
    compile 'com.android.support:support-annotations:26.1.0'
    compile  'com.android.support:customtabs:26.1.0'
    compile 'com.android.support:cardview-v7:26.1.0'
}

```

#### 2.**【方案二】**
如果您的网络不佳，请求不到远程仓库，可以从libs中加载aar文件。
首先将`Android Studio/aar/android_support_library` 中的文件全部拷贝到当前工程的libs目录下，然后参考如下方法修改`build.gradle`文件。

```groovy
dependencies {
    //core lib
    compile(name: 'UPAdsSdk_LayaJs_3.0.05_dex', ext: 'aar')
    //support libs
    compile(name: 'animated-vector-drawable-26.1.0', ext: 'aar')
    compile(name: 'appcompat-v7-26.1.0', ext: 'aar')
    compile(name: 'customtabs-26.1.0', ext: 'aar')
    compile(name: 'runtime-1.0.0', ext: 'aar')
    compile(name: 'support-compat-26.1.0', ext: 'aar')
    compile(name: 'support-core-ui-26.1.0', ext: 'aar')
    compile(name: 'support-core-utils-26.1.0', ext: 'aar')
    compile(name: 'support-fragment-26.1.0', ext: 'aar')
    compile(name: 'support-media-compat-26.1.0', ext: 'aar')
    compile(name: 'support-v4-26.1.0', ext: 'aar')
    compile(name: 'support-vector-drawable-26.1.0', ext: 'aar')
    compile(name: 'common-1.0.0', ext: 'jar')
    compile(name: 'supprot-common-1.0.0', ext: 'jar')
    compile(name: 'support-annotations-26.1.0', ext: 'jar')
    compile(name: 'recyclerview-v7-26.1.0', ext: 'aar')
}
```

**【注意】Android Support 支持库为强制依赖，请务必将其引入您的项目，且两个方案只能选择其一！**

### 五、添加*.Js文件
#### 添加*.Js文件到当前项目

当前Layabox工程是通过*.js源文件实现与UPSDK的接口跨平台调用，因此必须将`js/src/upltv`中所有的*.js文件复制到当前工程中。

1)、将`upltv`拷贝到Layabox工程的src文件夹下，只留.js文件。

2)、在Layabox工程的bin文件夹下的index.html中加入`UPLTV.js`,`UPLTVIos.js`的script引用。

如下图所示

![](http://docc.upltv.com/uploads/201809/5b98f2c8af661_5b98f2c8.png)

### 六、修改 Proguard
如果您的项目开启了混淆功能，请将 `proguard-project.txt` 文件中的内容复制粘贴到当前项目使用的 混淆配置文件中，否则会因为混淆的原因导致程序无法找到类文件出现崩溃异常。

### 七、65535方法数限制问题解决方案
如果因为接入UPSDK导致方法数超过65535，无法正常打包，请使用Android的分包方案打包。关于分包若有疑问，请阅读使用我们提供的[MultiDex分包方案](http://docs.upltv.com/zh/master/Android/android02_65535.html "如下方案")。

