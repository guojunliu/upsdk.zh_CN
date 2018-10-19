## GDPR 
`GDPR《一般数据保护法案》`是欧盟出台的数据保护方案，如果您的产品会面向欧盟用户，我们提供如下方案确保`UPSDK`遵守`GDPR`规范。

`UPSDK`在`3.0.03`版本支持欧盟`GDPR`规范，发行区域包含欧盟或涵盖欧盟用户的开发者必须处理此逻辑。

### GDPR 推荐用例
#### 方案一
推荐根据自己游戏的画面风格自定义GDPR的授权弹框界面，保证产品体验最佳。
采用此方案时，仅需要将授权结果同步给UPSDK后再初始化UPSDK。

示例代码：
```csharp
local function yourOwnGDPRCallback(result)
    -- 如果result : "true" 表示用户接受授权，false拒绝授权
    -- 请参考以下代码完成UPSDK的授权同步与初始化
    if result=="true" then
        upltv:updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusAccepted)
    else
        upltv:updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusDefined)
    end
    -- 调用updateAccessPrivacyInfoStatus()之后再初始化UPSDK
    -- 假定发行地区是海外，且不需要回调
    upltv:initSDK(0)
end

local function europeanUnionUserCallBack(result)
    print("=====> lua mEuropeanUnionUserCallBack result: " .. result)
    -- result: "true" 表示欧盟地区用户，否则非欧盟地区用户
    if result=="true" then
        -- 欧盟地区用户，调用自定义授权询问方法
        -- callYourOwnGDPRDialog()，是我们假定的方法
        -- yourOwnGDPRCallback，是我们假定的授权回调方法
        -- 请根据实际代码替换
        callYourOwnGDPRDialog(yourOwnGDPRCallback)
    else
        -- 非欧盟地区用户，直接初始化SDK
        -- 假定发行地区是海外，且不需要回调
        upltv:initSDK(0)
    end
end

-- GDPR
local function checkGDPR()
    local e = upltv:getAccessPrivacyInfoStatus()
    print("=====> lua getAccessPrivacyInfoStatus status: " .. e)
    -- 如果没有询问过授权
    if e==upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusUnkown then
        -- 先定位用户是否是欧盟地区
        upltv:isEuropeanUnionUser(europeanUnionUserCallBack)
    else
        -- 假定发行地区是海外，且不需要回调
        upltv:initSDK(0)
    end
end
``` 

#### 方案二
如果采用UPSDK提供的标准授权处理机制，请参考以下代码修改UPSDK的初始化流程。

示例代码：

```csharp
local function notifyAccessPrivacyInfoStatusCallBack(value)
    print("=====> lua notifyAccessPrivacyInfoStatus callback: " .. value)
    upltv:initSDK(0)
 end

local function europeanUnionUserCallBack(result)
    print("=====> lua mEuropeanUnionUserCallBack result: " .. result)
    -- result: "true" 表示欧盟地区用户，否则非欧盟地区用户
    if result=="true" then
        -- 弹出系统弹窗询问
        upltv:notifyAccessPrivacyInfoStatus(notifyAccessPrivacyInfoStatusCallBack)
    else
        -- 非欧盟地区用户，直接初始化SDK
        -- 假定发行地区是海外，且不需要回调
        upltv:initSDK(0)
    end
end

-- GDPR
local function checkGDPR()
    local e = upltv:getAccessPrivacyInfoStatus()
    print("=====> lua getAccessPrivacyInfoStatus status: " .. e)
    -- 如果没有询问过授权
    if (e==upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusUnkown) then
        -- 先定位用户是否是欧盟地区
        upltv:isEuropeanUnionUser(europeanUnionUserCallBack)
    else
        -- 假定发行地区是海外，且不需要回调
        upltv:initSDK(0)
    end
end
```
### GDPR API介绍

#### 1.notifyAccessPrivacyInfoStatus
弹出授权窗口，向用户说明重要数据收集的情况并询问用户是否同意授权，如果用户拒绝授权将放弃相关数据的收集。请在初始化UPSDK之前调用。

```csharp
function upltv:notifyAccessPrivacyInfoStatus(callback)
```
示例代码：

```csharp
local mNotifyAccessPrivacyInfoStatusCallBack=function(value)
    print("=====> lua notifyAccessPrivacyInfoStatus callback: " .. value)
    if upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusAccepted==value then
          --同意

    else
          --不同意
          
    end
end  

upltv:notifyAccessPrivacyInfoStatus(mNotifyAccessPrivacyInfoStatusCallBack)
```


#### 2.updateAccessPrivacyInfoStatus
外部进行GDPR授权时，调用此方法将用户授权结果同步到UPSDK，UPSDK将不再进行授权弹窗管理。请在初始化UPSDK之前调用。

```csharp
function upltv:updateAccessPrivacyInfoStatus(gdprPermissionEnumValue))
```

示例代码：

```csharp
{

    --如果用户拒绝，调用以下方法同步到UPSDK
    upltv:updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusDefined);
  
    -- 如果用户接受授权，调用以下方法同步到UPSDK
    upltv:updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusAccepted);
}
```


#### 3.getAccessPrivacyInfoStatus
获取用户授权结果，可以在初始化UPSDK之前调用。

```groovy
function upltv:getAccessPrivacyInfoStatus()
```

示例代码：
```csharp
local e = upltv:getAccessPrivacyInfoStatus()
```

#### 4.isEuropeanUnionUser
判断用户是否属于欧盟地区，异步返回结果。
可以在初始化UPSDK之前调用。
```csharp
function upltv:isEuropeanUnionUser(callback)
```
示例代码：
```csharp
local function europeanUnionUserCallBack(result)
    print("=====> lua mEuropeanUnionUserCallBack result: " .. result)
    -- result: true 表示欧盟地区用户，否则非欧盟地区用户
    if result=="true" then
        -- 用户处于欧盟地区
    
    else
        -- 用户处于非欧盟地区
    
    end
end

upltv:isEuropeanUnionUser(europeanUnionUserCallBack)
```

