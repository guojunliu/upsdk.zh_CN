## UnityPlugin 简介
UPSDK Unity Plugin集成了UPLTV AD SDK（Android与IOS双平台以及依赖库），直接面向Unity C#，提供原生API调用，实现插屏、激励视频、横幅广告等业务功能。

由于Unity的版本更新过多，我们仅对于以下平台以下版本做过适配支持：
1. android平台，仅支持Unity 5.0.0及以上的版本
2. ios平台，仅支持5.1.0及以上版本

> 如果需要支持更低版本，请联系我们。

UPSDK Unity Plugin是对UPSDK聚合集成的模块，因此包体是UPSDK的Android版本与IOS版本之和。考虑到IOS的SDK包体大小与Android版本相差悬殊，将灵活提供三种插件模式，请根据实际工程情况自由选择。

1. 仅Anroid平台插件

	如果你的工程仅面向Android市场发布，仅Android平台的插件是你的最佳选择。插件大小在10M以内(内部集成UPSDK的Anroid版本及第三方库)，相比较于IOS(540余M)非常轻量了。
	
2. 仅IOS平台的插件

	如果你的工程仅面向Apple Store，仅IOS平台插件将是你的最佳选择。由于此类插件包体较大(内含UPSDK的IOS版本及第三方库)，下载时请保持网络连接稳定，网速良好。
	
> 由于IOS的插件包相比Android大太多，我们将不再提供独立的IOS插件包，因此有IOS插件包的需求时，请下载`双平台插件`。
	
3. 双平台插件

	顾名思义，此类包内含UPSDK的IOS与Android版本以及相应依赖的第三方库（约550M，因此下载时请保持网络连接良好），如果你的工程要同时面向Android与IOS市场，此类插件将是你的最佳选择。

#### UPSDK Unity插件包，请跳转至 [UPSDK Unity Plugin 下载页](http://docc.upltv.com/docs/show/13 "SDK下载页面") 下载。
