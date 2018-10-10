## IOS CocosCreator说明 

  请注意，若使用CocosCreator1.7（或以上）的版本进行接入，需要在UpAdsBrigeJs.mm文件进行两处修改。
### 1、添加SeApi.h引用

  ```objective-c
#### include "cocos/scripting/js-bindings/jswrapper/SeApi.h"
```

### 2、ScriptingCore被se::ScriptEngine替代
在`[UpAdsBrigeJs vokeMethod:arg1:arg2]`方法里，将`ScriptingCore::getInstance()->evalString(jsCallStr.c_str());` 替换为`se::ScriptEngine::getInstance()->evalString(jsCallStr.c_str());`。

参考代码如下：
```objective-c
+ (void)vokeMethod:(NSString*) name arg1:(NSString*) param1 arg2:(NSString*)param2 {
    //...
    if (jsCallStr.c_str()) {
        //ScriptingCore::getInstance()->evalString(jsCallStr.c_str());
        se::ScriptEngine::getInstance()->evalString(jsCallStr.c_str());
    }
}

```