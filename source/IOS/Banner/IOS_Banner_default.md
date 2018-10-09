## 横幅广告-快速集成

> 特别说明：以下【广告位】可由接入方向**我方技术支持同事询问**。不同广告使用场景请尽量保持【广告位】的独立性，方便未来区分不同广告展示的收益来源。  
> 例如：在暂停界面中初始化广告时，【广告位】可传入"Pause" or "Menu"  

#### 使用实例
如果仅仅需要在视图顶部或底部构建长条横幅banner广告时，我们推荐使用此构造方法，能更快更简洁地实现目标。

```objective-c
/*
* 参数说明
* 1. avidPlacement：不能为空，广告位，NSString类型，用于来标识广告的类型
* 2. vc：不能为空，UIViewController对象，用于控制Banner跳转
* 3. showLocation：为展示banner的位置（Top or Bottom），枚举如下
*/
- (instancetype)initWithPlacement:(NSString *)avidPlacement controller:(UIViewController*)vc showLocation:(UPStripShowLocationType)type;
```
> 此api仅被ios sdk2037及以上的版本支持

对于视图顶部或底部构的位置类型，构造时请使用以下枚举明确声明。
```objective-c
typedef NS_ENUM(NSUInteger, UPStripShowLocationType) {
    UPStripShowLocationTypeTop       = 1,        //顶部
    UPStripShowLocationTypeBottom    = 2,        //底部
};
```
- 使用UPStripShowLocationTypeTop，则banner会自动展示在viewController的顶部
- 使用UPStripShowLocationTypeBottom，则banner会自动展示在viewController的底部


使用示例：
在某个ViewController.m中，初始化一个长条的Banner对象（展示在top）并设置回调代理。
```objective-c
- (void)viewDidLoad {
	// …
	// 初始化wrapper对象
    _topStripBanner = [[UPBannerStripWrapper alloc] initWithPlacement:@"banner_strip” controller:self showLocation:UPStripShowLocationTypeTop];
	// 设置代理回调对象
    _topStripBanner.delegate = self;
	// …
}
```

此时showLocation设置为UPStripShowLocationTypeTop，长条banner会自动展示在ViewController的顶部，无需再调用其他代码

<br>

#### 内存回收：

`当banner视图所在的ViewController被销毁时(例如在界面中被移除，从nav中被pop，内存被回收)，请同时将banner对象置为nil，以回收内存`

<br>


