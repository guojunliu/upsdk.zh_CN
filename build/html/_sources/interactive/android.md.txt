# Android Studio SDK集成


## **Android Studio工程**
请将最新版的互动广告aar添加到你app的libs目录下并在gradle中引用：

```java
repositories {
    flatDir {
        dirs 'libs'
    }
}
```

```java
dependencies {
    compile(name: 'AvidlyPlayableAd-1.0.6', ext: 'aar')
}
```
## **混淆配置**
```java
-keep class  com.avidly.playablead.app.** { *; }
```





