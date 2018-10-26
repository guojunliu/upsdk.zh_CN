## 插屏广告

### 插屏广告接入

有关插屏广告的方法与代理接口定义在UPIntersitialWrapper.h文件中。
#### 引用方式：

```objective-c
#import   <UPSDK/UPSDK.h>
```

#### 方法声明：
> 以下【广告位】可由接入方向**我方技术支持同事询问**。不同广告使用场景请尽量保持【广告位】的独立性，方便未来区分不同广告展示的收益来源。  
> 例如：在暂停界面中初始化广告时，【广告位】可传入"Pause" or "Menu"  

```objective-c
@interface UPIntersitialWrapper : NSObject

/**
* 初始化方法
* @ param avidPlacement：广告位
**/
- (instancetype)initAvidPlacement:(NSString *)avidPlacement;

/**
* 设置回调代理对象
* @ param delegate：UPIntersitialDelegate 代理对象
**/
- (void)setDelegate:(id<UPIntersitialDelegate>)delegate;

/**
* 判断视频广告是否可用
**/
- (BOOL)isReady;

/**
* 设置回调代理对象
* @ param viewController：UIViewController 对象，用于点击时跳转控制
**/
- (BOOL)show:(UIViewController *)viewController;

/**
 * 设置广告加载回调接口
 * @param delegate，回调代理
 **/
 - (void)load:(id<UPIntersitialLoadDelegate>)delegate;
@end
```
#### UPIntersitialDelegate 回调协议
插屏广告的回调协议并不是必须要实现的，根据需要自行设置。
```objective-c
@protocol UPIntersitialDelegate <NSObject>

/**
 * 插屏广告展示时，调用此方法
 * @ param wrapper ：消息发送者，UPIntersitialWrapper实例对象
 */
- (void)interstitialAdDidShow:(UPIntersitialWrapper *)wrapper;

/**
 * 插屏广告关闭，调用此方法
 * @ param wrapper 消息发送者，UPIntersitialWrapper实例对象
 */
- (void)interstitialAdDidClose:(UPIntersitialWrapper *)wrapper;

/**
 * 插屏广告点击，调用此方法
 * @ param wrapper 消息发送者，UPIntersitialWrapper实例对象
 */
- (void)interstitialAdDidClick:(UPIntersitialWrapper *)wrapper;

@end

```

### 示例代码
我们仅以XCode工程作为示例，其它IDE工程，请参考修改！

在XCode示例工程中，声明STInterstitialViewController类，用于展现插屏广告。 首先定义STInterstitialViewController.h类，由于仅用于插屏广告展示，所以代码非常简洁。

```objective-c
@interface STInterstitialViewController : UIViewController

@end
```

在STInterstitialViewController.m，只需要定义几行代码，就能完成插屏广告的加载与实现。

首先，定义一个UPIntersitialWrapper 对象：_intersitialWrapper，通过它实现插屏广告的加载与展现。原则上，一个插屏广告位对应一个UPIntersitialWrapper 对象。

代码如下：

```objective-c
@interface STInterstitialViewController () <UPIntersitialDelegate>
{
	//声明一个插屏wrapper对象，根据广告位数量，也可以声明多个
    UPIntersitialWrapper *_intersitialWrapper;
}
@end
```

然后添加如下代码：

```objective-c

- (void)viewDidLoad  {
    [super viewDidLoad];
    self.view.backgroundColor = [UIColor whiteColor];
    
	// 在viewDidLoad添加一个按钮测试插屏广告的加载与展现
    UIButton *button = [UIButton buttonWithType:UIButtonTypeRoundedRect];
    button.backgroundColor = [UIColor orangeColor];
    button.frame = CGRectMake(self.view.frame.size.width/2 - 250/2, 100, 250, 40);
    [button setTitle:@"测试插屏广告" forState:UIControlStateNormal];
	// 点击按钮时，会触发intersitialClick方法
    [button addTarget:self action:@selector(intersitialClick) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:button];
    
}

- (void)intersitialClick {
	// 假设"inter_bbb"为_intersitialWrapper对应的广告位
    _intersitialWrapper = [[UPIntersitialWrapper alloc] initAvidPlacement:@"inter_bbb"];
	// 设置回调代理
    [_intersitialWrapper setDelegate:self];
    
	//判断插屏广告是否可以展示
    if ([_intersitialWrapper isReady]) {
        [_intersitialWrapper show:self];
    }
    else
    {
        NSLog(@"Intersitial no ready");
    }
}
```

从代码上看，一个插屏广告的展现与控制非常简单，只需要三步：
1. 声明全局对象，例如：
```objective-c
 UPIntersitialWrapper *_intersitialWrapper;
```
2. 初始化对象
根据某个广告位，通常会在不同的展位位置设置不同标识值，因此这个值是预先声明可知的。如下示例，"inter_bbb"是某个真实存在的广告位。错误的广告位，会导致插屏广告加载失败。
```objective-c
_intersitialWrapper = [[UPIntersitialWrapper alloc] initAvidPlacement:@"inter_bbb"];
```
3. 控制广告展现
在需要展现的时候，如下添加代码。
```objective-c
if ([_intersitialWrapper isReady]) {
        [_intersitialWrapper show:self];
    }
```
