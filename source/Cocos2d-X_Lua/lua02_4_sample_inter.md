## 插屏广告
### 1. 插屏广告加载回调
插屏广告需要根据广告位来设置加载回调，此API用于监听插屏广告的加载结果。此接口一旦完成回调，内部会自动释放，再次监听时需要重新设置回调接口。
```lua
-- 根据广告位，判断某个插屏广告是否准备就绪
-- 异步返回bool结果，true 表示广告准备就绪可以展示，false表示广告还在请求中无法展示
-- 参数cpPlaceId：广告位
-- 参数callback：回调接口，如callback(true) 或 callback(false)
upltv:isInterstitialReadyAsyn(cpPlaceId, callback)
```
示例代码：
```lua
local btnInterstitialShow = createBtnAt(self, left, top, fontsize, "iLReadyAsyn")
btnInterstitialShow:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:isInterstitialReadyAsyn(ilPlaceId, function(r)
            if r == true then
                print("=====> Interstitial is ready")
            else
                print("=====> Interstitial is not ready")
            end
        end)
    end
end)
```
### 2. 插屏广告展示回调
设置插屏广告展示时的回调接口，用于监听插屏广告的在某次展示时诸如点击，关闭等回调事件。插件展示回调接口的引用会被内部保存，不会释放，因此不用多次设置回调接口。
```lua
-- 设置插屏广告展示时的回调接口，用于监听插屏广告的在某次展示时诸如点击，关闭等事件回调
-- 插件展示回调接口的引用会被内部保存，不会释放
-- 回调事件类型：展示，点击，关闭
-- 回调接口参数：回调事件，广告位，如showCall(type, cpPlaceId)调用
upltv:setInterstitialShowCallback(cpPlaceId, showCall)
```
示例代码：
```lua
local btnInterstitialShowCall = createBtnAt(self, left, top, fontsize, "iLShowCall")
btnInterstitialShowCall:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local function ilShowCall(type, cpadid)
            local event = "unkown"
            if type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_SHOW then
                event = "Did_Show"
            elseif type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_CLICK then
                event = "Did_Click"
            elseif type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_CLOSE then
                event = "Did_Close"
            end
            print("=====> Interstitial ShowCall Event:" .. event ..", at :" .. cpadid)
        end
        upltv:setInterstitialShowCallback(ilPlaceId, ilShowCall)
    end
end)
```
### 3. 判断插屏广告加载状态
根据广告位，判断某个插屏广告是否准备就绪，并同步返回bool结果，true 表示广告准备就绪可以展示，false表示广告还在请求中无法展示。
```lua
upltv:isInterstitialReady(cpPlaceId)
```
示例代码：
```lua
local btnInterstitialShow = createBtnAt(self, left, top, fontsize, "iLReadyAsyn")
btnInterstitialShow:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:isInterstitialReadyAsyn(ilPlaceId, function(r)
            if r == true then
                print("=====> Interstitial is ready")
            else
                print("=====> Interstitial is not ready")
            end
        end)
    end
end)
```
### 4. 展示插屏广告
根据广告位，展示相应的插屏广告。
```lua
upltv:showInterstitialAd(cpPlaceId)
```
示例代码：
```lua
local btnInterstitialShow = createBtnAt(self, left, top, fontsize, "iLShow")
btnInterstitialShow:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        if upltv:isInterstitialReady(ilPlaceId) == true then
            print("=====> Interstitial is ready")
            upltv:showInterstitialAd(ilPlaceId)
        else
            print("=====> Interstitial is not ready")
        end
    end
end)
```
### 5. 插屏广告调试模式
为了方便开发者查看广告的使用和加载状态，我们提供了插屏广告的调试页面，可以通过以下方法打开此界面观察插屏广告的配置参数与加载状态。
```lua
upltv:showInterstitialDebugUI()
```
示例代码：
```lua
local btnshowInterActivity = createBtnAt(self, left, top, fontsize, "iLDebugUI")
btnshowInterActivity:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:showInterstitialDebugUI()
    end
end)
```