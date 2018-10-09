## 横幅广告
横幅广告分为顶部banner和底部banner，CppPlugin进一步简化了横幅banner广告的实现，提供有展示，隐藏，移除以及事件回调等接口。

### 1. banner广告的回调
banner广告需要根据广告位来设置banner广告的展示，点击以及移除等事件的回调接口。回调接口会被插件内部保存，因此不需要多次设置，只有调用upltv:removeBannerAdAt(cpPlaceId)才会被删除。

```cpp
/**
* @param cpPlaceId banner广告位
* @param callback
*/
static void setBannerShowCallback(const char* cpPlaceId, UpltvSdkStringCallback_1 callback);
```
示例代码：
```cpp
void bannerCallback(UpltvAdEventEnum::AdEventType type, const char *cpid) {
    string s = "unkown";
    switch (type) {
        case UpltvAdEventEnum::AdEventType::BANNER_EVENT_DID_SHOW:{
            s = "Did_Show";
        }
            break;
        case UpltvAdEventEnum::AdEventType::BANNER_EVENT_DID_CLICK:{
            s = "Did_Click";
        }
            break;
        case UpltvAdEventEnum::AdEventType::BANNER_EVENT_DID_REMOVED:{
            s = "Did_Removed";
        }
            break;
    }
    log("====> Banner ad call event: %s, at cpid: %s", (s.c_str() != nullptr ? s.c_str() : ""), cpid?cpid:"");
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannertopkey = "BannerAd"; //定义banner的广告位
        switch (tag)
        {
            case 1001:
            {
                UpltvBridge::setBannerShowCallback(bannertopkey, bannerCallback);
            }
                break;
        }
   }
}
```
### 2. 展示顶部banner广告
根据广告位，将banner展示在屏幕的顶部。
```cpp
/**
* @param cpPlaceId
*/
static void showBannerAdAtTop(const char*cpPlaceId);
```
示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannertopkey = "BannerAd"; //定义banner的广告位
        switch (tag)
        {
            case 1001:
            {
               UpltvBridge::showBannerAdAtTop(bannertopkey);
            }
                break;
        }
   }
}
```
**需要注意一点，对Iphonex手机顶部Banner被状态栏遮挡时，可以通过调节顶部Banner的偏移值，解决此问题。**
```cpp
/**
* @param padding: 顶部Banner的偏移值，如32，则状态样会向下偏移32像素
* Android平台不支持此功能
* supported from 3002
*/
static void setTopBannerPadingForIphonex(int padding);
```
### 3. 隐藏顶部banner广告
隐藏当前屏幕顶端的banner广告。
```cpp
static void hideBannerAdAtTop();
```
示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannertopkey = "BannerAd"; //定义banner的广告位
        switch (tag)
        {
            case 1001:
            {
              UpltvBridge::hideBannerAdAtTop();
            }
                break;
        }
   }
}
```
### 4. 展示底部banner广告
根据广告位，将banner展示在屏幕的顶部。
```cpp
/**
* @param cpPlaceId
*/
static void showBannerAdAtBottom(const char*cpPlaceId);
```
示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannerbottomkey = "BannerAd"; //定义banner的广告位
        switch (tag)
        {
            case 1001:
            {
              UpltvBridge::showBannerAdAtBottom(bannerbottomkey);
            }
                break;
        }
   }
}
```
### 5. 隐藏底部banner广告
隐藏当前屏幕底部的banner广告。
```cpp
static void hideBannerAdAtBottom();
```
示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannerbottomkey = "BannerAd"; //定义banner的广告位
        switch (tag)
        {
            case 1001:
            {
              UpltvBridge::hideBannerAdAtBottom();
            }
                break;
        }
   }
}
```
### 6. 移除banner广告
UPSDK支持移除某个广告位上的banner广告。
```cpp
/**
* @param cpPlaceId banner广告位
*/
static void removeBannerAdAt(const char*cpPlaceId);
```
示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannerbottomkey = "BannerAd"; //定义banner的广告位
        switch (tag)
        {
            case 1001:
            {
              UpltvBridge::removeBannerAdAt(bannerbottomkey);
            }
                break;
        }
   }
}
```