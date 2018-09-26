## Eclipse SDK集成

### **Eclipse工程**
**第一步解压AvidlyPlayableAd-1.0.6.zip,得到如下文件：**
<br>![](http://docs.upltv.com/uploads/201709/59afaab8cf593_59afaab8.png)</br>
<br> **第二步将AvidlyPlayableAd-1.0.6.jar拷贝到libs文件夹下面**</br>
<br>![](http://docs.upltv.com/uploads/201709/59afab1333704_59afab13.png)</br>
 <br>**第三步将如下目录下面的资源依次拷贝到自己工程对应的目录下面：**</br>
<br>![](http://docs.upltv.com/uploads/201708/59a7d3306bb7a_59a7d330.png)</br>
<br> **第四步按需拷贝自己工程所需要的so：**</br>
<br>![](http://docs.upltv.com/uploads/201708/59a7d83a17ff4_59a7d83a.png)</br>

### **混淆配置**
```java
-keep class  com.avidly.playablead.app.** { *; }
```
