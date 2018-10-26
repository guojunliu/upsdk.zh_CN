## 激励视频

### 1. 激励视频加载回调
  该API用于监听当前激励视频的加载结果，此接口一旦回调，内部会自动释放，再次监听时需要重新设定回调接口。
```lua
-- 设置激励视频加载回调接口
-- 用于监听当前激励视频的加载结果（成功或失败）
-- 此接口一旦回调，内部会自动释放，再次监听时需要重新设定回调接口
upltv:setRewardVideoLoadCallback(loadsuccess, locadfail)
```
示例代码：
```lua
local btnshowvideoActivity = createBtnAt(self, left, top, fontsize, "rdLoadCall")
btnshowvideoActivity:addTouchEventListener(function(sender, eventType)
    if  (2== eventType) then
        upltv:setRewardVideoLoadCallback(function(cpadid,msg)
            print("=====>RewardVideoLoadCallback success at " .. cpadid)         
        end,
        function(cpadid, msg)
            print("=====> RewardVideoLoadCallback fail at " .. cpadid .. " because of " .. msg) 
        end)
    end
end)
```
### 2. 激励视频展示回调
  设置激励视频展示回调接口，用于监听激励视频广告的在某次展示时(如点击，关闭，奖励发放等)事件回调。激励视频展示回调接口的引用会被内部保存，不会释放，因此只须设置一次。
```lua
-- 设置激励视频展示回调接口，用于监听激励视频广告的在某次展示时诸如点击，关闭，奖励发放等事件回调
-- 展示接口的引用会被内部保存，不会释放
-- 回调事件类型：展示，点击，关闭，激励发放成功，激励发放失败
-- 回调接口参数：事件类型，广告位，如showCall(type, cpadid)调用
upltv:setRewardVideoShowCallback(showCall)
```

示例代码：
```lua
local btnshowvideoActivity = createBtnAt(self, left, top, fontsize, "rdShowCall")
btnshowvideoActivity:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local function rewardShowCall(type, cpadid)
            local event = "unkown"
            if type == upltv.AdEventType.VIDEO_EVENT_DID_SHOW then
                event = "Did_Show"
            elseif type == upltv.AdEventType.VIDEO_EVENT_DID_CLICK then
                event = "Did_Click"
            elseif type == upltv.AdEventType.VIDEO_EVENT_DID_CLOSE then
                event = "Did_Close"
            elseif type == upltv.AdEventType.VIDEO_EVENT_DID_GIVEN_REWARD then
                event = "Did_Given_Reward"
            elseif type == upltv.AdEventType.VIDEO_EVENT_DID_ABANDON_REWARD then
                event = "Did_Abandon_Reward"
            end
            print("=====> RewardVideo show call Event:" .. event ..", at :" .. cpadid)
        end
        upltv:setRewardVideoShowCallback(rewardShowCall)
    end
end)
```
### 3. 判断激励视频加载状态
判断激励视频是否加载成功，并同步返回boolean结果，true表示广告准备就绪可以展示，false表示广告还在请求中无法展示。
  ```lua
-- 判断激励视频是否准备好
-- 同步返回boolean结果，true 表示广告准备就绪可以展示，false表示广告还在请求中无法展示
-- 通常在showRewardVideo(cpPlaceId)前，调用此方法
upltv:isRewardReady()
```

示例代码：
```lua
local btnloadvideo = createBtnAt(self, left, top, fontsize, "rdReady")
btnloadvideo:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local r = upltv:isRewardReady()
        if (r==true) then
            print("=====> btn up isRewardReady: true")
        else
            print("=====> btn up isRewardReady: false")
        end
    end
end)
```
### 4. 展示激励视频
在激励视频展示的时候，需要上传一个cpPlaceId，这是激励视频的广告位，用于业务打点，以便于区分收益来源。
```lua
upltv:showRewardVideo(cpPlaceId)
```
示例代码：
```lua
local btnloadvideo = createBtnAt(self, left, top, fontsize, "rdShow")
btnloadvideo:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local r = upltv:isRewardReady()
        if (r==true) then
            upltv:showRewardVideo("show_reward")
            print("=====> btn up isRewardReady: true")
        else
            print("=====> btn up isRewardReady: false")
        end
    end
end)
```
### 5. 激励视频调试模式
为了方便开发者查看广告的使用和加载状态，我们提供了激励视频的调试页面，可以通过以下方法打开此界面观察激励视频的配置参数与加载状态。
```lua
upltv:showRewardDebugUI()
```
示例代码：
```lua
local btnshowvideoActivity = createBtnAt(self, left, top, fontsize, "rdDebugUi")
btnshowvideoActivity:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:showRewardDebugUI()
    end
end)
```

