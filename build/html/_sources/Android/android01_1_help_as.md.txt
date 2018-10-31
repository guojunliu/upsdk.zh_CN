# Android Studio 接入帮助

### 一、UPSDK的目录结构
针对Android Studio 或 Gradle 构建的工程，UPSDK提倡以`*.aar`的方式被其它主工程导入。UPSDK Studio版本的下载包解压后的目录结构如下：

![](http://docc.upltv.com/uploads/201805/5af5689a90a1a_5af5689a.png)

#### 1. UPSDK 主包
如上图所示，命名为`UPAdsSdk_x.x.xx.aar`的文件即为UPSDK 主包，必须添加到你的主工程中。

#### 2. UPSDK 联盟依赖库
UPSDK与其它联盟广告商是一种松散耦合在的关系，可以根据实际需要在主工程中剔除某些不支持广告依赖库，从而减少应用程序的包体大小。

这些联盟商的依赖库除了Admob与Facebook广告依赖库，其它的目前都是xxxx_ads.aar的文件形式存在。

#### 3. Admob与Facebook的广告依赖库
UPSDK的本地文件中，提供了Admob与Facebook 两种广告平台的aar依赖库，以备在网络不佳或其它原因时直接从本地libs中加载提供便利。即使如此，我们仍然提倡尽量在gradle中在线从两者的远程仓库更新下载合适的依赖库。
> 有关Admob与Facebook 两种广告平台的接入说明，在本节的后续内容中有详解解说。


### 二、使用 Android Studio 的 Gradle 导入UPSDK主包

根据上文的介绍，在你下载好的文件目录中找到名为 `UPAdsSdk_x.x.xx.aar`的文件，并添加到项目的`libs`目录下。
添加后，Studio工程的效果图如下所示：

![](http://docc.upltv.com/uploads/201805/5af56922f4043_5af56922.png)

> `UPAdsSdk_3.0.03.aar`仅作为示例参考说明

为了确保libs目录的aar包能正确被工程引用，请检查`app`目录下的`build.gradle`文件中，是否添加以下配置参数：

    repositories {
        flatDir {
            dirs 'libs'
        }
    }

最后将aar添加到compile方法中

    dependencies {
        // your other setting
        // 请将UPAdsSdk_3.0.33替换成实际的文件名
        compile(name: 'UPAdsSdk_3.0.03', ext: 'aar')
    }

至此，UPSDK的aar包已经成功配置到你的工程中，静等gradle编译生效。但UPSDK的工程导入工作还未完成，还差重要的最后一步工作要做：**添加其它依赖库**。

> 作为优秀而强大的聚合平台，UPSDK必须灵活兼容第三方广告联盟商的运行库，才能取百家之长发挥最大的广告效益。因此，正确添加第三方依赖库，才能发挥UPSDK的最大潜能。

如果您使用 Android Studio 或 Gradle 构建 Android 项目，那么首先需要将目录中的 `UPAdsSdk_x.x.xx.aar` 文件导入到您的项目中，目录中的 `xxx_ads.aar` 文件为使用各个广告联盟需要依赖的文件，您可以将其全部导入到项目中，如果有特殊原因导致您不希望导入某个平台的文件，您也可以只选择需要的广告联盟的文件进行导入。

### 三、添加广告联盟的依赖库

部分广告商的 SDK 运行依赖一些公共的第三方库，使用 Android Studio 构建的项目可以通过下述方式来将所依赖的第三方库导入你的项目。

**原则上，我们建议将依赖的第三方库全部导入你的项目，但在某些确定条件下可以有选则性地添加某些广告联盟库以减少工程大小。但无论如何确保你所添加的联盟库完全满足当前广告业务运行时所依赖的外部条件，否则会因为缺少某些必要的支持而导致崩溃.**

如果你想去除某联盟商的支持，但又不清楚如何操作，请优先与我们的技术支持人员沟通，在他们的协助下你将会正确而顺利达成目的。

####  1.添加联盟商的*.aar
在你下载的目标文件中，命名为 `xxx_ads.aar` 的文件为使用各个广告联盟需要依赖的文件。请参考**导入UPSDK aar文件**的方式将其添加到你的工程中。

####  2. 添加其它外部依赖

由于一些联盟商运行时，需要其它额外的Android Api支持，因此还必须添加如下外部依赖库。

#### 1. 在Gradle中配置jcenter节点
首先在你项目的主工程 `build.gradle` 文件中 `repositories` 节点加入 `jcenter`

    buildscript {
        repositories {
            mavenCentral();
            // ******** 加入 jcenter ********
            jcenter()
            maven {
                url 'https://maven.google.com/'
                name 'Google'
            }
        }
        dependencies {
            classpath 'com.android.tools.build:gradle:2.1.3'
        }
    }

    allprojects {
        repositories {
            // ******** 加入 jcenter ********
            jcenter()
            maven {
                url 'https://maven.google.com/'
                name 'Google'
            }
        }
    }


#### 2. 加入 Android Support 支持库

**【注意】由于Facebook等广告依赖了recyclerview的库，Google Admob等广告依赖Android Support V4库，因此请务必将这些依赖库引入你的项目，否则会导致程序崩溃！**

广告的正常展示需要 `support` 库的支持，请参考以下方式将其引入到您的项目中。

可以通过以下的方式添加依赖
    
    dependencies {
        compile 'com.android.support:recyclerview-v7:26.1.0'
        compile 'com.android.support:support-v4:26.1.0'
        compile 'com.android.support:appcompat-v7:26.1.0'
        compile 'com.android.support:support-annotations:26.1.0'
        compile  'com.android.support:customtabs:26.1.0'
        compile 'com.android.support:cardview-v7:26.1.0'
        //gson-2.7.jar在android_support_library目录中
        compile(name: 'gson-2.7', ext: 'jar')
    }
    
如果您不想通过上面的方式引入`support`库，我们还为您在  `android_support_library` 文件夹中准备了对应的 `xxx.aar` 文件，您只需要将文件添加到项目中并且加入`compile`命令即可。

```groovy
dependencies {
   //core lib
   compile(name: 'UPAdsSdk_Cpp_3.0.04_dex', ext: 'aar')
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


#### 3. 加入Google Ads SDK
如果您的项目中希望接入Admob的广告，我们需要您在的项目中加入 Google Ads 支持，可以按以下的方式添加依赖

    dependencies {
        compile 'com.google.android.gms:play-services-ads:16.0.0'
    }

> 如果你只想通过本地添加gms play所依赖的aar文件，也可以在gradle文件中忽略此配置。
> 特别地，如果你的工程中已经存在与UPSDK所依赖的gms play不同版时，请用更高的版本替换低版本。

### 四、修改 Proguard
如果你的项目使用了 `proguard`，你需要将 `proguard-project.txt` 文件中的内容复制粘贴到你项目使用的 `proguard` 配置文件中。

### 五、Demo工程
为帮助您更好的了解广告SDK的接入以及使用，我们在这里提供了一个简单的[Demo工程](https://github.com/AvidlyGit/AdSdkDemo-Studio "Demo工程")。