## 插屏互动广告


### **在 Activity 类中初始化插屏互动广告**
选择海外域名始化InterstitialAd:
```java
private AvidlyPlayableInterstitialAd interstitialAd;

@Override
public void onCreate(Bundle savedInstanceState) {
...
  // Instantiate an InterstitialAd object传入affkey
  interstitialAd = new AvidlyPlayableInterstitialAd(this,"YOUR_AFFKEY");
...
```
选择国内域名始化InterstitialAd:
```java
private AvidlyPlayableInterstitialAd interstitialAd;

@Override
public void onCreate(Bundle savedInstanceState) {
...
  // Instantiate an InterstitialAd object传入affkey
  interstitialAd = new AvidlyPlayableInterstitialAd(this,"YOUR_AFFKEY", false);
...
```
### **开启DEBUG模式可以监控广告加载的过程**
```java
  interstitialAd.setDebug(true);
```
### **显示插屏互动广告**：
设置 AdListener 并加载广告。
```java
avidlyPlayableInterstitialAd.setAdListener(new AvidlyPlayableIntersitialAdListener() {
            @Override
            public void onAdLoaded() {
            }

            @Override
            public void onAdLoadFailed() {

            }

            @Override
            public void onAdClicked() {

            }

            @Override
            public void onAdClosed() {

            }

            @Override
            public void onAdDisplayed() {

            }
        });
interstitialAd.load();
```

### **设置cpApId(可选)**
```java
interstitialAd.setCpApId("cp_ad_id");
```

### **展示广告：**
```java
if(interstitialAd.isReady())
   interstitialAd.show();
```

### **获取试玩广告的信息**
```java
  Map<String,String> infos = interstitialAd.getOfferInfo();
```
获取试玩广告的icon:
```java
  String icon = infos.get("icon");
```

### **最后，将以下代码添加到 Activity 类的 onDestroy() 函数，释放 InterstitialAd 使用的资源：**
```java
@Override
protected void onDestroy() {
  if (interstitialAd != null) {
    interstitialAd.destroy();
  }
  super.onDestroy();
}
```

### **注意事项**：
插屏互动广告支持的最小OS版本为4.1








