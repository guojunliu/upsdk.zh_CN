## 解决65535方法数过多
如果您由于集成的 SDK 过多在构建过程中导致出现

Conversion to Dalvik format failed: Unable to execute dex: method ID not in [0, 0xffff]: 65536

问题时，我们提供如下方案为您解决此问题。

Unity工程请参考[Unity MultiDex方案](http://docs.upltv.com/docs/show/232 "Unity MultiDex方案")。

### 一、开启混淆

如果您的应用未开启混淆，我们建议您开启混淆功能，配置ProGuard后能有效排除一些未使用的类和方法，关于广告SDK的混淆配置请参考下载的资源中的`proguard-project.txt`文件，如果开启混淆后仍然超出限制，请继续参考下面的方案。

### 二、使用MultiDex方案

MultiDex方案是Google官方为突破64K方法数限制而推出的解决方案，如果您使用AndroidStudio进行开发，那么此方案简单易用，按照以下顺序进行配置即可。

首先将如下配置加入工程 build.gradle中：

    android {
        defaultConfig {
            multiDexEnabled true
        }
    }
    dependencies {  
        compile 'com.google.android:multidex:1.0.1'
    } 

然后，如果您的工程中已经含有`Application`类，那么让它继承`android.support.multidex.MultiDexApplication`类即可。

如果您的`Application`已经继承了其他类，不想做出改动，可以通过覆写`Application`类中的`attachBaseContext()`方法解决：

    public class MyApplication extends FooApplication {  
        @Override  
        protected void attachBaseContext(Context base) {  
			super.attachBaseContext(base);
			MultiDex.install(this);  
        }  
    }  

这样就可以有效解决AndroidStudio开发的项目中出现的64K方法数限制问题。

### 三、使用Dex动态加载方案

如果您是使用Eclipse开发，接入MultiDex方案会比较复杂，因此我们提供了另外一种解决方案：Dex动态加载

在下载的SDK压缩包中，您可以看到目录中包含有 `assets` 文件夹，然后将 `assets` 目录拷贝到项目中，如果您不需要接入其中的某个联盟，将 `assets/up_android` 文件夹中对应的 `dex_xxx.jar` 文件删除掉即可。

**【注意】 在以上的复制文件的过程中请不要修改任意文件或者文件夹的命名**

**【注意】`admob`、`batmobi` 和 `inmobi` 广告平台暂时无法支持此种加载方式**

