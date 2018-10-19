## 插屏广告

### 1. 插屏广告加载回调
插屏广告需要根据广告位来设置加载回调，此API用于监听插屏广告的加载结果。此接口一旦完成回调，内部会自动释放，再次监听时需要重新设置回调接口。
```cpp
/**
* @param cpPlaceId   广告位
* @param loadSuccess 加载成功的回调接口, loadSuccess(cpid, message)
* @param loadFail    加载失败的回调接口, loadFail(cpid, message)
*/
static void setInterstitialLoadCallback(const char* cpPlaceId, UpltvSdkStringCallback_2 loadSuccess, UpltvSdkStringCallback_2 loadFail);
```
示例代码：
```cpp
void iLLoadCallSuccess(const char *cpid, const char *msg) {
    log("====> iLLoadCallSuccess at cpid: %s", (cpid != nullptr ? cpid : ""));
}

void iLLoadCallFail(const char *cpid, const char *msg) {
    log("====> iLLoadCallFail  at cpid: %s", (cpid != nullptr ? cpid : ""));
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        string ilkey = "Interstitial_LevelPass";//定义插屏的广告位
        switch (tag)
        {
            case 1001:
            {
                 UpltvBridge::setInterstitialLoadCallback(ilkey.c_str(), iLLoadCallSuccess, iLLoadCallFail);
            }
                break;
        }
   }
}
```
### 2. 插屏广告展示回调
设置插屏广告展示时的回调接口，用于监听插屏广告的在某次展示时诸如点击，关闭等回调事件。插件展示回调接口的引用会被内部保存，不会释放，因此不用多次设置。
```cpp
/**
* 回调接口调用方式，callback(type, cpid)，type表示事件类型，有：展示回调，点击回调，关闭回调三种类型
* @param cpPlaceId
* @param callback
*/
static void setInterstitialShowCallback(const char* cpPlaceId, UpltvSdkStringCallback_1 callback);
```
示例代码：
```cpp
void iLShowCallback(UpltvAdEventEnum::AdEventType type, const char *cpid) {
    string s = "unkown";
    switch (type) {
        case UpltvAdEventEnum::AdEventType::INTERSTITIAL_EVENT_DID_SHOW:{
            s = "Did_Show";
        }
            break;
        case UpltvAdEventEnum::AdEventType::INTERSTITIAL_EVENT_DID_CLICK:{
            s = "Did_Click";
        }
            break;
        case UpltvAdEventEnum::AdEventType::INTERSTITIAL_EVENT_DID_CLOSE:{
            s = "Did_Close";
        }
            break;
    }
    log("====> interstitial ad showCall event: %s, at cpid: %s", (s.c_str() != nullptr ? s.c_str() : ""), cpid?cpid:"");
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        string ilkey = "Interstitial_LevelPass";//定义插屏的广告位
        switch (tag)
        {
            case 1001:
            {
                 UpltvBridge::setInterstitialShowCallback(ilkey.c_str(), iLShowCallback);
            }
                break;
        }
   }
}
```
### 3. 判断插屏广告加载状态
根据广告位，判断某个插屏广告是否准备就绪，并同步返回bool结果，true 表示广告准备就绪可以展示，false表示广告还在请求中无法展示。
```cpp
 /**
* @param cpPlaceId 广告位
* @return bool
*/
static bool isInterstitialReady(const char* cpPlaceId);
```
示例代码：
```cpp
void iLReadyCallback(const char* cpPlaceId, bool r) {
    log("====> cpp iLReadyCallback(%s, %s)", cpPlaceId, (r ? "true" : "false"));
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        string ilkey = "Interstitial_LevelPass";//定义插屏的广告位
        switch (tag)
        {
            case 1001:
            {
                UpltvBridge::isInterstitialReadyAsyn(ilkey.c_str(), iLReadyCallback);
            }
                break;
        }
   }
}
```
### 4. 展示插屏广告
根据广告位，展示相应的插屏广告。
```cpp
/**
* @param cpPlaceId 广告位
*/
static void showInterstitialAd(const char* cpPlaceId);
```
示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        string ilkey = "Interstitial_LevelPass";//定义插屏的广告位
        switch (tag)
        {
            case 1001:
            {
                if (UpltvBridge::isInterstitialReady(ilkey.c_str())) {
                    UpltvBridge::showInterstitialAd(ilkey.c_str());
                }
            }
                break;
        }
   }
}
```
### 5. 插屏广告调试模式
为了方便开发者查看广告的使用和加载状态，我们提供了插屏广告的调试页面，可以通过以下方法打开此界面观察插屏广告的配置参数与加载状态。
```cpp
static void showInterstitialDebugUI();
```
示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        string ilkey = "Interstitial_LevelPass";//定义插屏的广告位
        switch (tag)
        {
            case 1001:
            {
                UpltvBridge::showInterstitialDebugUI();
            }
                break;
        }
   }
}
```