## 插屏广告
### 1. 插屏广告加载回调
插屏广告需要根据广告位来设置加载回调，此API用于监听插屏广告的加载结果。此接口一旦完成回调，内部会自动释放，再次监听时需要重新设置回调接口。
```javascript
// 根据广告位，设置某个插屏广告加载回调接口
// 用于监听插屏广告的加载结果（成功或失败）
// 此接口一旦回调，内部会自动释放，再次监听时需要重新设定回调接口
setInterstitialLoadCallback : function(cpPlaceId, loadsuccess, locadfail)
```

示例代码：
```javascript
var ilLoadCallUIButton = this.createButton(x, y, "ilLoadCall");
ilLoadCallUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.setInterstitialLoadCallback(cpPlaceId,
            function(cpid, msg) {
                console.log("===> js il load callback success: %s at placementid:%s", msg, cpid);
            },
            function(cpid, msg) {
                console.log("===> js il load callback fail: %s at placementid:%s", msg, cpid);
            });
    }
}, this);
```

### 2. 插屏广告展示回调
设置插屏广告展示时的回调接口，用于监听插屏广告的在某次展示时诸如点击，关闭等回调事件。插件展示回调接口的引用会被内部保存，不会释放，因此不用多次设置。
```javascript
// 设置插屏广告展示时的回调接口，用于监听插屏广告的在某次展示时诸如点击，关闭等事件回调
// 插件展示回调接口的引用会被内部保存，不会释放
// 回调接口功能顺序：展示回调，点击回调，关闭回调
// 回调接口参数：事件类型，广告位，showCall(type, cpPlaceId)
setInterstitialShowCallback : function(cpPlaceId, showCall)
```
示例代码：
```javascript
var ilShowCallUIButton = this.createButton(x, y, "ilShowCall");
ilShowCallUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.setInterstitialShowCallback(cpPlaceId, 
            function(type, cpid) {
                var event = "unkown";
                if (type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_SHOW) {
                    event = "Did_Show";
                }
                else if (type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_CLICK) {
                    event = "Did_Click";
                }
                else if (type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_CLOSE) {
                    event = "Did_Close";
                }
            }
        );
    }
}, this);
```

### 3. 判断插屏广告加载状态
根据广告位，判断某个插屏广告是否准备就绪，并同步返回bool结果，true 表示广告准备就绪可以展示，false表示广告还在请求中无法展示。
```javascript
// 根据广告位，判断某个插屏广告是否准备就绪
// 同步返回boolean结果，true 表示广告准备就绪可以展示，false表示广告还在请求中无法展示
// 参数cpPlaceId：广告位
isInterstitialReady : function(cpPlaceId)
```

示例代码：
```javascript
var ilReadyAsynUIButton = this.createButton(x, y, "ilReadyAsyn");
ilReadyAsynUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.isInterstitialReadyAsyn(cpPlaceId, function(r) {
            console.log("===> js il ad isreadyasyn: %s at placementid:%s", r, cpPlaceId);
        });
    }
}, this);
```

### 4. 展示插屏广告
根据广告位，展示相应的插屏广告。
```javascript
// 根据广告位，展示某个插屏广告
showInterstitialAd : function(cpPlaceId)
```

示例代码：
```javascript
var ilShowUIButton = this.createButton(x, y, "ilShow");
ilShowUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        var r = upltv.isInterstitialReady(cpPlaceId);
        if (r == true) {
            upltv.showInterstitialAd(cpPlaceId);
        }
    }
}, this);
```

### 5. 插屏广告调试模式
为了方便开发者查看广告的使用和加载状态，我们提供了插屏广告的调试页面，可以通过以下方法打开此界面观察插屏广告的配置参数与加载状态。
```javascript
showInterstitialDebugUI : function()
```
示例代码：
```javascript
var ilShowUIButton = this.createButton(x, y, "ilShowUI");
ilShowUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.showInterstitialDebugUI();
    }
}, this);
```