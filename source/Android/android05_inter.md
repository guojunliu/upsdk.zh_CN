## 插屏广告 

### 广告对象初始化

使用如下方式初始化插屏广告
>以下【广告位】可由接入方向**我方技术支持同事询问**。不同广告使用场景请尽量保持【广告位】的独立性，方便未来区分不同广告展示的收益来源。  
> 例如：在暂停界面中初始化广告时，【广告位】可传入"Pause" or "Menu"

    mInterstitialAd = new UPInterstitialAd(this, "广告位");


### 广告加载监听
监听插屏广告加载结果，如果长时间未加载成功，会产生onLoadFailed()的回调。
```java
public void load(UPInterstitialLoadCallback callback)
```

示例代码：

```java
UPInterstitialAd mInterstitialAdAAA = new UPInterstitialAd(InterstitialActivity.this, "inter_aaa");
final UPInterstitialLoadCallback callback = new UPInterstitialLoadCallback() {
        @Override
        public void onLoadSuccessed(String placement) {
            Log.i(TAG, "InterstitialAd " + placement + " onLoadSuccessed:");

            if (placement.equals("inter_aaa")) {
                // inter_aaa位的插屏广告加载成功，可以展示
            }
        }

        @Override
        public void onLoadFailed(String placement) {
            Log.i(TAG, "InterstitialAd " + placement + " onLoadFailed:");
            if (placement.equals("inter_aaa")) {
                // inter_aaa位的插屏广告加载失败
            }
        }
    };

// 设置回调接口
mInterstitialAdAAA.load(callback);
```

### 显示插屏广告
根据您的业务逻辑，在您希望显示插屏广告的时机，调用show方法显示插屏广告

**请注意在显示之前需要调用`isReady()`方法判断当前是否有广告可以显示**

    if (mInterstitialAd != null && mInterstitialAd.isReady()) {
        mInterstitialAd.show();
    }

### 回调接口
插屏广告可以通过`setAvidlyInterstitialAdListener`方法设置回调接口，您的业务中如果没有需要针对相应回调的特殊处理，可以不使用回调。

    mInterstitialAd.setUpInterstitialAdListener(new UPInterstitialAdListener() {
        @Override
        public void onClicked() {
            // 此处为广告点击的回调
        }

        @Override
        public void onClosed() {
            // 此处为广告关闭的回调
        }

        @Override
        public void onDisplayed() {
            // 此处为广告展示的回调
        }
    });
    
    
### 调试页面
为方便您了解视频广告的使用和加载状态，我们提供了视频广告调试页面，帮助您在开发过程中更好的了解视频广告的使用情况，可通过如下方法调用

    if (mInterstitialAd != null) {
        mInterstitialAd.showInterstitialDebugActivity(this);
    }

页面显示效果如下图，通过左右滑动您可以分别查看当前是否引入了对应联盟的SDK，服务器是否配置为您配置了当前联盟的广告位信息，以及联盟广告是否加载成功，在已经加载成功的情况下，点击条目可以观看联盟的广告效果。

<img src="http://docc.upltv.com/uploads/201810/5bcd4065a93a4_5bcd4065.png">
<img src="http://docc.upltv.com/uploads/201810/5bcd408d13bcb_5bcd408d.png">
