# 支付打点

## 添加引用
```objective-c
#import  &lt;AvidlyAnalysisSDK/AvidlyAnalysisSDK.h>
```

## Apple IAP上报
部分使用者会使用到APP Store的应用内支付(IAP)，如果使用这些功能需要调用我们的对应的方法来进行支付上报。

```objective-c
+ (void)LogPaymentWithPlayerId:(NSString *)playerId receiptDataString:(NSString *)receiptDataString;
```

如果需要上报（区/服）参数，可以使用
```objective-c
+ (void)LogPaymentWithPlayerId:(NSString *)playerId gameAccountServer:(NSString *)gameAccountServer receiptDataString:(NSString *)receiptDataString;
```
参数说明：
- playerId: 游戏用户 ID，请传入CP方自己的player ID，用于后续对应
- gameAccountServer: 游戏区/服ID
- receiptDataString: 支付凭证字符串（base64编码）

Apple IAP 上报，需要在支付成功后调用，即**-(void)paymentQueue:(SKPaymentQueue *)queue updatedTransactions:(NSArray *)transaction**中调用。

## 第三方支付平台上报
部分使用者会使用到mycard、bluepay等第三方支付平台，如果使用这些功能需要调用我们的对应的方法来进行支付上报。

```objective-c
+ (void)ThirdpartyLogPaymentWithPlayerId:(NSString *)playerId thirdparty:(NSString *)thirdparty receiptDataString:(NSString *)receiptDataString;
```
如果需要上报（区/服）参数，可以使用
```objective-c
+ (void)ThirdpartyLogPaymentWithPlayerId:(NSString *)playerId gameAccountServer:(NSString *)gameAccountServer thirdparty:(NSString *)thirdparty receiptDataString:(NSString *)receiptDataString;
```
参数说明：
- playerId： 游戏用户 ID，请传入CP方自己的player ID，用于后续对应
- gameAccountServer： 游戏区/服ID
- thirdparty： 为第三方支付平台名称 如mycard、bluepay等
## 添加引用
```objective-c
#import  &lt;AvidlyAnalysisSDK/AvidlyAnalysisSDK.h>
```

## Apple IAP上报
部分使用者会使用到APP Store的应用内支付(IAP)，如果使用这些功能需要调用我们的对应的方法来进行支付上报。

```objective-c
+ (void)LogPaymentWithPlayerId:(NSString *)playerId receiptDataString:(NSString *)receiptDataString;
```

如果需要上报（区/服）参数，可以使用
```objective-c
+ (void)LogPaymentWithPlayerId:(NSString *)playerId gameAccountServer:(NSString *)gameAccountServer receiptDataString:(NSString *)receiptDataString;
```
参数说明：
- playerId: 游戏用户 ID，请传入CP方自己的player ID，用于后续对应
- gameAccountServer: 游戏区/服ID
- receiptDataString: 支付凭证字符串（base64编码）

Apple IAP 上报，需要在支付成功后调用，即**-(void)paymentQueue:(SKPaymentQueue *)queue updatedTransactions:(NSArray *)transaction**中调用。

## 第三方支付平台上报
部分使用者会使用到mycard、bluepay等第三方支付平台，如果使用这些功能需要调用我们的对应的方法来进行支付上报。

```objective-c
+ (void)ThirdpartyLogPaymentWithPlayerId:(NSString *)playerId thirdparty:(NSString *)thirdparty receiptDataString:(NSString *)receiptDataString;
```
如果需要上报（区/服）参数，可以使用
```objective-c
+ (void)ThirdpartyLogPaymentWithPlayerId:(NSString *)playerId gameAccountServer:(NSString *)gameAccountServer thirdparty:(NSString *)thirdparty receiptDataString:(NSString *)receiptDataString;
```
参数说明：
- playerId： 游戏用户 ID，请传入CP方自己的player ID，用于后续对应
- gameAccountServer： 游戏区/服ID
- thirdparty： 为第三方支付平台名称 如mycard、bluepay等
- receiptDataString： 第三方支付平台单据
