=============================
UPSDK Cocos2d-X Lua帮助文档
=============================


## CocosLua 插件简介
CocosLua插件包括了对Android与iOS平台的支持，满足于在Lua中实现对UPSDK的广告调用及回调事件接收。由于Cocos2d版本较多，当前我们仅支持Cocos2d-X 3.0以上版本（含QuickLua3.X版本），而且要求你当前所使用的Cocos2d-X引擎中保留Lua-Bridge库（Lua-Bridge是Cocos引擎额外提供的Lua与Java及IOS跨平台调用解决方案，并集成cocos2d库中）。对于早期或不支持Lua-Bridge的Cocos2d-X Lua版本，请使用原生接入的方式。


## CocosLua 插件平台接入
如果您对如何将UPSDK CocosLua插件包的接入到当前工程还不熟悉，请仔细阅读[UPSDK CocosLua插件接入帮助](http://docs.upltv.com/docs/show/153 "参考这里")。我们根据UPSDK CocosLua插件的特性，详细说明了Android及IOS工程下接入时的操作细节。

## CocosLua 插件使用示例
如果您需要详细了解UPSDK CocosLua插件的API定义及使用示例，请阅读[UPSDK CocosLua插件使用示例](http://docs.upltv.com/docs/show/154 "参考这里")。
Contents:

.. toctree::
   :maxdepth: 1
   :glob:

   ../Cocos2d-X_Lua/*
