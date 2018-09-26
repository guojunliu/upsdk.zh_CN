## 激励互动广告


### **在 Application 类中初始化激励互动广告**
选择海外域名初始化AvidlyPlayableRewardAd:
```java
private AvidlyPlayableRewardAd avidlyPlayableRewardAd;

@Override
public void onCreate(Bundle savedInstanceState) {
...
  // Instantiate an InterstitialAd object
   avidlyPlayableRewardAd = AvidlyPlayableRewardAd.getInstance(this, YOUR_AFFKEY);
...
```
选择国内域名初始化AvidlyPlayableRewardAd:
```java
private AvidlyPlayableRewardAd avidlyPlayableRewardAd;

@Override
public void onCreate(Bundle savedInstanceState) {
...
  // Instantiate an InterstitialAd object
   avidlyPlayableRewardAd = AvidlyPlayableRewardAd.getInstance(this, YOUR_AFFKEY, false);
...
```
### **开启DEBUG模式可以监控广告加载的过程**
```java
  avidlyPlayableRewardAd.setDebug(true);
```
### **显示激励互动广告**
设置 AdListener 并加载广告。
```java
avidlyPlayableRewardAd.setAdListener(new avidlyPlayableRewardAd.setAdListener(new AvidlyPlayableRewardAdListener() {
                        @Override
                        public void onAdReward() {

                        }

                        @Override
                        public void onAdDontReward(String reason) {

                        }

                        @Override
                        public void onAdLoaded() {
                           avidlyPlayableRewardAd.show();
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
                }
avidlyPlayableRewardAd.load();
```
### **获取试玩广告的信息**
```java
  Map<String,String> infos = avidlyPlayableRewardAd.getOfferInfo();
```
获取试玩广告的icon:
```java
  String icon = infos.get("icon");
```

### **最后，将以下代码添加到 Activity 类的 onDestroy() 函数，释放 InterstitialAd 使用的资源**
```java
@Override
protected void onDestroy() {
  if (avidlyPlayableRewardAd != null) {
    avidlyPlayableRewardAd.destroy();
  }
  super.onDestroy();
}
```

### **注意事项**：
激励互动广告支持的最小OS版本为4.1








