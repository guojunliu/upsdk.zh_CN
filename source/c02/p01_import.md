# 工程引入


## 添加AvidlyAnalysisSDK
将AvidlyAnalysisSDK.framework拖入工程中，并勾选如下配置
![aaa](http://docs.upltv.com/uploads/201807/5b3c81f77d038_5b3c81f7.png "aaa")

## 添加系统framework
- 在工程中添加以下系统库
 - libsqlite3.dylib
 
   如下图所示:
   ![002](http://docs.upltv.com/uploads/201807/5b3c825f09428_5b3c825f.png "002")
   
## 工程配置
- 在info.plist中加入以下节点，以兼容http模式
```java
 &lt;key>NSAppTransportSecurity &lt;/key>
 &lt;dict>
     &lt;key>NSAllowsArbitraryLoads &lt;/key>
     &lt;true/>
 &lt;/dict>
```

- 在TARGETS → Build Setting → Linking → Other Linker Flags 中加入 -ObjC
  ![333](http://docs.upltv.com/uploads/201807/5b3c85c61e5f1_5b3c85c6.png "333")
  
- 开启Keychain Sharing（iOS 10以上需要开启）
  ![555](http://docs.upltv.com/uploads/201807/5b3c860d2db60_5b3c860d.jpeg)

## 兼容性
- 语言兼容性
 1. objective-c 正常使用
 2. swift 需创建桥接
 
- bitcode兼容
 1. 支持bitcode
 
- 设备兼容性
 1. 模拟器
 2. iphone5及以上机型
 
- CPU兼容性
 1. i386
 2. armv7
 3. x86_64
 4. arm64

- iOS版本兼容性
1. ios 7.0 以上

