## Unity Plugin 初始化方法
版本3003，增加了新的访问类`UPSDK.cs`，但依然兼容了`PolyADSDK.cs`所有的方法与回调接口。

示例代码以新类接口作参考。

### Unity Plugin 初始化
在插件中初始化SDK一样也非常简单，只需要调用如下API:
```csharp
/*
 * 初始化UPSDK
 * 即使多次调用，此方法也仅会初始化一次
 * @param zone:0，海外；1，中国大陆；2，自动根据ip判断
* 如果使用常量类型，自3003开始请通过UPConstant类调用常量：SDKZONE_FOREIGN,SDKZONE_CHINA,SDKZONE_AUTO；
* 3003以前的版本仍然通过PolyADSDK类调用以上常量。
 * 从 2030开始,initPolyAdSDK()增加zone参数
 */
public static string initPolyAdSDK(int zone)
```

对于android平台，低于2031版本初始化时特别注意以下API的调用
```csharp
public static void setManifestPackageName(string packagename)
```
> 为了避免出错，请将AndroidManifest.xml文件中，Application节点对应的packagename属性正确传递到setManifestPackageName()方法中。

#### 示例代码
在示例代码中，通过点击UI上的初始化按钮，调用onButton_init_Click()方法。即使多次点击初始化按钮，**重复调用UPSDK.initPolyAdSDK()**，但SDK的**实际初化只会进行一次**。
```csharp
public void 调用onButton_init_Click()
{
    //TextEditor text = GameObject.Find ("CallText").GetComponent<TextEditor>();


    if (!inited) {
        UPSDK.UPSDKInitFinishedCallback = new System.Action&ltbool, string>(actionForSdkInitFinish);
        UPSDK.UPInterstitialDidClickCallback = new System.Action&ltstring, string>(actionForInterstitialDidClick);
        UPSDK.UPInterstitialDidCloseCallback = new System.Action&ltstring, string>(actionForInterstitialDidClose);
        UPSDK.UPInterstitialDidShowCallback = new System.Action&ltstring, string>(actionForInterstitialDidShow);

        UPSDK.UPBannerDidShowCallback = new System.Action&ltstring, string>(actionForSdkBannerDidShow);
        UPSDK.UPBannerDidClickCallback = new System.Action&ltstring, string>(actionForSdkBannerDidClick);
        UPSDK.UPBannerDidRemoveCallback = new System.Action&ltstring, string>(actionForSdkBannerRemove);

        UPSDK.UPRewardDidOpenCallback = new System.Action&ltstring, string>(actionForSdkRewardDidOpen);
        UPSDK.UPRewardDidClickCallback = new System.Action&ltstring, string>(actionForSdkRewardDidClick);
        UPSDK.UPRewardDidCloseCallback = new System.Action&ltstring, string>(actionForSdkRewardDidClose);
        UPSDK.UPRewardDidGivenCallback = new System.Action&ltstring, string>(actionForSdkRewardDidGiven);
        UPSDK.UPRewardDidAbandonCallback = new System.Action&ltstring, string>(actionForSdkRewardDidAbandon);

        #if UNITY_ANDROID && !UNITY_EDITOR

            UPSDK.UPExitAdDidShowCallback = new System.Action&ltstring> (actionForSdkExitAdDidShow);
            UPSDK.UPExitAdDidClickCallback = new System.Action&ltstring> (actionForSdkExitAdDidClick);
            UPSDK.UPExitAdDidClickMoreCallback = new System.Action&ltstring> (actionForSdkExitAdDidClickMore);
            UPSDK.UPExitAdOnExitCallback = new System.Action&ltstring> (actionForSdkExitAdOnExit);
            UPSDK.UPExitAdOnCancelCallback = new System.Action&ltstring> (actionForSdkExitAdOnExit);

        #endif
    }
    inited = true;

    string tt = UPSDK.initPolyAdSDK (0);
    Debug.Log ("initPolyAdSDK ====> " + tt);
}
```

回调接口的实现原则如下：
回调接口不是必须实现，根据自己的需求可以全部不实现或选择性地实现。示例工程的代码，说明了每个接口的作用以及参数意义。


```csharp
#if UNITY_ANDROID && !UNITY_EDITOR
private void actionForSdkExitAdDidShow(string msg) {
    Debug.Log ("===> actionForSdkExitAdDidShow Callback");
}

private void actionForSdkExitAdDidClick(string msg) {
    Debug.Log ("===> actionForSdkExitAdDidClick Callback");
}

private void actionForSdkExitAdDidClickMore(string msg) {
    Debug.Log ("===> actionForSdkExitAdDidClickMore Callback");
}

private void actionForSdkExitAdOnExit(string msg) {
    Debug.Log ("===> actionForSdkExitAdOnExit Callback");
}

private void actionForSdkExitAdDidOnCancel(string msg) {
    Debug.Log ("===> actionForSdkExitAdDidOnCancel Callback");
}
#endif

// test for reward video callback
private void actionForSdkRewardDidOpen(string placeId, string msg) {
    Debug.Log ("===> actionForSdkRewardDidOpen Callback at: " + placeId);
}

private void actionForSdkRewardDidClick(string placeId, string msg) {
    Debug.Log ("===> actionForSdkRewardDidClick Callback at: " + placeId);
}

private void actionForSdkRewardDidClose(string placeId, string msg) {
    Debug.Log ("===> actionForSdkRewardDidClose Callback at: " + placeId);
}

private void actionForSdkRewardDidGiven(string placeId, string msg) {
    Debug.Log ("===> actionForSdkRewardDidGiven Callback at: " + placeId);
}

private void actionForSdkRewardDidAbandon(string placeId, string msg) {
    Debug.Log ("===> actionForSdkRewardDidAbandon Callback at: " + placeId);
}

private void actionForSdkBannerRemove(string placeId, string msg) {
    Debug.Log ("===> actionForSdkBannerRemove Callback at: " + placeId);
}

private void actionForSdkBannerDidClick(string placeId, string msg) {
    Debug.Log ("===> actionForSdkBannerDidClick Callback at: " + placeId);
}

private void actionForSdkBannerDidShow(string placeId, string msg) {
    Debug.Log ("===> actionForSdkBannerDidShow Callback at: " + placeId);
}

private void actionForSdkInitFinish(bool result, string msg) {
    Debug.Log ("===> actionForSdkInitFinish Callback r: " + result + ", msg: " + msg);
}

private void actionForInterstitialDidShow(string placeId, string msg) {
    Debug.Log ("===> actionForInterstitialDidShow Callback at: " + placeId);
}

private void actionForInterstitialDidClick(string placeId, string msg) {
    Debug.Log ("===> actionForInterstitialDidClick Callback at: " + placeId);
}

private void actionForInterstitialDidClose(string placeId, string msg) {
    Debug.Log ("===> actionForInterstitialDidClose Callback at: " + placeId);
}
```