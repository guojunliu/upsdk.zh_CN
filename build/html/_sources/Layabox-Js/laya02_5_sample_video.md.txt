## 激励视频

### 1. 激励视频加载回调
  该API用于监听当前激励视频的加载结果，此接口一旦回调，内部会自动释放，再次监听时需要重新设定回调接口。
```javascript
// 设置激励视频加载回调接口
// 用于监听当前激励视频的加载结果（成功或失败）
// 此接口一旦回调，内部会自动释放，再次监听时需要重新设定回调接口
setRewardVideoLoadCallback : function(loadsuccess, locadfail)
```

示例代码：
```javascript
var rdLoadCallButton = this.createButton(x, y, "rdLoadCall");
rdLoadCallButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.setRewardVideoLoadCallback(function(cpid, msg) {
            console.log("===> js RewardVideo LoadCallback Success at: %s", cpid);
        }, function(cpid, msg) {
            console.log("===> js RewardVideo LoadCallback Fail at: %s", cpid);
        });
    }
}, this);
```

### 2. 激励视频展示回调
  设置激励视频展示回调接口，用于监听激励视频广告的在某次展示时(如点击，关闭，奖励发放等)事件回调。激励视频展示回调接口的引用会被内部保存，不会释放，因此只须设置一次。
```javascript
// 设置激励视频展示回调接口，用于监听激励视频广告的在某次展示时诸如点击，关闭，奖励发放等事件回调
// 展示接口的引用会被内部保存，不会释放
// 回调接口功能顺序：展示回调，点击回调，关闭回调，激励发放成功回调，激励发放失败回调
// 回调接口参数：事件类型，广告位，showCall(type, cpadid)
setRewardVideoShowCallback : function(showCall)
```

示例代码：
```javascript
var rdShowCallButton = this.createButton(x, y, "rdShowCall");
rdShowCallButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.setRewardVideoShowCallback(function(type, cpid) {
            var event = "unkown";
            if (type == upltv.AdEventType.VIDEO_EVENT_DID_SHOW) {
                event = "Did_Show";
            }
            else if (type == upltv.AdEventType.VIDEO_EVENT_DID_CLICK) {
                event = "Did_Click";
            }
            else if (type == upltv.AdEventType.VIDEO_EVENT_DID_CLOSE) {
                event = "Did_Close";
            }else if (type == upltv.AdEventType.VIDEO_EVENT_DID_GIVEN_REWARD) {
                event = "Did_Given_Reward";
            }else if (type == upltv.AdEventType.VIDEO_EVENT_DID_ABANDON_REWARD) {
                event = "Did_Abandon_Reward";
            }
        });
    }
}, this);
```

### 3. 判断激励视频加载状态
判断激励视频是否加载成功，并同步返回boolean结果，true表示广告准备就绪可以展示，false表示广告还在请求中无法展示。
```javascript
// 判断激励视频是否准备好
// 同步返回boolean结果，true 表示广告准备就绪可以展示，false表示广告还在请求中无法展示
// 通常在showRewardVideo(cpPlaceId)前，调用此方法
isRewardReady : function()
```

示例代码：
```javascript
var readyRdUIButton = this.createButton(x, y, "rdReady");
readyRdUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        var r = upltv.isRewardReady();
        console.log("===> js isRewardReady r: %s", r.toString());
    }
}, this);
```

### 4. 展示激励视频
在激励视频展示的时候，需要上传一个cpPlaceId，这是激励视频的广告位，用于业务打点，以便于区分收益来源。
```javascript
// 参数cpPlaceId：激励视频展示时的广告位，用于业务打点，便于区分收益来源
showRewardVideo : function(cpPlaceId)
```

示例代码：
```javascript
var showRdUIButton = this.createButton(x, y, "rdShow");
showRdUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        var r = upltv.isRewardReady();
        if (r == true) {
            upltv.showRewardVideo();
        }
    }
}, this);
```

### 5. 激励视频调试模式
为了方便开发者查看广告的使用和加载状态，我们提供了激励视频的调试页面，可以通过以下方法打开此界面观察激励视频的配置参数与加载状态。
```javascript
// 打开激励视频的debug界面
showRewardDebugUI : function()
```

示例代码：
```javascript
var showRdUIButton = this.createButton(x, y, "rdDebugUI");
showRdUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.showRewardDebugUI();
    }
}, this);
```

