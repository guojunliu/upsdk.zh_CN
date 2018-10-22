## 65535方法数超量问题

当方法数超过65535的限制时，apk生成时会出现以下失败：

![222](http://docc.upltv.com/uploads/201807/5b39ca2058b2a_5b39ca20.png "222")

> `trouble writing output: Too many method references to fit in one dex file: 67449; max is 65536.`

如果你的unity IDE接入过多的aar或jar包导致方法数起过65535的时候，会导致以上打包失败。针对这种问题，Unity自5.5开始支持Multidex，通过Dex分包解决。

### 1. [Android Studio分包](http://docs.upltv.com/docs/show/234 "Android Studio分包")
在Android Studio工程中，可能通过gradle灵活地完成apk打包，前提是必须安装Android Studio2.2.3或更高的版本。

### 2. [UnityIDE 分包](http://docs.upltv.com/docs/show/233 "UnityIDE分包")
只须复制必要的文件与初始设置，就能简单地完成分包。