## 横幅广告

横幅广告分为顶部banner和底部banner，JavascriptPlugin进一步简化了横幅banner广告的实现，提供有展示，隐藏，移除以及事件回调等接口。

### 1. banner广告的回调
banner广告需要根据广告位来设置banner广告的展示，点击以及移除等事件的回调接口。回调接口会被插件内部保存，因此不需要多次设置，回调接口会被保存只有调用upltv:removeBannerAdAt(cpPlaceId)才会被删除。
```javascript
// 参数cpPlaceId：banner广告位
// 参数bannerCall：banner回调接口
// 回调接口参数：事件类型，广告位，bannerCall(type, cpPlaceId)
setBannerShowCallback : function(cpPlaceId, bannerCall)
```
示例代码：
```javascript
var bnCallButton = this.createButton(x, y, "SetBNCall");
bnCallButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        var bannerCall = function(type, cpadid) {
            var event = "unkown";
            if (type == upltv.AdEventType.BANNER_EVENT_DID_SHOW) {
                event = "Did_Show";
            }
            else if (type == upltv.AdEventType.BANNER_EVENT_DID_CLICK) {
                event = "Did_Click";
            }
            else if (type == upltv.AdEventType.BANNER_EVENT_DID_REMOVED) {
                event = "Did_Removed";
            }
        };

        upltv.setBannerShowCallback(topCpId, bannerCall);
        upltv.setBannerShowCallback(bottomCpId, bannerCall);
    }
}, this);
```

### 2. 展示顶部banner广告
根据广告位，将banner展示在屏幕的顶部。
```javascript
// 将某个广告位的banner广告展示在屏幕顶部
showBannerAdAtTop : function(cpPlaceId)
```

示例代码：
```javascript
var bnTopButton = this.createButton(x, y, "TopBNShow");
bnTopButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.showBannerAdAtTop(topCpId);
    }
}, this);
```

**需要注意一点，对Iphonex手机顶部Banner被状态栏遮挡时，可以通过调节顶部Banner的偏移值，解决此问题。**
```javascript
/**
* @param padding: 顶部Banner的偏移值，如32，则状态样会向下偏移32像素
* Android平台不支持此功能
* supported from 3002
*/
setTopBannerPadingForIphoneX : function(padding);
```
### 3. 隐藏顶部banner广告
隐藏当前屏幕顶端的banner广告。
```javascript
// 隐藏当前屏幕的顶部banner广告
hideBannerAdAtTop : function()
```

示例代码：
```javascript
var bnHideButton = this.createButton(x, y, "hideAllBN");
bnHideButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.hideBannerAdAtTop();
    }
}, this);
```

### 4. 展示底部banner广告
根据广告位，将banner展示在屏幕的顶部。
```javascript
// 将某个广告位的banner广告展示在屏幕底部
showBannerAdAtBottom : function(cpPlaceId)
```

示例代码：
```javascript
var bnBottomButton = this.createButton(x, y, "BottomBNShow");
bnBottomButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.showBannerAdAtBottom(bottomCpId);
    }
}, this);
```

### 5. 隐藏底部banner广告
隐藏当前屏幕底部的banner广告。
```javascript
// 隐藏当前屏幕的底部广告
hideBannerAdAtBottom : function()
```

示例代码：
```javascript
var bnHideButton = this.createButton(x, y, "hideAllBN");
bnHideButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.hideBannerAdAtBottom();
    }
}, this);
```

### 6. 移除banner广告
UPSDK支持移除某个广告位上的banner广告。
```javascript
// 移除某个广告位的banner广告
removeBannerAdAt : function(cpPlaceId)
```

示例代码：
```javascript
var bnRemoveButton = this.createButton(x, y, "removeAllBn");
bnRemoveButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.removeBannerAdAt(topCpId);
        upltv.removeBannerAdAt(bottomCpId);
    }
}, this);
```