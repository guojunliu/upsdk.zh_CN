

## Unity Plugin 横幅广告

#### 1.Banner广告展示API

```csharp
// 在屏幕顶部显示某个广告位的banner广告
public static void showBannerAdAtTop(string cpPlaceId);

// 在屏幕底部显示某个广告位的banner广告
public static void showBannerAdAtBottom(string cpPlaceId);
```
> 参数：cpPlaceId，实际广告位，必须正确指定，否则广告无法显示。

#### 示例代码：
通过不同的测试按钮，测试不同的banner广告显示。
```csharp
// 在当前窗口顶部，展现一个命名为banner_aaa的banner广告
// 如果banner_aaa对应的业务未正确配置，将无法展现广告
public void onBtnBanner_Top_Click()
{
    // 添加对iphonex刘海的适配（根据游戏实际情况决定是否下移）
    UPSDK.setTopBannerForIphonex(32);
    // 添加对华为P20的适配（根据游戏实际情况决定是否下移）
    UPSDK.setTopBannerForHuaWeiP20(75);
    // 显示顶部banner
    UPSDK.showBannerAdAtTop("banner_aaa");
}

// 在当前窗口底部，展现一个命名为banner_bbb的banner广告
// 如果banner_bbb对应的业务未正确配置，将无法展现广告
public void onBtnBanner_Bottom_Click()
{
    UPSDK.showBannerAdAtBottom("banner_bbb");
}
```

> Banner广告不需要判断状态，在合适的时候调用它，SDK会自动加载，关在加载成功后自动填充广告并显示。
虽然Banner广告是自动填充与显示，但尽可能早地调用它的方法，以便留出足够的时间让SDK完成广告的加载与填充。


#### 2.Banner广告隐藏API
```csharp
// 隐藏屏幕顶部位置的banner广告
public static void hideBannerAdAtTop();

// 隐藏屏幕底部位置的banner广告
public static void hideBannerAdAtBottom();
```
> Banner广告被隐藏后，可以通过showBannerAdAtTop()或showBannerAdAtBottom()再次显示，不需要重新加载。


#### 3.Banner广告移除API
```csharp
// 根据广告位移除某个Banner广告的资源，重新展示时会再次加载广告资源
public static void removeBannerAdAt(string cpPlaceId);
```

#### 示例代码：

```csharp
// 将广告位为‘banner_aaa’的顶部Banner移除
// 如果banner_aaa对应的广告不存在，则提示移除失败，否则提示成功
public void onBtnBanner_TopRemove_Click()
{
    UPSDK.removeBannerAdAt("banner_aaa");
}

```
#### 4.顶部Banner位置调整

```csharp
// 仅对ios iphonex
void setTopBannerForIphonex(int padding);
// 仅对华为p20
void setTopBannerForHuaWeiP20(int padding);
```
示例代码：
```csharp
// 让IphoneX手机，顶部Banne下移32像素
UPSDK.setTopBannerForIphonex (32);
```