## 横幅广告
横幅广告分为顶部banner和底部banner，TypeScriptPlugin进一步简化了横幅banner广告的实现，提供有展示，隐藏，移除以及事件回调等接口。

### 1. banner广告的回调
banner广告需要根据广告位来设置banner广告的展示，点击以及移除等事件的回调接口。回调接口会被插件内部保存，因此不需要多次设置，回调接口会被保存只有调用upltv.removeBannerAdAt(cpPlaceId)才会被删除。
```javascript
//展示回调
    static bannerAdDidShow(callback:(cpPlaceId:string)=>void)
//点击回调
    static bannerAdDidClick(callback:(cpPlaceId:string)=>void)
//移除回调
    static bannerAdDidRemove(callback:(cpPlaceId:string)=>void)
```

### 2. 展示顶部banner广告
根据广告位，将banner展示在屏幕的顶部。
```javascript
// 将某个广告位的banner广告展示在屏幕顶部
    static showBannerAdAtTop(cpPlaceId:string)
```

**需要注意一点，对Iphonex手机顶部Banner被状态栏遮挡时，可以通过调节顶部Banner的偏移值，解决此问题。**
```javascript
/**
* @param padding: 顶部Banner的偏移值，如32，则状态样会向下偏移32像素
* Android平台不支持此功能
* supported from 3002
*/
static setTopBannerPading(padding)
```

### 3. 隐藏顶部banner广告
隐藏当前屏幕顶端的banner广告。
```javascript
// 隐藏当前屏幕的顶部banner广告
    static hideBannerAdAtTop()
```

### 4. 展示底部banner广告
根据广告位，将banner展示在屏幕的底部。
```javascript
// 将某个广告位的banner广告展示在屏幕底部
    static showBannerAdAtBottom(cpPlaceId:string)
```

### 5. 隐藏底部banner广告
隐藏当前屏幕底部的banner广告。
```javascript
// 隐藏当前屏幕的底部广告
    static hideBannerAdAtBottom()
```

### 6. 移除banner广告
UPSDK支持移除某个广告位上的banner广告。
```javascript
// 移除某个广告位的banner广告
    static removeBannerAdAt(cpPlaceId:string)
```