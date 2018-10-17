## SDK初始化

仅以Xcode工程作示例讲解，若你使用的是其它工程请参考Xcode工程操作，若有不便敬请谅解。

在UPSDK所有API接口中，初始化API最早被调用，即只有初始化UPSDK之后，才能正常使用UPSDK所支持的广告功能。

```typescript
/*
 初始化广告sdk
 请务必优先完成sdk初始化，然后才能正常使用SDK的其它API接口
 参数zone：产品发行的区域，0海外,1中国大陆,2自动根据ip定位
 可选参数callback：SDK初始化完成后的回调接口, 回调接口包含一个布尔参数 callback(boolean)，true表示成功，否则失败
*/
static initSDK(zone: number, callback?:(res:boolean)=>void)
```
示例代码：
```typescript
let initButton = new eui.Button();
initButton.label = "initButton";
this.addChild(initButton);
initButton.addEventListener(egret.TouchEvent.TOUCH_TAP, this.initButtonClick, this);

initButtonClick(e: egret.TouchEvent){
        upltv.initSDK(0);
}
```