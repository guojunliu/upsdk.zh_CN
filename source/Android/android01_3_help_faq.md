## 接入问题解决

### 一、Android Studio AndroidManfiest.xml 声明冲突

#### 1. com.google.android.gms.version 冲突或缺失报错
在**Android Studio**版本中，UPSDK为了避免此类冲突，从2.0.21的Studio版本开始，在AndroidManfiest.xml 中已经去掉了如下声明:

	<!-- admob -->
	<meta-data
            android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version"/>

因此，对于2.0.21之前的版本，如果存在gms.version冲突时，请联系我们的技术支持人员协助解决。

但在**Android Elipse**的版本中，需要在主工程的AndroidManfiest.xml 中添加以下声明：

	<!-- admob -->
	<meta-data
            android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version"/>

特别地，在*res/values*目录的xml文件中检查是否存在*google_play_services_version*的定义。一般地，UPSDK将此属性定义在*version_ad.xml*文件中。如果没有定义可直接复制*version_ad.xml*到*res/values*目录下。

*version_ad.xml*内容：

	<?xml version="1.0" encoding="utf-8"?>
	<resources>
		// 定义gms version的值，integer类型
		// 如果当前版本不是9080000，请换成正确的版本值
		<integer name="google_play_services_version">11020000</integer>
	</resources>

#### 2. activity 冲突或缺失报错
在Android Studio的版本中，UPSDK从2.0.21的Studio版本开始，在AndroidManfiest.xml中去掉了对Admob以及Facebook广告所需要依赖的Activity组件声明。

	<!-- admob -->

        <activity
            android:name="com.google.android.gms.ads.AdActivity"
            android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize"
            android:multiprocess="true"
            android:theme="@android:style/Theme.Translucent"/>
        <!-- admob -->


        <!-- facebook -->
        <activity
            android:name="com.facebook.ads.InterstitialAdActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:multiprocess="true"/>

原因是因为Admob以及Facebook在Studio工程中，都以aar方式引入，在各自包中的AndroidManfiest.xml文件里已经声明了相应的组件。

相反，对于Eclipse工程或以jar包接入的Studio工程，需要手动在主工程的AndroidManfiest.xml中添加上以Activity的声明。

### 二、Admob gms 冲突

#### 1. Facebook Ads依赖Admob Ad导致冲突
Facebook Ads通过compile方式在线更新audience-network-sdk时，除了下载audience-network-sdk所对应的aar文件外，还会下载如下两个google gms库文件：
- play-services-ads-8.4.0
- play-services-basement-8.4.0

如果此时google gms service也是在线更新其它版本(如10.0.1)的依赖库时，Studio Gradle会自动去除这两个8.4.0的库；但是如果google gms service(非8.4.0)是**以本地方式**导入时，工程中有会出现两个版本的play-services-ads与play-services-basement的库包，**从而导致gms某些class重复引用的冲突**。

因此如下三种情况下正确：
1. google gms service与facebook audience-network-sdk同时在线更新下载
2. google gms service与facebook audience-network-sdk同时从本地导入aar
3. google gms service在线更新下载，facebook audience-network-sdk从本地导入(aar或jar包都可以)

#### 2.其它情况引起的冲突

由于Admob广告，需要gms play service的支持，对于期望支持Admob广告的工程来说，必须正确添加gms相关的依赖库以及配置。

UPSDK对于Admob的支持，需要依赖gms player service如下几个方面的库：
- play-services-ads-x.x.x
- play-services-ads-lite-x.x.x
- play-services-base-x.x.x
- play-services-basement-x.x.x
- play-services-tasks-x.x.x

> UPSDK目前内置gms的版本是play-services-ads:11.0.4，如果与主工程中现存的gms版本不一致时，请优先保留高版本的gms。为了避免不必要的错误，无论保留那个版本，请务必保证上述五个依赖库齐备且处于同一版本条件下，否则可能会引起class引入重名或重复引用的冲突。


### 三、xxx.so缺失的问题
UPAdsSDK_x.x.x.aar以及adcolony_ads.aar中，提供了如下三种cpu类型的so文件：
- armeabi
- armeabi-v7a
- x86

如果你的工程（如cococs引擎开发的游戏一般仅支持armeabi）支持的cpu类型少于、多于或不同于以上三类，程序运行时都会出现类似于以下的错误。

	java.lang.UnsatisfiedLinkError:
	Couldn't load cocos2dlua from loader dalvik.system.PathClassLoader
		[DexPathList[[zip file "/data/app/org.cocos2dx.xxxxx-1.apk"],
		nativeLibraryDirectories=[/data/app-lib/org.cocos2dx.xxxxx-1, 
		/vendor/lib, /system/lib, /data/datalib]]]: findLibrary returned null
				  at java.lang.Runtime.loadLibrary(Runtime.java:358)
				  at java.lang.System.loadLibrary(System.java:555)
				  at org.cocos2dx.lib.Cocos2dxActivity.onLoadNativeLibraries(Cocos2dxActivity.java:248)
				  at org.cocos2dx.lib.Cocos2dxActivity.onCreate(Cocos2dxActivity.java:264)
				  at org.cocos2dx.lua.AppActivity.onCreate(AppActivity.java:57)

原因是当前应用不同CPU架构所需要的so文件**未对齐**。

解决的办法有二种：
1.对于gradle工程，直接在app目录下的gradle.build文件中添加对实际架构的支持：

	defaultConfig {
        ndk {
			// 假设应用支持如下四种CPU
            abiFilters 'armeabi' ,'armeabi-v7a', 'x86', 'arm64-v8a'
        }

    }

> abiFilters 后面添加的CPU架构，为当前项目实际支持的架构。

2.对于Eclipse或不能修改gradle.build的工程
只要在libs目录下删除当前不需要支持的CPU架构即可。对于aar文件，可以直接用解压软件打开，删除不需要的CPU架构。




