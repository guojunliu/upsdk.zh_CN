## UnityPlugin 接入帮助
### Unity 工程如何添加/升级插件

下载好的UPSDK Unity Plugin，,是一个*.unitypackage文件。文档将以双平台插件做为示例参考，具体参考过程如下：

> 熟悉此过程的工程师，可以跳过本节


#### 1.插件升级

如果之前已经接入UPSDK Unity Plugin，为保证升级后插件库的正确性，建议在升级先完整地删除UPSDK Unity Plugin。

在你的Unity工程中，首先请检查是否存在`Assets/PolyADSDK/Plugins/`以及`Assets/StreamingAssets/avidly_android/`，一般只须删除`PolyADSDK`与`avidly_android`文件夹（包含其子目录）就已经完整地清除了已经导入的插件。

> 从3003开始，UPSDK Unity Plugin已经去掉`Assets/StreamingAssets/avidly_android/`

如果你更改过`PolyADSDK`目录的名称或其子目录的位置，请记得找到正确的文件位置并删除。特别地，如果你在`PolyADSDK`中添加过其它文件但升级后仍需保留，清除时小心误删此类文件。

然后继续**下一步**操作。

#### 2.准备导入

在你的工程中，选择Assets，右键之后选择：Import Package -> Custom Package

![import](http://docc.upltv.com/uploads/201705/592e66a727d89_592e66a7.png "import")

#### 3.选择插件

选择你下载好的插件文件：如export_3001_both.unitypackage，然后选择“open”按钮，Unity会自动加载插件包。

![open](http://docc.upltv.com/uploads/201804/5acc540715865_5acc5407.png "111")

插件的命名规则：
`export_` + `版本号` + `_` + `平台标识` + `后缀`
1. 双平台插件全名：`export_版本号_both.unitypackage`
2. ios平台插件全名：`export_版本号_ios.unitypackage`
3. android平台插件全名：`export_版本号_android.unitypackage`

#### 4.装载插件

Untiy加载完成插件包后，会弹出一个询问界面，提示你导入哪些插件资源。

以3001版本为例，效果图如下所示：

![import plugin](http://docc.upltv.com/uploads/201804/5acc5b73f1774_5acc5b73.png "import plugin")

自3003开始，UPSDK Unity插件包目录做了细微的修改，导入时相较于3003以下版本目录结果有以下些许不同。

![24444](http://docc.upltv.com/uploads/201805/5b02619429610_5b026194.jpeg "24444")


一般地选择“All”按键，然后点击“Import”开始导入。如果你的工程仅支持Android平台，那可以忽略“**IOS**”的勾选；如果仅支持IOS平台，请忽略“**Android**”的勾选。

#### 5.检查导入结果

成功导入UPSDK的插件包后，你的工程`Assets`目录下将会出现一个名为**PolyADSDK**的文件夹。`PolyADSDK/Plugins/Android`为Android平台需要用到的库文件与资源文件，`PolyADSDK/Plugins/IOS`为IOS平台需要用的库文件与资源文件。示例中，考虑到android与ios双平台的支持，因此插件资源被全部导入。

3003及更高的版本，插件文件全在`PolyADSDK`目录下

![3444](http://docc.upltv.com/uploads/201805/5b050d981d064_5b050d98.jpeg "3444")

3002及以前的版本，插件资源分别包含在`PolyADSDK`与`avidly_android`目录下

![load ok](http://docc.upltv.com/uploads/201804/5acc58ed3691b_5acc58ed.png "load ok")


