## 横幅广告
横幅广告分为顶部banner和底部banner，LuaPlugin进一步简化了横幅banner广告的实现，提供有展示，隐藏，移除以及事件回调等接口。

### 1. banner广告的回调
banner广告需要根据广告位来设置banner广告的展示，点击以及移除等事件的回调接口。回调接口会被插件内部保存，因此不需要多次设置，只有调用upltv:removeBannerAdAt(cpPlaceId)才会被删除。

```lua
-- 设置某个banner广告位的展示的回调接口，回调接口会被保存只有调用upltv:removeBannerAdAt(cpPlaceId)才会被删除
-- 参数cpPlaceId：banner广告位
-- 参数showCall：广告展示，点击跳转或移除时的回调
-- 回调参数：事件类型，广告位，如showCall(type, cpPlaceId)
 upltv:setBannerShowCallback(cpPlaceId, showCall)
```
示例代码：
```lua
local btnsetbannercall = createBtnAt(self, left, top, fontsize, "bannerCall")
btnsetbannercall:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local function bannerShowCall(type, cpadid)
            local event = "unkown"
            if type == upltv.AdEventType.BANNER_EVENT_DID_SHOW then
                event = "Did_Show"
            elseif type == upltv.AdEventType.BANNER_EVENT_DID_CLICK then
                event = "Did_Click"
            elseif type == upltv.AdEventType.BANNER_EVENT_DID_REMOVED then
                event = "Did_Removed"
            end
            print("=====> banner ShowCall Event:" .. event ..", at :" .. cpadid)
        end
        upltv:setBannerShowCallback("BannerAd",bannerShowCall)
        upltv:setBannerShowCallback("banner_bbb",bannerShowCall)
    end
end)
```

### 2. 展示顶部banner广告
根据广告位，将Banner广告展示在屏幕的顶部。
```lua
-- 根据广告位‘cpPlaceId’，在当前屏幕顶部展示Banner广告
upltv:showBannerAdAtTop(cpPlaceId)
```

示例代码：
```lua
-- show top banner
top = top - disht
local btnshowtopbanner = createBtnAt(self, left, top, fontsize, "topBanner")
btnshowtopbanner:addTouchEventListener(function(sender,eventType)
    if  (2 == eventType) then
        upltv:showBannerAdAtTop("BannerAd")
    end
end)
```

**需要注意一点，对Iphonex手机顶部Banner被状态栏遮挡时，可以通过调节顶部Banner的偏移值，解决此问题。**
```lua
-- @param padding: 顶部Banner的偏移值，如50，则状态样会向下偏移50像素
-- Android平台不支持此功能
-- supported from 3003
upltv:setTopBannerPadingForIphonex(padding)
```
示例代码：
```lua
top = top - disht
local btnshowtopbanner = createBtnAt(self, left, top, fontsize, "topBanner")
btnshowtopbanner:addTouchEventListener(function(sender,eventType)
    if  (2 == eventType) then
        --顶部Banner广告下移50像素
        upltv:setTopBannerPadingForIphonex(50)
    end
end)
```

### 3. 隐藏顶部banner广告
隐藏当前屏幕顶端的banner广告。
```lua
-- 隐藏顶部Banner广告
upltv:hideBannerAdAtTop() 
```
示例代码：
```lua
local btnhideallbanner = createBtnAt(self, left, top, fontsize, "hideAll")
btnhideallbanner:addTouchEventListener(function(sender,eventType)
    if  (2 == eventType) then
        upltv:hideBannerAdAtTop()
    end
end)
```
### 4. 展示底部banner广告
根据广告位，将Banner广告展示在屏幕的底部。
```lua
-- 根据广告位，将Banner广告展示在屏幕底部
upltv:showBannerAdAtBottom(cpPlaceId)
```
示例代码：
```lua
local btnshowbottombanner = createBtnAt(self, left, top,fontsize, "bottomBanner")
btnshowbottombanner:addTouchEventListener(function(sender,eventType)
    if  (2 == eventType) then
        upltv:showBannerAdAtBottom("banner_bbb")
    end
end)
```
### 5. 隐藏底部banner广告
隐藏当前屏幕底部的Banner广告。
```lua
upltv:hideBannerAdAtBottom()
```
示例代码：
```lua
local btnhideallbanner = createBtnAt(self, left, top, fontsize, "hideAll")
btnhideallbanner:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:hideBannerAdAtBottom()
    end
end)
```
### 6. 移除banner广告
UPSDK支持移除某个广告位上的Banner广告。
```lua
upltv:removeBannerAdAt(cpPlaceId)
```
示例代码：
```lua
local btnremoveallbanner = createBtnAt(self, left, top, fontsize, "removeAll")
btnremoveallbanner:addTouchEventListener(function(sender,eventType)
    if  (2 == eventType) then
        upltv:removeBannerAdAt("banner_bbb")
        upltv:removeBannerAdAt("BannerAd")
    end
end)
```