## 横幅广告-自定义

UPSDK提供两种横幅(Banner)广告的封装实现，一种是基于*UPBannerStripWrapper*类的长条形广告，另一种是基于*UPBannerRectangleWrapper*的矩形广告。使用时请根据实际情况，实例化广告相应的Wrapper类。接入过程，请参考如下步骤：

#### 引入Banner的头文件
Banner两个类型的头文件，分别是：UPBannerStripWrapper.h与UPBannerRectangleWrapper.h。头文件的引入方式如下：

```objective-c
#import   <UPSDK/UPSDK.h>
```

<br>

#### 实例化Wrapper对象

> 特别说明：以下【广告位】可由接入方向**我方技术支持同事询问**。不同广告使用场景请尽量保持【广告位】的独立性，方便未来区分不同广告展示的收益来源。  
> 例如：在暂停界面中初始化广告时，【广告位】可传入"Pause" or "Menu"  


#### 构造方法：
此方法构造的banner对象，可以自由控制banner view的显示位置，因此实现过程比构造方法多一此view的布局与控制操作。

```objective-c
/**
* 参数说明
* 1. avidPlacement：不能为空，广告位，NSString类型，用于来标识广告的类型
* 2. vc：不能为空，UIViewController对象，用于控制Banner跳转
*/
- (instancetype)initWithPlacement:(NSString *)avidPlacement controller:(UIViewController*)vc;
```

使用示例：
在某个ViewController.m中，初始化一个矩形的Banner对象并设置回调代理。
```objective-c
- (void)viewDidLoad {
	// …
	// 初始化wrapper对象
    _bottomRectBanner = [[UPBannerRectangleWrapper alloc] initWithPlacement:@"banner_rect_bottom” controller:self];
	// 设置代理回调对象
    _bottomRectBanner.delegate = self;
	// 初始化一个用于显示banner广告的UIView，size未知
    _bannerRectView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 0, 0)];
    [self.view addSubview:_bannerRectView];
    // 获取广告的UIView对象，并装载
    UIView *view = [_bottomRectBanner getView];
    [_bannerRectView addSubview:view];
	// …
}
```


<br>

#### Banner回调协议
UPBannerWrapperProtocol是Banner回调监听协议，主要有以下两个接口回调。

```objective-c
- (void)bannerAdClick:(id)wrapper;

- (void)bannerAdDidShow:(id)wrapper size:(CGSize)size;
```

接口说明如下：

1.  -(void)bannerAdClick:(id)wrapper
用于监听banner是否被点击，当banner被用户点击跳转时，wrapper会给此接口发消息

2. -(void)bannerAdDidShow:(id)wrapper size:(CGSize)size;
当banner可被显示时，wrapper会给此接口发消息，参数size是当前被显示的banner的实际大小。通常通过此接口，完成banner的布局调整。

接口实现示例：
```objective-c
/**
 *  广告展示完成
 *
 *  @param wrapper 广告对象
 */
- (void)bannerAdDidShow:(id)wrapper size:(CGSize)size{
    // 判断wrapper对象是否是期望的对象
    if (_bottomRectBanner == wrapper){
        CGRect frame = _bannerRectView.frame;
        frame.size = size;
        // 让_bannerRectView水平居中，垂直靠底显示在父类中
        frame.origin.x = (self.view.frame.size.width - frame.size.width)/2;
        frame.origin.y = self.view.frame.size.height - frame.size.height;
        _bannerRectView.frame = frame;
        // _bannerRectView设置为显示
        _bannerRectView.hidden = NO;
    }
}


/**
 *  广告被点击
 *
 *  @param wrapper wrapper的实例对象对象
 */
- (void)bannerAdClick:(id)wrapper{
    // 示例中此处不重要
}
```

<br>

#### 内存回收：

`当banner视图所在的ViewController被销毁时(例如在界面中被移除，从nav中被pop，内存被回收)，请同时将banner对象置为nil，以回收内存`

<br>
