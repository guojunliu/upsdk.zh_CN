## A/B Test接口调用

### 1. A/B Test初始化
进行A/B测试时，请在初始化SDK之后，调用此方法完成A/B Test初始化。
```lua
-- 进行A/B测试时，先初始化广告配置
-- 参数 gameAccountId ：        string类型， 用户在游戏中的帐号id（必填）
-- 参数 isCompleteTask  ：      boolean类型，是否完成了游戏中的新手任务
-- 参数 isPaid ：               number类型，是否付费用户，0则未付费
-- 参数 promotionChannelName ： string类型，推广渠道，没有可以传 ""
-- 参数 gender ：               string类型，"M", "F"，未知可以传""
-- 参数 age ：                  number类型，未知可以传-1
-- 参数 tags ：                 table类型（string数组），标签，没有可以传 nil
upltv:initAbtConfigJson(gameAccountId, isCompleteTask, isPaid, promotionChannelName, gender, age, tags)
```
示例代码：
```lua
local btninitAbtConfigJson = createBtnAt(self, left, top, fontsize, "initABtest")
btninitAbtConfigJson:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:initAbtConfigJson("u89731", true, 0, "Facebook", "M", -1, {"This is the first element.", "The second one.", "The last one."})
    end
end)
```

### 2. 获取A/B Test的结果
完成A/B Test初始化后，通过此方法获取结果。
```lua
-- 完成A/B Testing初始化后，通过此方法获取结果
-- 为了保证正确获取结果，调用时间建议延后initAbtConfigJson() 2秒以上
-- 参数 cpPlaceId ：        string类型， 广告位
upltv:getAbtConfig(cpPlaceId)
```
示例代码：
```lua
local btngetAbtConfigJson = createBtnAt(self, left, top, fontsize, "getAbtConfig")
btngetAbtConfigJson:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local r = upltv:getAbtConfig("pass")
        print("=====> getAbtConfig r:" .. type(r))
    end
end)
```