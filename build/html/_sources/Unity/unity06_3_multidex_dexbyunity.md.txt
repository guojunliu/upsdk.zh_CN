## 使用Unity 解决65535问题
Unity 5.5及以上版本支持，不用导出到Android Studio工程，直接在Unity IDE中完成分包设置。

### 1.开启`Gradle build system`

- 在`Unity Editor`, 打开`Build Settings` 窗口 ( File > Build Settings…)
- 在平台列表中, 选择 `Android`
- 设置 `Build System` 到 `Gradle (new)`

不同Unity版本，**Build Settings**略有差异，请灵活处理。

![666](http://docc.upltv.com/uploads/201807/5b39e51a967db_5b39e51a.jpeg "666")

### 2.更改`Gradle settings`

###### (1) 对于Unity2017.2及以上版本，可以打开 `Player Settings`，如下勾选 `Custom Gradle Template`复选框。其它版本，需要复制 `mainTemplate.gradle` 文件(在Unity安装目录搜索mainTemplate )到 `Assets/Plugins/Android/mainTemplate.gradle`。

![777](http://docc.upltv.com/uploads/201807/5b39ec4b74539_5b39ec4b.jpeg "777")

###### (2) 在**defaultConfig**中添加**multiDexEnabled true**

```groovy
defaultConfig {
    targetSdkVersion **TARGETSDKVERSION**
    applicationId '**APPLICATIONID**'
    multiDexEnabled true
    ndk {
            abiFilters **ABIFILTERS**
        }
}
```

###### (3) 在dependencies中添加**com.android.support:multidex:1.0.1**
一般当前应用的Android API最低版本都在20以下，因此需要在dependencies中添加**com.android.support:multidex:1.0.1**。
```groovy
dependencies {
    compile 'com.android.support:multidex:1.0.1'
    compile fileTree(dir: 'libs', include: ['*.jar'])
}
```

###### (4) 删除minifyEnabled与useProguard参数
如果出现`shrinking/minification is not supported with Multidex`错误，请删除useProguard参数，甚至进一步删除minifyEnabled参数。
如下所示：

```groovy
buildTypes {
    debug {
        minifyEnabled **MINIFY_DEBUG**
        //useProguard **PROGUARD_DEBUG**
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-unity.txt'**USER_PROGUARD**
        jniDebuggable true
    }
    release {
        minifyEnabled **MINIFY_RELEASE**
        //useProguard **PROGUARD_RELEASE**
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-unity.txt'**USER_PROGUARD**
        **SIGNCONFIG**
    }
}
```

### 3.初始化Multidex 
如果 `Assets/Plugins/Android/ `不存在AndroidManifest.xml，请从Unity的安装目录复制默认的AndroidManifest.xml到此目录。如果AndroidManifest.xml的**application**标签中没有为`android:name`设置MultiDexApplication或MultiDexApplication的子类，请添加`android.support.multidex.MultiDexApplication`。

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">
    <application
            android:name="android.support.multidex.MultiDexApplication" >
        ...
    </application>
</manifest>
```

### 4.开启混淆
建议开启混淆功能优化代码，只要勾选“Use Proguard File”的复选框，UnityIDE会自动在`Assets/Plugins/Android`目录下生成一个`proguard-user.txt`的文件，用于添加混淆配置条件。如果这个文件是空的，首先将以下内容添加到文件中。
```groovy
-keep class bitter.jnibridge.* { *; }
-keep class com.unity3d.player.* { *; }
-keep class org.fmod.* { *; }
-ignorewarnings
```

然后再请将UPSDK的混淆配置（位于`PolyADSDK/Plugins/Android/lib_res/proguard-project.txt`）复制到`proguard-user.txt`中。
