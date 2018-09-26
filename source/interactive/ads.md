## 互推互动广告


### 互推激励互动广告


#### **生成互推激励互动广告**
```java
 AvidlyPlayableRecommendedRewardAd rewardAd = AvidlyPlayableRecommendedRewardAd.newRdAd();
```

#### **监听插屏互动广告回调，并加载广告**
```java
rewardAd.setAdListener((new PlayableRewardListener() {
                        @Override
                        public void onAdReward() {

                        }

                        @Override
                        public void onAdNoReward(String reason) {

                        }

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
rewardAd.load();
```

#### **展示广告：**
```java
if(rewardAd.isReady())
   rewardAd.show();
```

### 互推插屏互动广告

#### **生成互推插屏互动广告**
```java
 AvidlyPlayableRecommendedInterstitialAd interstitialAd = AvidlyPlayableRecommendedAd.newIlAd();
```

#### **监听插屏互动广告回调，并加载广告**
```java
interstitialAd.setAdListener(new PlayableAdListener() {

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



