## 回调接口

版本3003，我们修改了Unity Plugin的访问类为`UPSDK.cs`，`UPSDK.cs`中的回调接口(Action)以静态方法定义，因此其它模块可以直接访问。

#### 1. 激励视屏回调接口：
#### 回调接口调定义

```csharp
public static Action&lt;bool, string> UPSDKInitFinishedCallback = null;
	 
/*
* 激励视屏回调
* 激励视屏打开时（显示出来）回调此代理
*/
public static Action&lt;string, string> UPRewardDidOpenCallback = null;
		
/*
* 激励视屏被点击时回调此代理
*/
public static Action&lt;string, string> UPRewardDidClickCallback = null;
		
/*
* 激励视屏奖励发放时回调此代理
*/
public static Action&lt;string, string> UPRewardDidGivenCallback = null;
		
/*
* 激励视屏奖励发放失败时或发放条件不成立时回调此代理
*/
public static Action&lt;string, string> UPRewardDidAbandonCallback = null;
		
/*
* 激励视屏被关闭回调此代理
*/
public static Action&lt;string, string> UPRewardDidCloseCallback = null; 

```
#### 回调接口调用顺序
###### 1. 激励视屏显示时回调
激励视屏只要显示成功，必定会首先调用UPRewardDidOpenCallback。

#### 2. 激励视屏被点击时回调
仅当用户有点击行为时才会调用UPRewardDidClickCallback，因此点击回调有可能不被调用。

#### 3. 激励视屏奖励发放回调
激励视屏只要显示成功，都会根据结果产生奖励发放或不发放事件。奖励发放时调用UPRewardDidGivenCallback，不发放时UPRewardDidAbandonCallback，且只能调用二之一。

#### 4. 激励视屏关闭时回调
激励视屏被关闭时必定会回调UPRewardDidCloseCallback，而且是最后被执行的回调。

> 激励视屏回调顺序简而言之：RewardDidOpen->RewardDidClick(如果有则调用)->RewardDidGiven(or RewardDidAbandon)->RewardDidClose。

#### 2. 插屏广告回调接口
#### 回调接口定义

```csharp
/*
* 插屏广告回调
* 插屏广告显示时回调此代理
*/
public static Action&lt;string, string> UPInterstitialDidShowCallback = null;

/*
* 插屏广告点击时回调此代理
*/
public static Action&lt;string, string> UPInterstitialDidClickCallback = null;

/*
* 插屏广告关闭时回调此代理
*/
public static Action&lt;string, string> UPInterstitialDidCloseCallback = null;
```
#### 回调接口调用顺序
###### 1. 插屏广告显示时回调
插屏广告只要显示成功，UPInterstitialDidShowCallback。
###### 2. 插屏广告被点击时回调
仅当用户有点击行为时才会调用UPInterstitialDidClickCallback，因此点击回调有可能不被调用。
###### 3. 插屏广告关闭时回调
插屏广告被关闭时必定会回调UPInterstitialDidCloseCallback，而且是最后被执行的回调。
> 插屏广告回调顺序简而言之：DidOpen->DidClick(如果有则调用)->DidClose。


#### 3.Banner及退出广告回调
Bannber广告的显示回调仅在第一次展示时调用UPBannerDidShowCallback，每次被点击时都会调用UPBannerDidClickCallback。Bannber广告被删除后会调用UPBannerDidRemoveCallback事件。

退出广告仅适用于Android平台，目前游戏中很少有使用场景，暂不做详细介绍。

```csharp
		
/*
* Banner广告回调
* Banner广告显示时回调此代理
*/
public static Action&lt;string, string> UPBannerDidShowCallback = null;
		
/*
* Banner广告被点击时回调此代理
*/
public static Action&lt;string, string> UPBannerDidClickCallback = null;
		
/*
* Banner广告被删除时回调此代理
*/
public static Action&lt;string, string> UPBannerDidRemoveCallback = null;

#if UNITY_ANDROID && !UNITY_EDITOR
/* 退出广告，仅android支持
* 退出广告显示出来
*/
public static Action&lt;string> UPExitAdDidShowCallback = null;
		
/*
* 退出广告点击回调
*/
public static Action&lt;string> UPExitAdDidClickCallback = null;
		
/*
* 退出广告点击更多的回调
*/
public static Action&lt;string> UPExitAdDidClickMoreCallback = null;
		
/*
* 退出广告退出回调
*/
public static Action&lt;string> UPExitAdOnExitCallback = null;
		
/*
* 退出广告取消回调
*/
public static Action&lt;string> UPExitAdOnCancelCallback = null;
#endif
```