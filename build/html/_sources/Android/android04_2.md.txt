## 横幅广告-快速集成
### UPGameEasyBannerWrapper 应用场景
如果仅仅需要在视图顶部或底部构建长条横幅banner广告时，而且banner广告所依附的activity为当前游戏的activity，我们推荐参考以下方式，能更快更简洁地实现目标。
针对这种情况，UPSDK进一步封装以下对象：
`com.up.ads.wrapper.banner.UPGameEasyBannerWrapper`

### UPGameEasyBannerWrapper API接口说明
  UPGameEasyBannerWrapper以单例模式设计,请用`UPGameEasyBannerWrapper.getInstance().XXXX()`方式调用以下api接口。

```java

   /**
   * 必须优先正确调用此方法，完成game banner的初始化
   * @param gameActivity 不能为空，当前banner广告所依附的activity
   */
   public void initGameBannerWithActivity(Activity gameActivity);

   /**
   * 根据广告位，将banner展现到当前activity的顶部
   * 请在actvity onresume之后调用此方法，避免异常：BadTokenException: Unable to add window -- token null is not valid;
   * @param cpPlaceId banner广告位，通常用来标注某种广告的业务类型
   */
    public void showTopBannerAtADPlaceId(String cpPlaceId);

   /**
   * 根据广告位，将banner展现到当前activity的底部
   * 请在onresume之后调用此方法，避免异常：BadTokenException: Unable to add window -- token null is not valid;
   * @param cpPlaceId banner广告位，通常用来标注某种广告的业务类型
   */
   public void showBottomBannerAtADPlaceId(String cpPlaceId);

   /**
   * 根据广告位，添加banner的回调代理
   * @param cpPlaceId banner广告位，通常用来标注某种广告的业务类型
   * @param callback banner的回调对象
   */
   public void addBannerCallbackAtADPlaceId(String cpPlaceId, UPBannerAdListener callback)

   /**
   * 根据指定的广告位，移除某个banner广告，但不会删除相应的回调
   * 如果此广告位不存在，不会引发任何实际的操作
   * banner广告位一旦被移除，下次show的时候，会再次加载
   * 如果仅仅是隐藏当前banner广告,请调用hideTopBanner()或hideBottomBanner()
   * @param cpPlaceId banner广告位，通常用来标注某种广告的业务类型
   */
   public void removeGameBannerAtADPlaceId(String cpPlaceId);
   
   /**
   * 隐藏当前顶部banner广告
   * 无须区分广告位
   * 再次展示时，需要调用需要调用showTopBannerAtADPlaceId()
   * 2037开始支持
   */
    public void hideTopBanner();
	
   /**
   * 隐藏当前底部banner广告
   * 无须区分广告位
   * 再次展示时，需要调用showBottomBannerAtADPlaceId()
   * 2037开始支持
   */
    public void hideBottomBanner();
	
```
   
	
### UPGameEasyBannerWrapper 使用示例

以下是在当前activity构建一个底部banner的快速方式。

```java

protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_banner);
        // 初始化banner
        UPGameEasyBannerWrapper.getInstance().initGameBannerWithActivity(this);
        // 添加回调接口
        UPGameEasyBannerWrapper.getInstance().addBannerCallbackAtADPlaceId("banner_aaa", new UPBannerAdListener() {
            @Override
            public void onClicked() {
                Log.i(TAG, "banner_aaa onClicked ");
            }

            @Override
            public void onDisplayed() {
                Log.i(TAG, "banner_aaa onDisplayed ");
            }
        });
        // 延期0.2s后显示banner，demo仅仅保证在activity onresume之后调用showBottomBannerAtADPlaceId()
        (new Handler(Looper.getMainLooper())).postDelayed(new Runnable() {
            @Override
            public void run() {
                UPGameEasyBannerWrapper.getInstance().showBottomBannerAtADPlaceId("banner_aaa");
            }
        }, 200);
}
```