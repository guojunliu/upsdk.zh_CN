## SDK初始化

仅以Xcode工程作示例讲解，若你使用的是其它工程请参考Xcode工程操作，若有不便敬请谅解。

在UPSDK所有API接口中，初始化API最早被调用，即只有初始化UPSDK之后，才能正常使用UPSDK所支持的广告功能。

### 在project.json工程中添加引用，如下所示
请将`UPLTV.js`与`UPLTVIos.js`添加到`src->upltv`目录下，然后再配置到`project.json`文件中。
```javascript
"jsList" : [
        "src/resource.js",
        "src/app.js",
        "src/upltv/UPLTV.js",
        "src/upltv/UPLTVIos.js"
]
```
###  初始化UPSDK
```javascript
/*
 初始化广告sdk
 请务必优先完成sdk初始化，然后才能正常使用SDK的其它API接口
 参数zone：产品发行的区域，0:海外，1:中国大陆，2:自动根据ip定位
 可选参数callback：SDK初始化完成后的回调接口, 回调接口包含一个布尔参数 callback(boolean)，true表示成功，否则失败
*/
intSdk : function(zone, callback)
```

示例代码：
```javascript
var initSdkButton = this.createButton(x, y, "initSdk");
initSdkButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.intSdk(0);
    }
}, this);
```
####  setCustomerId()
仅Android支持，对于非GP的包，可以传androidid，避免用户新增统计错误。
```asp

// 请在初始化SDK之前调用
// Version 3004(subversion 5) and above support this method
// @param androidid

upltv.setCustomerId(androidid)
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

