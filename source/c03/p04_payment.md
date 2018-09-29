# 支付打点

如果你的应用使用到Google应用内支付功能时需要把支付结果同步我们的统计服务器以便分析用户数据，请使用以下API完成相应的支付打点(上报)。
## 添加引用
```java
import com.hola.pay.HolaPay; 
```

## 支付日志打点

```java
void logPayment(String playerId, String iabPurchaseOriginalJson, String iabPurchaseSignature);
```
- playerId： 游戏用户ID，请传入CP方自己的player ID（请确认同登录打点的playerId保持一致）
- iabPurchaseOriginalJson： Purchase.getOriginalJson()
  > Google支付传入在onConsumeFinished(Purchase, IabResult)中返回的原始数据</br></br>
MyCard支付传入返回的原始json数据</br></br>
BluePay支付传入返回的原始json数据</br></br>
Amazon支付传入json格式：</br>
{“receiptId”:”amazonReceiptId”,”userId”:”amazonUserId”}，示例：{“receiptId”:”mINy5VRd1mk2z-WBtTqw9sdf1GWhnuVx07kzTBMR600=:2:11”,”userId”:”LRyD0FfW_3zeOlfJyxpVll-Z1rKn6dSf9xs12fFg0=”}
![](http://docs.upltv.com/uploads/201809/5b908e78b07d9_5b908e78.png)

- iabPurchaseSignature ： Purchase.getSignature()
  > Google支付请务必传入Google返回的原始数据</br>
MyCard支付传入”mycard”</br>
BluePay支付传入”bluepay”</br>
Amazon支付传入”amazon”


示例代码：

```java
HolaPay.logPayment("palyer_id", "purchase_json", "purchase_signature");
```

## 带游戏服务器的支付日志上报
如果上报支付数据时需要区分游戏的服务器分区，可以使用同logPaymentWithServer()方法。
```java
 void logPaymentWithServer(String playerId, String playerServer, String iabPurchaseOriginalJson, String iabPurchaseSignature);
```
- playerId 同logPayment方法
- playerServer 游戏服务器或者分区标识
- iabPurchaseOriginalJson 同logPayment方法
- iabPurchaseSignature 同logPayment方法

示例代码：

```java
HolaPay.logPaymentWithServer("palyer_id", "palyer_server", "purchase_json", "purchase_signature");
```

