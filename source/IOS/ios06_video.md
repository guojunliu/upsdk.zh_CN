## 激励视频
### 激励视频接入

有关激励视频的接口方法与协议都定义在UPRewardWrapper.h文件中。

在XCode工程中引用UPRewardWrapper.h头文件方式：
```objective-c
#import   <UPSDK/UPSDK.h>
```

激励视频唯一区别于插屏广告的地方是**它采用了单例设计模式**，但在方法定义上，与插屏广告非常相似。
*UPRewardWrapper.h*仅有的几个方法解释如下：

    @interface UPRewardWrapper : NSObject
    
    /*
     * 获取Wrapper的单例对象
     */
    + (instancetype)shareInstance;
    
    /*
     * 设置回调代理（非必须）
     */
    - (void)setDelegate:(id<UPRewardDelegate>)delegate;
    
    /*
     * 判断视频内容是否可显示
     */
    - (BOOL)isReady;
    
    /**
     * 显示视频
     * @param viewController，必须正确设置，用于控制视频跳转实现
     * @param adid，cp根据自己需求，自定义视频展示时位置标识，用于统计
     **/
    
    - (BOOL)show:(UIViewController *)viewController placeId:(NSString*)adId;
    
    /**
    * 设置广告加载回调接口
    * @param delegate，回调代理
    **/
    - (void)load:(id<UPRewardLoadDelegate>)delegate;
    @end

激励视频与插屏广告以及横幅广告一样，都对回调定义了回调代理协议。激励视频的回调协议*UPRewardDelegate*也声明在*UPRewardWrapper.h*中。

```objective-c
@protocol UPRewardDelegate <NSObject>

/*
 * 激励视频广告打开
 */
- (void)UPRewardVideoAdDidOpen;

/*
 * 激励视频广告点击
 */
- (void)UPRewardVideoAdDidCilck;

/*
 * 激励视频广告关闭
 */
- (void)UPRewardVideoAdDidClose;

/*
 * 准备发放奖励
 * @param reward: 奖励的有关数据内容
 */
- (void)UPRewardVideoAdDidRewardUserWithReward:(NSDictionary *)reward;

/*
 * 激励条件不足，无法发放奖励
 * @param error: 条件不足的原因
 */
- (void)UPRewardVideoAdDontReward:(NSError *)error;

@end
```

<br>

### 示例代码

定义一个*STRewardViewController*类，用于测试激励视频的显示。
> 我们仅以XCode工程作为示例，其它IDE工程，请参考修改！
    
    @interface STRewardViewController : UIViewController
    
    @end

> STRewardViewController 添加一个按钮，点击它时实现激励视频显示，因此功能简单。

在STRewardViewController.m文件中，添加如下代码

```objective-c

#import   <UPSDK/UPSDK.h>

@interface STRewardViewController () <UPRewardDelegate>

@end
```

> STRewardViewController将实现UPRewardDelegate协议，用于监听视频相关事件的发生。

接下来，继续添加以下代码：

```objective-c
- (void)viewDidLoad {
    [super viewDidLoad];
    self.view.backgroundColor = [UIColor whiteColor];
    
    UIButton *button = [UIButton buttonWithType:UIButtonTypeRoundedRect];
    button.backgroundColor = [UIColor orangeColor];
    button.frame = CGRectMake(self.view.frame.size.width/2 - 250/2, 100, 250, 40);
    [button setTitle:@"激励视频广告" forState:UIControlStateNormal];
    [button addTarget:self action:@selector(rewardClick) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:button];
    
    [[UPRewardWrapper shareInstance] setDelegate:self];
}

- (void)rewardClick
{
    if ([[UPRewardWrapper shareInstance] isReady]) {
        [[UPRewardWrapper shareInstance] show:self placeId:@"aaaa"];
    }
    else
    {
        NSLog(@"Reward no ready");
    }
}

```

到此，激励视频的主要测试代码已经完成，运行后点击“激励视频广告”的按钮，就会显示广告。如果看到“Reward no ready”的提示，说明视频广告还没有加载好，这时要检查下网络是否连接正确。

最后，添加视频代理协议的代码：

```objective-c
#pragma mark - UPRewardDelegate

//激励视频广告打开
- (void)UPRewardVideoAdDidOpen
{
    NSLog(@"UPRewardVideoAdDidOpen");
}

//激励视频广告点击
- (void)UPRewardVideoAdDidCilck
{
    NSLog(@"UPRewardVideoAdDidCilck");
}

//激励视频广告关闭
- (void)UPRewardVideoAdDidClose
{
    NSLog(@"UPRewardVideoAdDidClose");
}

//激励视频广告发放奖励。如果回调此方法，则应该给用户发放奖励
- (void)UPRewardVideoAdDidRewardUserWithReward:(NSDictionary *)reward
{
    NSLog(@"UPRewardVideoAdDidRewardUserWithReward");
}

//激励视频广告不发放奖励。如果回调此方法，表示用户未满足发放奖励邀请，则不发放奖励
- (void)UPRewardVideoAdDontReward:(NSError *)error
{
    NSLog(@"UPRewardVideoAdDontReward");
}
```
> 视频显示后，可以通过观察以上的打印，测试相关的事件行为是否正确发生。

<font color=#DC143C>注意：在收到`UPRewardVideoAdDidRewardUserWithReward:`方法回调之后，才能给用户发放奖励</font>

<br>

### 加载回调

推荐使用`[[UPRewardWrapper shareInstance] isReady]`来判断广告是否加载成功。

但如果明确需要广告加载结果回调，我们提供以下方式获取加载回调

```
// 在对应类中声明UPRewardLoadDelegate协议，并调用加载方法
[[UPRewardWrapper shareInstance] load:self];
```

```
// 实现加载成功协议方法
- (void)UPRewardVideoAdDidLoad {
    NSLog(@"激励视频广告 加载成功");
}
// 实现加载失败协议方法
- (void)UPRewardVideoAdDidLoadFail {
    NSLog(@"激励视频广告 加载失败");
}
```
