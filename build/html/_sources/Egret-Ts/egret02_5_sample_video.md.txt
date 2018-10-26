## 激励视频

### 1. 激励视频加载回调
  该API用于监听当前激励视频的加载结果，此接口一旦回调，内部会自动释放，再次监听时需要重新设定回调接口。
```javascript
// 获取激励视频加载结果
// 回调参数：加载结果（成功或失败，只有一个有值）
    static getRewardAdLoadResult(success:(cpPlaceId:string,message:string)=>void,failure:(cpPlaceId:string,message:string)=>void)
```

示例代码：
```javascript
upltv.getRewardAdLoadResult(function(cpid:string,msg:string){
//success
},function(cpid:string, msg:string){
//failure
});
```

### 2. 激励视频展示回调
  设置激励视频展示回调接口，用于监听激励视频广告的在某次展示时(如点击，关闭，奖励发放等)事件回调。激励视频展示回调接口的引用会被内部保存，不会释放，因此只须设置一次。
```javascript
// 展示回调
// 回调参数：广告位
    static rewardAdDidOpen(callback:(cpPlaceId:string)=>void)
//点击回调
    static rewardAdDidClick(callback:(cpPlaceId:string)=>void)
//关闭回调
    static rewardAdDidClose(callback:(cpPlaceId:string)=>void)
//激励发放成功回调
    static rewardAdDidGiven(callback:(cpPlaceId:string)=>void)
//激励发放失败回调
    static rewardAdDidAbandon(callback:(cpPlaceId:string)=>void)
```

### 3. 判断激励视频加载状态
判断激励视频是否加载成功，并同步返回boolean结果，true表示广告准备就绪可以展示，false表示广告还在请求中无法展示。
```javascript
// 回调参数：true 表示广告准备就绪可以展示，false表示广告还在请求中无法展示
// 通常在showRewardVideo(cpPlaceId)前，调用此方法
    static isRewardAdReady(callback:(ready:boolean)=>void)
```

### 4. 展示激励视频
在激励视频展示的时候，需要上传一个cpPlaceId，这是激励视频的广告位，用于业务打点，以便于区分收益来源。
```javascript
// 展示激励视频
// 参数cpPlaceId：激励视频展示时的广告位，用于业务打点，便于区分收益来源
    static showRewardAd(cpPlaceId:string)
```

### 5. 激励视频调试模式
为了方便开发者查看广告的使用和加载状态，我们提供了激励视频的调试页面，可以通过以下方法打开此界面观察激励视频的配置参数与加载状态。
```javascript
// 打开激励视频的debug界面
    static showRewardAdDebugUI()
```
