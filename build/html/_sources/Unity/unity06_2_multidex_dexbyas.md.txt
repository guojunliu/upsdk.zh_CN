## 使用Android Studio 解决65535问题

Unity 5.5开始支持Gradle build system，对于65535超量的错误可以考虑将工程导出到Android Studio通过分包来解决。

#### 1.导出到Android Studio工程

![3333](http://docc.upltv.com/uploads/201807/5b39cc6bd83bb_5b39cc6b.png "3333")


#### 2.打开Android Studio工程
必须安装Android Studio2.2.3及上版本。

![4444](http://docc.upltv.com/uploads/201807/5b39ccc80a994_5b39ccc8.png "4444")

#### 3.配置build.gradle
如果你的gradle版本过低，请设置到2.2.3或以上。
![555](http://docc.upltv.com/uploads/201807/5b39cd2136c17_5b39cd21.png "555")

在build.gradle文件中添加分包设置：

```groovy
android {
    defaultConfig {
        ...
        minSdkVersion 16 
        targetSdkVersion 26
        multiDexEnabled true
    }
    ...
}
dependencies {
  compile 'com.android.support:multidex:1.0.1'
}
```

在**AndroidManifest.xml **中添加：
```groovy
<application
            android:name="android.support.multidex.MultiDexApplication" >
        ...
</application>
```

#### 4.OutOfMemory问题
运行工程，出现OutOfMemory的问题时，在gradle.build中添加以下配置偿试解决。

```groovy
android {
    defaultConfig {
        ....
    }
    dexOptions {
        javaMaxHeapSize "4g"
    }
}
```

#### 5. 混淆修改
Android Studio中出现`Build-in class shrinker and multidex are not supported yet`错误提示时，大多数情况是因为useProguard字段导致的。请将useProguard 改成minifyEnabled试试。
```groovy
android {
    buildTypes {
        release {
            minifyEnabled true
        }
    }
}
```
> 如果未开启混淆，可以去掉minifyEnabled配置或设为false。

如果开启混淆功能，请将UPSDK的混淆配置（`PolyADSDK/Plugins/Android/lib_res/proguard-project.txt`）复制到Android Studio的`proguard-unity.txt`中。

```groovy
release {
    minifyEnabled true
    //useProguard false
    proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-unity.txt'
    signingConfig signingConfigs.release
}
```