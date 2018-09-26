## 激励互动广告


### **激励互动广告**
PlayableAds提供一种可交互的试玩广告，增加了广告的趣味性。激励互动广告使用单例模式，模态窗口的方式展示广告。使用简单，参考如下步骤：

#### **1 引入头文件 PlayableAds.h**<br>
	#import <PlayableAdsSDK/PlayableAds.h>

<br>

#### **2 初始化并设置代理**<br>
```objective-c
/*
 * 初始化
 */
+ (PlayableAdsReward *_Nonnull)sharedInstance;

/**
 * 设置激励互动广告回调
 *
 */
@property (nonatomic, weak, nullable)id <PlayableAdsRewardDelegate> delegate;
```

<br>

#### **3 设置广告位名字（可选）**<br>

```objective-c
/*
 * 广告位名字
 */
@property (nonatomic, strong) NSString *_Nullable placeId;
```

<br>

#### **4 设置区域，默认为国外（可选）**<br>
```objective-c
/*
 * 区域(区分国内国外)，请在load和show方法前设置
 */
@property (nonatomic, assign) VGGlobalZone zone;
```

<br>



#### **5 检查广告是否可显示**<br>
```objective-c
/*
 * 判断预加载的内容是否可显示(校验使用)
 */
- (BOOL)isReady;

```

<br>

#### **6 加载广告**<br>
```objective-c
/*
 * key 广告位的key
 */
- (void)loadRewardAdsResourcesWithKey:(NSString *_Nonnull)key;
```

<br>

#### **7 展示广告**<br>

```objective-c
/**
 * 展示激励互动广告
 * viewController  展示广告的控制器(controller)
 */
- (void)showRewardAdsWithController:(UIViewController *_Nonnull)viewController;
```

<br>

#### **8 示例代码**<br>
我们仅以Xcode工程作为示例，其它IDE工程，请参考修改！<br>
```objective-c
- (void)enterGame:(id)sender{
        if ([[PlayableAds sharePlayableAds] isReady]) {
            [[PlayableAds sharePlayableAds] showRewardAdsWithController:self];
        }else{
            [[PlayableAds sharePlayableAds] loadRewardAdsResourcesWithKey:@"key"];
        }
}

```
<br>

#### **9 获取即将播放的广告信息**<br>
```objective-c
/**
 * 返回待播放的激励互动广告Info
 * @{@"icon":@""}
 */
- (NSDictionary *_Nullable)getOfferInfo;
```

<br>

在对象被销毁时，将代理置空：

```objective-c
[PlayableAds sharePlayableAds].delegate = nil;
```

<br>

目前提供的回调方法有：
```objective-c
/**
 *加载成功
 */
- (void)playableAdsRewardLoadSuccessWithKey:(NSString *_Nonnull)key placeId:(NSString *_Nullable)placeId;

/**
 *加载失败
 */
- (void)playableAdsRewardLoadFailed:(NSDictionary *_Nullable)dic withKey:(NSString *_Nonnull)key placeId:(NSString *_Nullable)placeId;

/**
 *广告打开
 */
- (void)playableAdsRewardDidOpenWithKey:(NSString *_Nonnull)key placeId:(NSString *_Nullable)placeId;

/**
 *广告点击
 */
- (void)playableAdsRewardDidClick:(NSString *_Nullable)placeString withKey:(NSString *_Nonnull)key placeId:(NSString *_Nullable)placeId;
/**
 *广告播放完成
 */
- (void)playableAdsRewardDidPlayCompleteWithKey:(NSString *_Nonnull)key placeId:(NSString *_Nullable)placeId;

/**
 *广告关闭
 */
- (void)playableAdsRewardDidCloseWithKey:(NSString *_Nonnull)key placeId:(NSString *_Nullable)placeId;

/**
 *准备发放奖励
 */
- (void)playableAdsRewardDidRewardUserWithReward:(NSDictionary *_Nullable)reward withKey:(NSString *_Nonnull)key placeId:(NSString *_Nullable)placeId;

/**
 *不符合发放奖励
 */
- (void)playableAdsRewardDontReward:(NSError *_Nullable)error withKey:(NSString *_Nonnull)key placeId:(NSString *_Nullable)placeId;

```

<br>

**调用展示广告和加载广告方法前，必须先设置delegate，否则将收不到回调。播放当前广告时，默认预加载下一个。加载广告的顺序是先检查本地，如果没有，再向服务器请求。**<br><br>

<br>

