## SDK初始化

仅以Xcode工程作示例讲解，若你使用的是其它工程请参考Xcode工程操作，若有不便敬请谅解。

在UPSDK所有API接口中，初始化API最早被调用，即只有初始化UPSDK之后，才能正常使用UPSDK所支持的广告功能。

### 在工程中添加如下引用
```lua
local upltv = require "src.app.views.UPLTV"
```
###  初始化UPSDK
```lua
-- 初始化广告sdk
-- 参数zone：产品发行的区域，0:海外，1:中国大陆，2:自动根据ip定位
-- 可选参数callback：初始的回调接口, 接口定义 callback(boolean)，true表示成功，否则失败
upltv:initSDK(zone, ...) 
```
示例代码：
```lua
local btn = createBtnAt(self, left, top, fontsize, "init")
btn:addTouchEventListener(function(sender, eventType)
    if (2 == eventType) then
       local zone = 0
       local callback = function(r)
            if r == true then
               print("===> initSDK success")
           else
               print("===> initSDK fail")
           end
        end
        upltv:initSDK(zone, callback)
    end
end)
```

### setCustomerId
仅Android支持，对于非GP的包，可以传androidid，避免用户新增统计错误。
```lua
-- 请在初始化SDK之前调用
-- Version 3004(subversion 5) and above support this method
-- @param androidid
upltv:setCustomerId(androidid)
```

###  UPSDK前后台控制
对于Android平台，我们强烈要求在当前游戏的前台后切换时，调用UPSDK以下两个接口，以便UPSDK内部做出正确的响应，避免不必要的出错。

#### 游戏恢复到前台时调用API
```java
UPAdsSdk.onApplicationResume();
```
#### 游戏恢复到后台时调用API
```java
UPAdsSdk.onApplicationPause();
```

示例代码：

通常Cocos2d-X引擎生成的游戏Activity，继承于org.cocos2dx.lib.Cocos2dxActivity的子类。在Demo工程中，此类是AppActivity。

```java
@Override
protected void onResume() {
    super.onResume();
    UPAdsSdk.onApplicationResume();
}

@Override
protected void onPause() {
    super.onPause();
    UPAdsSdk.onApplicationPause();
}
```


