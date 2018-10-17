## A/B Test接口调用

### 1. A/B Test初始化
进行A/B测试时，请在初始化SDK之后，调用此方法完成A/B Test初始化。
```cpp
/**
* @param gameAccountId          const char*类型， 用户在游戏中的帐号id（必填）
* @param completeTask           bool类型，是否完成了游戏中的新手任务
* @param isPaid                 int类型，是否付费用户，0则未付费
* @param promotionChannelName   const char*类型，推广渠道，没有可以传 ""
* @param gender                 const char*，"M", "F"，未知可以传""
* @param age                    int类型，未知可以传-1
* @param tags                   vector类型，标签，没有可以传 nullptr
*/
static void initAbtConfigJson(const char* gameAccountId, bool completeTask, int isPaid,const char* promotionChannelName, const char* gender, int age, vector<string> *tags);
```
示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        switch (tag)
        {
            case 1001:
            {
                // 以下参数没有任何实际意义，仅作为演示如何调用API
                vector<string> tags = {"This is the first element.","The second one.","The last one."};
                UpltvBridge::initAbtConfigJson("u89731", true, 0, "Facebook", "", -1, &tags);
            }
                break;
        }
   }
}
```
### 2. 获取A/B Test的结果
完成A/B Test初始化后，通过此方法获取结果。
```cpp
/**
* 为了保证正确获取结果，调用时间建议延后initAbtConfigJson() 2秒以上
* @param cpPlaceId  string类型， 广告位
* @return const char*
*/
static const char* getAbtConfig(const char* cpPlaceId);
```
示例代码：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        switch (tag)
        {
            case 1001:
            {
                // 获取config配置
                string r = UpltvBridge::getAbtConfig("pass");
                log("===> cpp getABtConfig r :%s",r.c_str());
            }
                break;
        }
   }
}
```