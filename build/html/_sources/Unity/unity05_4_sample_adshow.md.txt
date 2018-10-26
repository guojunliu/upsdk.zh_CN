

## Unity Plugin 广告展示

#### 插屏广告展示API
```csharp
public static void showIntersitialAd(string cpPlaceId);
```
> 参数：cpPlaceId为广告位标识，必须正确指定，否则广告显示异常或无法显示。
> showIntersitialAd()方法，首先会根据参数cpPlaceId查询对应的广告是否存在，如果存在则进一步判断是否准备就绪。只有在二者都成立的条件下，才会显示广告。因此showIntersitialAd()内部会调用isInterstitialReady()方法。

#### 检测插屏广告状态API
插件保留了对插屏广告状态检测API的支持，一般情况下直接调用showIntersitialAd()方法即可完成广告显示。但在有特殊要求时，比如根据插屏广告的状态来控制某个按钮是否显示，可以通过此方法来实现。

    public static bool isInterstitialReady(string cpPlaceId)

#### 示例代码：
“**inter_bbb**”与“**inter_ccc**”是插屏广告的广告位，分别对应两种不同条件的插屏广告。

```csharp
public void onBtnIntertitialClick() 
{ 
    if (UPSDK.isInterstitialReady("inter_bbb")) {
        UPSDK.showIntersitialAd("inter_bbb");
    }
}

public void onBtnIntertitial_CCC_Click()
{
    if (UPSDK.isInterstitialReady("inter_ccc")) {
        UPSDK.showIntersitialAd("inter_ccc");
    }
}
```
> 由于示例代码不关心插屏是否准备就绪，因此无须调用*isInterstitialReady(string cpPlaceId)*的方法判断广告状态。



### Unity Plugin 激励视频广告

#### 激励视频显示API

```csharp
public static void showRewardAd(string cpCustomId)
```
参数：cpCustomId为用户自定义标识
showRewardAd()运行时也会在内部检查视频广告是否准备就绪。

#### 检测视频状态API
一般情况下，直接调用showRewardAd()方法就能实现视频的显示。此方法与isInterstitialReady()类似，也是用于满足特定要求而提供的。
```csharp
public static bool isRewardReady()
```

#### 示例代码：
```csharp
public void onBtnReward_aaa_Click()
{
    if (UPSDK.isRewardReady()) {
         UPSDK.showRewardAd("aaa");
    }
}
```

> 点击测试激励视频的按钮时，调用onBtnReward_aaa_Click()方法。参数"aaa"是用户自定义标识，用于统计打点做区分。