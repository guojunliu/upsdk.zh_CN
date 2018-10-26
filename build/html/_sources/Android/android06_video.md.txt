## 激励视频
### 广告对象初始化
使用如下方式初始化视频广告

    mVideoAd = UPRewardVideoAd.getInstance(this);

### 广告加载监听
监听激励视频加载结果，如果长时间未加载成功，会产生onLoadFailed()的回调。
```java
public void load(UPRewardVideoLoadCallback callback)
```

示例代码：

```java
mVideoAd = UPRewardVideoAd.getInstance(VideoActivity.this);
mVideoAd.load(new UPRewardVideoLoadCallback() {
        @Override
        public void onLoadFailed() {
            // code
            // 激励视频加载失败，请等待加载成功
        }

        @Override
        public void onLoadSuccessed() {
            // code
            // 激励视频加载成功，可以展示
        }
    });
```

### 显示视频广告
根据您的业务逻辑，在您希望显示视频广告的时机，调用`isReady()`方法判断是否有可播放的视频广告，来决定您是否显示播放视频广告的按钮，然后调用`show("广告位")`方法显示视频广告，在`show("广告位")`方法中，传入可以帮助您区分视频广告入口的参数

**【注意】在播放之前需要调用`isReady()`方法判断当前是否有视频广告可以播放，`show("广告位")`在不同的激励视频广告点传入可以代表当前广告点的参数，用于后期分析各广告点广告的数据情况，<font color=red>不同的广告点请不要传入相同的参数</font>**

> 以下【广告位】可由接入方自定义。不同广告使用场景请尽量保持【广告位】的独立性，方便未来区分不同广告展示的收益来源。  
> 例如：在暂停界面中初始化广告时，【广告位】可传入"Pause" or "Menu",为了保证此参数有意义，不能传空值(""或null)。在无法明确用途时，可以设定为"avidly_rewardvideo"


    if (mVideoAd != null && mVideoAd.isReady()) {
        mVideoAd.show("广告位");
    }

### 回调接口
插屏广告可以通过`setUpVideoAdListener`方法设置回调接口，原则上您只需要关心`onVideoAdReward`和`onVideoAdDontReward`两个回调方法，来决定您是否需要向用户发送奖励

**【注意】只有在收到`onVideoAdReward`这个回调之后，才发奖励，其他情况不发**

    mVideoAd.setUpVideoAdListener(new UPRewardVideoAdListener() {
        @Override
        public void onVideoAdClicked() {
            // 此处为广告点击的回调
        }

        @Override
        public void onVideoAdClosed() {
            // 此处为广告关闭的回调
        }

        @Override
        public void onVideoAdDisplayed() {
            // 此处为广告展示的回调
        }

        @Override
        public void onVideoAdReward() {
            // 此处为广告可以发放奖励的回调
        }

        @Override
        public void onVideoAdDontReward(String reason) {
            // 此处为广告观看不符合条件，不发放奖励的回调，一般是因为观看视频时间短
        }
    });

### 调试页面
为方便您了解视频广告的使用和加载状态，我们提供了视频广告调试页面，帮助您在开发过程中更好的了解视频广告的使用情况，可通过如下方法调用

	if (mVideoAd != null) {
		mVideoAd.showVideoDebugActivity(this);
	}

页面显示效果如下图，通过左右滑动可以分别查看当前是否引入了对应联盟的SDK，服务器是否配置为您配置了当前联盟的广告位信息，以及联盟视频广告是否加载成功，在已经加载成功的情况下，点击条目可以观看联盟的广告效果。

![](http://docc.upltv.com/uploads/201805/5af577cb48bf4_5af577cb.png)	![](http://docc.upltv.com/uploads/201805/5af5789e0836b_5af5789e.png)