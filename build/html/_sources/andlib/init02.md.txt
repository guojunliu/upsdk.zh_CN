# 初始化

## 添加引用
所有方法都以static定义在`HolaAnalysis`类中，请将**HolaAnalysis**引用到你的Java代码中。
```java
import com.hola.sdk.HolaAnalysis;
```

## 初始化API 一
根据产品ID以及渠道ID初始统计包。此方法等同于initWithZone(context, productId, channelId, 0)，即zone参数以默认值0（海外区域）调用`初始化API 二`。
```java
void init(Context context, String productId, String channelId);
```

## 初始化API 二
3.1.1.6版本开始推荐调用此API初始化统计包。zone取值0：表示海外区域，1中国大陆区域，2自动定位区域。
```java
void initWithZone(Context context, String productId, String channelId, int zone);
```

## 初始化API调用之前可调用的特别方法
由于GP以及GDPR的政策要求，统计包增加了以下方法，满足应用对不同需求的支持。

### 设置CUSTOMER_ID
向统计包中设置customerId参数，用于设置AndroidId，此方法在发布GooglePlay以外的版本时才调用。
注意：此方法在Ver 3.1.1.5 以后版本中添加，请在调用SDK初始化API之前使用。
```java
void setCustomerId(final String customerId);
```

### 获取OPEN_ID
如果需要向appsflyer中设置openid参数，可使用如下方法获取。

注意：此方法在Ver 3.1.1.5 以后版本中添加，可以在SDK初始化之前使用。
 ```java
String getOpenId(Context context);
```

### 禁止获取设备信息
禁止统计包获取设备信息

此方法在Ver 3.1.1.5 以后版本中添加，请确保在SDK初始化之前调用(如果有GDPR 授权功能，在获得GDPR授权被拒绝之后调用)。
```java
void disableAccessPrivacyInformation();
```




