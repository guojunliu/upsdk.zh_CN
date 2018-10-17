## SDK初始化

仅以Xcode工程作示例讲解，若你使用的是其它工程请参考Xcode工程操作，若有不便敬请谅解。

UPLTV SDK的初始化非常简单。

### 在工程中添加如下引用
```cpp
#include "UpltvBridge.h"
```

###  初始化UPSDK
初始化的方式有两种，我们推荐第一种初始化：

#### 1. 根据区域参数初始化UPSDK
```cpp
/**
* 初始化upltv广告sdk
*/
static void initSdkByZone(int zone);
```
以下为示例代码：

```cpp
/**
* 请务必优先完成sdk初始化后，才能正常使用SDK的其它API接口
* @param zone 产品发行的区域，0海外，1中国大陆，2自动根据ip定位
*/
UpltvBridge::initSdkByZone(0);
```

#### 2. 根据区域参数及回调函数初始化UPSDK

如果想监听初始化结果是否成功，可以使用此方法。
```cpp
/**
* @param callback SDK初始化完成后的回调接口, 回调接口包含一个布尔参数 callback(boolean)，true表示成功，否则失败
*/
initSdkByZoneWithCall(int zone, UpltvSdkBoolCallback callback);
```
以下为示例代码：
```cpp
/**
*SDK初始化回调接口，参数true代表初始化成功，否则初始化失败
*/
void sdkCallback(bool r)
{
   log("====> cpp initSdkCallback(%s)", (r ? "true" : "false"));
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                //初始化SDK，并监听回调结果
                UpltvSdkBoolCallback callback = sdkCallback;
                UpltvBridge::initSdkByZoneWithCall(0, callback);
            }
                break;

        }
   }
}

```

#### 3. setCustomerId()
仅Android支持，对于非GP的包，可以传androidid，避免用户新增统计错误。
```asp
/**
* 请在初始化SDK之前调用
* Version 3004(subversion 5) and above support this method
* @param androidid
*/
UpltvBridge::setCustomerId(androidid);
```

#### 4. onApplicationFocus()添加
请参考以下代码在当前工程的AppDelegate类中正确添加onApplicationFocus()方法，以便通知UPSDK游戏前台切换变化。

```cpp
void AppDelegate::applicationDidEnterBackground() {
    // 进入后台时，调用UpltvBridge::onApplicationFocus(false)
    UpltvBridge::onApplicationFocus(false);

    Director::getInstance()->stopAnimation();
#if USE_AUDIO_ENGINE
    AudioEngine::pauseAll();
#elif USE_SIMPLE_AUDIO_ENGINE
    SimpleAudioEngine::getInstance()->pauseBackgroundMusic();
    SimpleAudioEngine::getInstance()->pauseAllEffects();
#endif
}

// this function will be called when the app is active again
void AppDelegate::applicationWillEnterForeground() {
    // 进入前台时，调用UpltvBridge::onApplicationFocus(true)
    UpltvBridge::onApplicationFocus(true);

    Director::getInstance()->startAnimation();
#if USE_AUDIO_ENGINE
    AudioEngine::resumeAll();
#elif USE_SIMPLE_AUDIO_ENGINE
    SimpleAudioEngine::getInstance()->resumeBackgroundMusic();
    SimpleAudioEngine::getInstance()->resumeAllEffects();
#endif
}
```