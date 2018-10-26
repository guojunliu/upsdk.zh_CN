## 激励视频

#### 1. 激励视频加载回调
  该API用于监听当前激励视频的加载结果，此接口一旦回调，内部会自动释放，再次监听时需要重新设定回调接口。
```cpp
/**
* 设置激励视频加载回调接口
* @param successCall 激励视频加载成功时回调，successCall(cpadid, msg)
* @param failCall    激励视频加载失败时回调，failCall(cpadid, msg)
*/
static void setRewardVideoLoadCallback(UpltvSdkStringCallback_2 successCall, UpltvSdkStringCallback_2 failCall);
```
示例代码：
```cpp
// 激励视频加载成功时回调此方法
void rdLoadCallSuccess(const char *cpid, const char *msg) {
    log("====> reward video didLoad Success at cpid: %s", (cpid != nullptr ? cpid : ""));
}

// 激励视频加载失败时回调方法
void rdLoadCallFail(const char *cpid, const char *msg) {
    log("====> reward video didLoad Fail at cpid: %s", (cpid != nullptr ? cpid : ""));
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                // 设置激励视频加载回调接口
                UpltvSdkStringCallback_2 loadSuccess = rdLoadCallSuccess;
                UpltvSdkStringCallback_2 loadFail = rdLoadCallFail;
                UpltvBridge::setRewardVideoLoadCallback(loadSuccess, loadFail);
            }
                break;
        }
   }
}
```
#### 2. 激励视频展示回调
  设置激励视频展示回调接口，用于监听激励视频广告的在某次展示时(如点击，关闭，奖励发放等)事件回调。激励视频展示回调接口的引用会被内部保存，不会释放，因此只须设置一次。
```cpp
/**
* @param callback(type, cpid)
*/
static void setRewardVideoShowCallback(UpltvSdkStringCallback_1 callback);
```

示例代码：
```cpp
// 激励视频展示时，回调此方法，监听事件类型
void rdShowCallback(UpltvAdEventEnum::AdEventType type, const char *cpid) {
    string s = "unkown";
    switch (type) {
        case UpltvAdEventEnum::AdEventType::VIDEO_EVENT_DID_SHOW:{
            s = "Did_Show";
        }
            break;
        case UpltvAdEventEnum::AdEventType::VIDEO_EVENT_DID_CLICK:{
            s = "Did_Click";
        }
            break;
        case UpltvAdEventEnum::AdEventType::VIDEO_EVENT_DID_CLOSE:{
            s = "Did_Close";
        }
            break;
        case UpltvAdEventEnum::AdEventType::VIDEO_EVENT_DID_GIVEN_REWARD:{
            s = "Did_Give";
        }
            break;
        case UpltvAdEventEnum::AdEventType::VIDEO_EVENT_DID_ABANDON_REWARD:{
            s = "Did_Abandon";
        }
            break;
    }
    log("====> reward video showCall event: %s, at cpid: %s", (s.c_str() != nullptr ? s.c_str() : ""), cpid?cpid:"");

}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                {
                //设置激励视频展示回调接口，用于监听激励视频广告的在某次展示时诸如点击，关闭，奖励发放等事件回调
                UpltvBridge::setRewardVideoShowCallback(rdShowCallback);
            }
                break;
        }
   }
}
```
#### 3. 判断激励视频加载状态
判断激励视频是否加载成功，并同步返回boolean结果，true表示广告准备就绪可以展示，false表示广告还在请求中无法展示。
  ```cpp
 /**
* 通常在showRewardVideo(cpPlaceId)前，调用此方法
* @return bool
*/
static bool isRewardReady();
```

示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                {
                // 判断激励视频是否加载好
                bool r = UpltvBridge::isRewardReady();
                log("====> cpp print isRewardReady : %s",(r?"true":"false"));
            }
                break;
        }
   }
}
```
#### 4. 展示激励视频
在激励视频展示的时候，需要上传一个cpPlaceId，这是激励视频的广告位，用于业务打点，以便于区分收益来源。
```cpp
/**
* @param cpPlaceId 激励视频展示时的广告位，用于业务打点，便于区分收益来源
*/
static void showRewardVideo(const char* cpPlaceId);

```
示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                {
               if (UpltvBridge::isRewardReady()) {
               UpltvBridge::showRewardVideo(nullptr);
                }
            }
                break;
        }
   }
}
```
#### 5. 激励视频调试模式
为了方便开发者查看广告的使用和加载状态，我们提供了激励视频的调试页面，可以通过以下方法打开此界面观察激励视频的配置参数与加载状态。
```cpp
static void showRewardVideoDebugUI();
```
示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                {
                  UpltvBridge::showRewardVideoDebugUI();
                }
            }
                break;
        }
   }
}
```

