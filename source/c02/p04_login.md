# 登录打点


## 游客登录上报
使用游客方式登录后，需要调用此方法上报
```objective-c
/**
 @param playerId 游戏用户ID
 */
+ (void)guestLoginWithGameId:(NSString *)playerId;
```

示例代码：
```objective-c
[HolaAnalysis guestLoginWithGameId:@"yourPlayerId"];
```

## facebook登录上报
使用facebook方式登录后，需要调用此上报
```objective-c
/**
 @param playerId  游戏用户ID
 @param openId    openId
 @param openToken openToken
 */
+ (void)facebookLoginWithGameId:(NSString *)playerId
                         OpenId:(NSString *)openId
                      OpenToken:(NSString *)openToken;
```
示例代码：
```objective-c
[HolaAnalysis facebookLoginWithGameId:@"yourPlayerId" OpenId:@"yourFacebookOpenId" OpenToken:@"yourFacebookOpenToken"];
```

## twitter登录上报
使用twitter方式登录后，需要调用此方法上报
```objective-c
/**
 @param playerId 游戏用户ID
 @param twitterId 推特用户id
 @param twitterUserName 推特用户Name
 @param twitterAuthToken 推特用户Token
 */
+ (void)twitterLoginWithPlayerId:(NSString *)playerId
                       twitterId:(int64_t)twitterId
                 twitterUserName:(NSString *)twitterUserName
                twitterAuthToken:(NSString *)twitterAuthToken;
```

示例代码：
```objective-c
[HolaAnalysis twitterLoginWithPlayerId:@"yourPlayerId" twitterId:12313123 twitterUserName:@"yourTwitterUserName" twitterAuthToken:@"yourTwitterAuthToken"];
```
## 平台登录上报
```objective-c
/**
 @param playerId 游戏用户ID
 @param portalId 平台Id
 */
+ (void)portalLoginWithPlayerId:(NSString *)playerId
                       portalId:(NSString *)portalId;
```
示例代码：
```objective-c
[HolaAnalysis portalLoginWithPlayerId:@"yourPlayerId" portalId:@"yourPortalId"];
```

