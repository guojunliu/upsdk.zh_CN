## SDK初始化

请在主Activity中尽早初始化广告SDK，并根据您游戏的发行区域分别传入对应的`UPAdsGlobalZone`参数。

```java
    UPAdsSdk.init(final Context context, final UPAdsGlobalZone zone);

    public enum UPAdsGlobalZone {
        UPAdsGlobalZoneForeign,     //海外
        UPAdsGlobalZoneDomestic,    //中国大陆
        UPAdsGlobalZoneAuto,        //自动根据ip判断
    }
```

### setCustomerId()

对于国内发行的产品，由于无法正常收集`GAID`，导致用户新增计算错误，从3003的版本开始增加`UPAdsSdk.setCustomerId(String customerId)`方法，customerId参数取**AndroidId**的值。
请在UPAdsSdk.init()之前调用此方法。

```java
public static void setCustomerId(String customerId)
```


### 开启 debug
为方便您的接入调试，您可以在开发期间通过以下方法开启调试log，并且需要在正式发布时将其关闭

    UPAdsSdk.setDebuggable(true);
