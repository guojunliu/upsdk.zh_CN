## 插屏广告
### 1. 插屏广告加载回调
插屏广告需要根据广告位来设置加载回调，此API用于监听插屏广告的加载结果。此接口一旦完成回调，内部会自动释放，再次监听时需要重新设置回调接口。
```javascript
// 获取插屏加载结果
// 回调参数：加载结果（成功或失败，只有一个有值）
    static getInterstitialAdLoadResult(cpPlaceId:string,
                                       success:(cpPlaceId:string,message:string)=>void,
                                       failure:(cpPlaceId:string,message:string)=>void)
```

示例代码：
```javascript
upltv.getInterstitialAdLoadResult("ilLoadCall",function(cpid:string,msg:string){
//success
},function(cpid:string,msg:string){
//failure
});
```

### 2. 插屏广告展示回调
设置插屏广告展示时的回调接口，用于监听插屏广告的在某次展示时诸如点击，关闭等回调事件。插件展示回调接口的引用会被内部保存，不会释放，因此不用多次设置。
```javascript
//展示回调
    static interstitialAdDidShow(callback:(cpPlaceId:string)=>void)
//点击回调
    static interstitialAdDidClick(callback:(cpPlaceId:string)=>void)
//关闭回调
    static interstitialAdDidClose(callback:(cpPlaceId:string)=>void)
```

### 3. 判断插屏广告加载状态
根据广告位，判断某个插屏广告是否准备就绪，并同步返回bool结果，true 表示广告准备就绪可以展示，false表示广告还在请求中无法展示。
```javascript
// 根据广告位，判断某个插屏广告是否准备就绪
// true 表示广告准备就绪可以展示，false表示广告还在请求中无法展示
// 参数cpPlaceId：广告位
// 参数callback：回调接口，如callback(true) 或 callback(false)
    static isInterstitialAdReady(cpPlaceId:string, callback:(ready:boolean)=>void)
```

### 4. 展示插屏广告
根据广告位，展示相应的插屏广告。
```javascript
// 根据广告位，展示某个插屏广告
    static showInterstitialAd(cpPlaceId:string)
```

### 5. 插屏广告调试模式
为了方便开发者查看广告的使用和加载状态，我们提供了插屏广告的调试页面，可以通过以下方法打开此界面观察插屏广告的配置参数与加载状态。
```javascript
// 打开插屏的debug界面
    static showInterstitialAdDebugUI()
```
