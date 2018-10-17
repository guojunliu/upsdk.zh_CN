
## Unity Plugin A/B Test接口调用

#### 1.AbTest初始化
```csharp
/*
 * 在获取AbTest的广告配置前，必须通过此API正确初始化AbTest配置
 */
public static void initAbtConfigJson(string gameAccountId, bool completeTask, int isPaid, string promotionChannelName,  string gender, int age, string[] tags) ;
```
示例代码：

```csharp
public void onBtnInitABConfig_Click()
{
    //以下参数没有实际意义，仅仅演示如何调用此API
    UPSDK.initAbtConfigJson("mygameAccountId_123", true, 18, "324000", "gender", 33, new string[]{"This is the first element.", "The second one.", "The last one."});
}
```
#### 2.获取AbTest的配置结果

```csharp
/*
 * 获取avidly的聚合广告abtest配置
 * 返回结果为Json字符串，可能为null
 */
public static string getAbtConfig(string placementId)；

```

示例代码：

```csharp
public void onBtnGetABConfig_Click()
{
    string r = UPSDK.getAbtConfig ("hello");
    Debug.Log ("==> onBtnGetABConfig_Click:" + r);
}
```