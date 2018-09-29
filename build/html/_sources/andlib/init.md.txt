# 工程引入

## 添加SDK包
统计SDK提供有*.aar与*.jar两种格式的包，如果使用AndroidStudio或Unity工程（也可以选择插件包）请选择*.aar格式的包，Eclipse工程请选择*.jar包。

### 1. AndroidStudio工程
请将*.aar包复制到工程的`libs`目录下，如下图所示的`analysissdk-gaid_R3.1.1.6.aar`。

 ![1001](http://docs.upltv.com/uploads/201807/5b3c6ae443849_5b3c6ae4.jpeg "1001")

然后将**analysissdk-gaid_R3.1.1.6.aar**添加到**build.gradle**的**dependencies**标签中。

```groovy

repositories {
    flatDir {
        dirs 'libs'
    }
}

dependencies {
   // 引用统计包
    compile(name: 'analysissdk-gaid_R3.1.1.6', ext: 'aar')
}
```

### 2.  Eclipse工程
请将*.jar文件复制到工程的`libs`目录，如**analysissdk-gaid_R3.1.1.6.jar**。


### 3. Unity工程
请将*.aar文件复制到**Assets/Plugins/Android**目录下，如下图所示。
 ![1002](http://docs.upltv.com/uploads/201807/5b3c6d5ddc658_5b3c6d5d.jpeg "1002")


## 权限依赖
统计包依赖如下权限：
```xml
 &lt;uses-permission android:name="android.permission.INTERNET"/>
 &lt;uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
 &lt;uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
```

**对于以*.jar方式引用的统计包，请将以上权限添加到项目的AndroidManifest.xml中。**

## 应用安装数配置
为了正确统计应用的安装数，统计包中依赖**com.android.vending.INSTALL_REFERRE**的静态广播组件。
```xml
&lt;receiver
     android:name="com.statistics.channel.InstallReferrerReceiver"
     android:exported="true">
     &lt;intent-filter>
         <action android:name="com.android.vending.INSTALL_REFERRER" />
     &lt;/intent-filter>
 &lt;/receiver>
```

**对于以*.jar方式引用的统计包，请将以上receiver添加到项目的AndroidManifest.xml中。**

## 混淆配置

如果您的项目开启了混淆功能，请按照如下规则添加混淆配置。
```xml
-keep class com.hola.sdk.* {*;}
-keep class com.statistics.channel.* {*;}
```

## 2.X版本兼容
如果您之前接入的版本是2.X或更早的版本，请在请将以上receiver添加到项目的AndroidManifest.xml中删除如下的 provider 定义，否则会出现程序崩溃。


```xml
&lt;provider
    android:name="com.statistics.channel.ChannelProvider"
    android:authorities="${applicationId}_com.hola.a.provider" />
```
