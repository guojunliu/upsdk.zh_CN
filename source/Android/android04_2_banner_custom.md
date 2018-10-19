## 横幅广告-自定义
### 广告对象初始化
我们支持两种横幅广告的展示，分别为长条形和方形的横幅广告，请根据您的产品特点选择希望使用的广告方式。

> 以下【广告位】可由接入方向**我方技术支持同事询问**。不同广告使用场景请尽量保持【广告位】的独立性，方便未来区分不同广告展示的收益来源。  
> 例如：在暂停界面中初始化广告时，【广告位】可传入"Pause" or "Menu"

如果是长条形的广告，请使用如下方式初始化广告对象

    mBannerAd = new UPBannerAd(this, "广告位");

如果是方形的广告，请使用如下方式初始化广告对象

    mRectangleAd = new UPRectangleAd(this, "广告位");

### 显示横幅广告
以长条形广告为例，首先请在布局文件中准备好广告的父视图，例如：

    <LinearLayout
        android:id="@+id/banner_container"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"/>

然后在代码中通过调用`getBannerView()`方法取得广告视图并加入到父视图中，在广告加载完成后即可在父视图中显示。

    banner_container = (LinearLayout) findViewById(R.id.banner_container);
    banner_container.addView(mBannerAd.getBannerView());

### 回调接口
横幅广告可以通过`setUpBannerAdListener`方法设置回调接口，您的业务中如果没有需要针对相应回调的特殊处理，可以不使用回调。

    mBannerAd.setUpBannerAdListener(new UPBannerAdListener() {
        @Override
        public void onClicked() {
            // 此处为广告点击的回调
        }

        @Override
        public void onDisplayed() {
            // 此处为广告显示的回调
        }
    });
