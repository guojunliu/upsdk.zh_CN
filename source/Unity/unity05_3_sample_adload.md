

## Unity Plugin 广告加载

#### 1.插屏广告加载回调API
```csharp

/*
* UPSDK插屏广告加载回调接口
* @param cpPlaceId: 第一个参数，插屏广告位标识符，不能为空或null
* @param success 第二个参数，加载成功后的回调
* @param fail 第三个参数，加载失败后的回调
* 
* 回调参数类型：Action<string,string>，第一个为cpPlaceId，广告位，可能为空或null；第二个参数为描述信息，可能为空或null
* 此方法自 2028开始支持
*/
public static void setIntersitialLoadCallback(string cpPlaceId, Action<string,string> success, Action<string, string> fail)
```

使用示例代码：

```csharp

// 调用此方法，为"inter_aaa"广告位设置加载成功与失败的广告回调代理方法
public void onBtn_ClickForIntsLoadCallback() {
    // "inter_aaa"为插屏广告位
    UPSDK.setIntersitialLoadCallback ("inter_aaa", 
        new System.Action<string, string>(actionForIntsLoadSuccess),
        new System.Action<string, string>(actionForIntsLoadFail) 
    );
}

private void actionForIntsLoadFail(string placeId, string msg)
{
    Debug.Log ("===> actionForIntsLoadFail Callback at: " + placeId);
}

private void actionForIntsLoadSuccess(string placeId, string msg)
{
    Debug.Log ("===> actionForIntsLoadSuccess Callback at: " + placeId);
}


```

#### 2.激励广告加载回调API

```csharp
/*
* UPSDK激励视频广告加载回调接口
* @param success 第一个参数，加载成功后的回调
* @param fail 第二个参数，加载失败后的回调
* 
* 回调参数类型：Action<string,string>，第一个为cpPlaceId，广告位，可能为空或null；第二个参数为描述信息，可能为空或null
* 此方法自 2028开始支持
*/
public static void setRewardVideoLoadCallback(Action<string,string> success, Action<string, string> fail)


```
使用示例代码：
```csharp

//激励视频的回调代理，不用区分广告位，因此相比于插屏加载接口少一个广告位的参数
public void onBtn_ClickForRewardLoadCallback() {
   UPSDK.setRewardVideoLoadCallback ( 
        new System.Action<string, string>(actionForRewardLoadSuccess),
        new System.Action<string, string>(actionForRewardLoadFail) 
    );
}

//激励视频加载失败后回调此方法
//参数说明：placeId一般为空或为某个特定值，msg参数通常用来描述失败的原因
private void actionForRewardLoadFail(string placeId, string msg)
{
    Debug.Log ("===> actionForRewardLoadFail Callback at: " + placeId + ", fail reason: " + msg);
}

//激励视频加载成功后回调此方法
//参数说明：placeId一般为空或为某个特定值，msg参数通为空
private void actionForRewardLoadSuccess(string placeId, string msg)
{
    Debug.Log ("===> actionForRewardLoadSuccess Callback at: " + placeId);
}


```
