# Eclipse 接入帮助

### 一、UPSDK的目录结构
如果您使用 Eclipse 或 Ant 构建 Android 项目，相比较于Android Studio 构建的工程，接入工作稍有一些复杂。UPSDK Eclipse版本的下载包解压后的目录结构如下：

![](http://docc.upltv.com/uploads/201805/5af56a8672b4b_5af56a86.png)

UPSDK Eclipse的主包体处于`upsdk_ads`的文件夹下，`upsdk_ads`中的内容必须添加到你的主工程中。

### 二、复制UPSDK Eclipse 主包体文件到项目
在`upsdk_ads`目录下，有`libs`、`assets`以及`res`几个目录，请分别将这些目录下的内容复制到你Eclipse主工程的相应的目录下。

`UPADSDK 3.0.03`版本在`Eclipse`接入时默认使用`dex动态加载`的方式加载广告联盟，广告联盟的文件存放在`upsdk_ads\assets\up_android`文件夹下，如果您不需要接入其中的某个联盟，将文件夹中对应的 `dex_xxx.jar` 文件删除掉即可。

**【注意】 以上的文件请不要修改任意文件或者文件夹的命名**

**【注意】`admob`、`batmobi`、`appnext`、`youappi`、`toutiao` 和 `inmobi` 广告平台暂时无法支持`dex动态加载`方式，如果需要接入，请单独将`optional_ads`中的内容添加到项目中**

> 对于非*.so文件，复制时出现重名文件时，请慎重操作：**仅在重名的文件是UPAdsSdk原有的文件时可以复盖操作。**非此情况下的文件重名冲突时，可以根据经验自行酌情解决(比如res/values/目录下的文件可以重命名)，当然也可以直接与我们的技术反馈问题寻求解决方案。

### 三、加入 Android Support 支持库
**【注意】Android Support 支持库为强制依赖，请务必将其引入你的项目，否则可能会导致程序崩溃！**

广告的正常展示需要 `support` 库的支持，所以请将其引入到您的项目中，我们为您在  `android_support_library/libs` 文件夹中准备了对应的 `xxx.jar` 文件，您只需要将文件添加到项目的 `libs` 目录下即可

**【注意】Android Support 支持库为强制依赖，请务必将其引入你的项目，否则可能会导致程序崩溃！**

### 四、加入Google Ads SDK
如果您的项目需要发布到海外市场，我们需要在您的项目中加入 Google Ads 支持，我们为您在  `admob_ads/libs` 目录中准备了对应的 `play-services-xxx.jar` 文件。

同时，请检查你的AndroidManifest.xml是否存在以下定义：

	<!-- admob -->
	<meta-data	
		android:name="com.google.android.gms.version"
		android:value="@integer/google_play_services_version"/>

	<activity
		android:name="com.google.android.gms.ads.AdActivity"
		android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize"
		android:multiprocess="true"
		android:theme="@android:style/Theme.Translucent"/>
	<!-- admob -->


### 五、修改 AndroidManifest.xml 文件
将 `AndroidManifest.xml` 文件中的内容复制加入到您项目的对应节点中。

### 六、修改 Proguard
如果你的项目使用了 `proguard`，你需要将 `proguard-project.txt` 文件中的内容复制粘贴到你项目使用的 `proguard` 配置文件中。

### 七、Demo工程
为帮助您更好的了解广告SDK的接入以及使用，我们在这里提供了一个简单的[Demo工程](https://github.com/AvidlyGit/AdSdkDemo-Eclipse "Demo工程")。
