## GDPR
`GDPR《一般数据保护法案》`是欧盟出台的数据保护方案，如果您的产品会面向欧盟用户，我们提供如下方案确保`UPSDK`遵守`GDPR`规范。

`UPSDK`在`3.0.03`版本支持欧盟`GDPR`规范，发行区域包含欧盟或涵盖欧盟用户的开发者必须处理此逻辑。

### GDPR 推荐用例
#### 方案一
推荐根据自己游戏的画面风格自定义GDPR的授权弹框界面，保证产品体验最佳。
采用此方案时，仅需要将授权结果同步给UPSDK后再初始化UPSDK。

示例代码：
```javascript
yourOwnGDPRCallback(result) {
        // 如果result : "true" 表示用户接受授权，false拒绝授权
        // 请参考以下代码完成UPSDK的授权同步与初始化
        if (result=="true") {
            upltv.updateAccessPrivacyInfoStatus(1);
        } else {
            upltv.updateAccessPrivacyInfoStatus(2);
        }
        // 调用updateAccessPrivacyInfoStatus()之后再初始化UPSDK
        // 假定发行地区是海外，且不需要回调
        upltv.initSDK(0)
   }

   europeanUnionUserCallBack(result) {
        console.log("=====> ts europeanUnionUserCallBack result: " + result);
        // result: "true" 表示欧盟地区用户，否则非欧盟地区用户
        if (result=="true") {
            // 欧盟地区用户，调用自定义授权询问方法
            // callYourOwnGDPRDialog()，是我们假定的方法
            // yourOwnGDPRCallback，是我们假定的授权回调方法
            // 请根据实际代码替换
            callYourOwnGDPRDialog(yourOwnGDPRCallback);
        } else {
            // 非欧盟地区用户，直接初始化SDK
            // 假定发行地区是海外，且不需要回调
            upltv.initSDK(0);
        }
    }


// GDPR
checkGDPR() {
        upltv.getAccessPrivacyInfoStatus(function(permission:number){
            console.log("=====> ts getAccessPrivacyInfoStatus status: %d", permission)
            // 如果没有询问过授权
            if (permission==0)
            {
                // 先定位用户是否是欧盟地区
                upltv.isEuropeanUnionUser(function(isEuropean:boolean){

                });
            } else {
                // 假定发行地区是海外，且不需要回调
                upltv.initSDK(0);
            }
        }); 
    }
```


#### 方案二
如果采用UPSDK提供的标准授权处理机制，请参考以下代码修改UPSDK的初始化流程。

示例代码：
```javascript
notifyAccessPrivacyInfoStatusCallBack(value) {
        console.log("=====> ts notifyAccessPrivacyInfoStatusCallBack callback: %d ",  value);
        upltv.initSDK(0);
    }

    europeanUnionUserCallBack(result) {
        console.log("=====> ts europeanUnionUserCallBack result: " + result)
        // result: "true" 表示欧盟地区用户，否则非欧盟地区用户
        if (result=="true"){
            // 弹出系统弹窗询问
            upltv.notifyAccessPrivacyInfoStatus(function(permission:number){

            });
        } else {
            // 非欧盟地区用户，直接初始化SDK
            // 假定发行地区是海外，且不需要回调
            upltv.initSDK(0);
        }
    }

    // GDPR
    checkGDPR() {
        upltv.getAccessPrivacyInfoStatus(function(permission:number){
            console.log("=====> ts getAccessPrivacyInfoStatus status: %d ", e);
            if (permission==0)
            {
                // 先定位用户是否是欧盟地区
                upltv.isEuropeanUnionUser(function(isEuropean:boolean){

                });
            } else {
                // 假定发行地区是海外，且不需要回调
                upltv.initSDK(0);
            }
        });
        
    }
```

### GDPR API介绍

#### 1.notifyAccessPrivacyInfoStatus
弹出授权窗口，向用户说明重要数据收集的情况并询问用户是否同意授权，如果用户拒绝授权将放弃相关数据的收集。请在初始化UPSDK之前调用。
```javascript
// 弹出授权窗口，向用户说明重要数据收集的情况并询问用户是否同意授权
// 如果用户拒绝授权将放弃相关数据的收集
// 请在初始化UPSDK之前调用
// @param callback  permission： 0 Unkown，1 Accepted，2 Defined
// Version 3003 and above support  upltvoc method
    static notifyAccessPrivacyInfoStatus(callback:(permission:number)=>void)
```
#### 2.updateAccessPrivacyInfoStatus
外部进行GDPR授权时，调用此方法将用户授权结果同步到UPSDK，UPSDK将不再进行授权弹窗管理。请在初始化UPSDK之前调用。
```javascript
// 外部进行GDPR授权时，将用户授权结果同步到UPSDK时，调用此方法
// 请在初始UPSDK之前调用
// @param permission： 0 Unkown，1 Accepted，2 Defined
// Version 3003 and above support  upltvoc method
    static updateAccessPrivacyInfoStatus (permission:number)
```
#### 3.getAccessPrivacyInfoStatus
获取用户授权结果，可以在初始化UPSDK之前调用。
```javascript
//获取当前GDPR授权状态，返回 0 Unkown，1 Accepted，2 Defined
// 可在初始UPSDK之前调用
// Version 3003 and above support  upltvoc method
    static getAccessPrivacyInfoStatus(callback:(permission:number)=>void)
```
#### 4.isEuropeanUnionUser
判断用户是否属于欧盟地区，异步返回结果。
可以在初始化UPSDK之前调用。
```javascript
static isEuropeanUnionUser(callback:(param:boolean)=>void)
```
