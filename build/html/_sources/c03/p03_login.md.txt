# 登陆打点

如果你的应用使用到Facebook等登录功能时需要把登陆结果同步我们的统计服务器以便分析用户数据，请使用以下API完成相应登陆打点(上报)。
## 添加引用
```java
import com.hola.account.HolaLogin;
```

## 游客登录日志打点

```java
void guestLogin(String playerId);
```
- playerId： 游戏用户ID，请传入CP方自己的player ID

示例代码：

```java
HolaLogin.guestLogin("player_id");
```


## Facebook登录日志打点

```java
void facebookLogin(String playerId, String openId, String openToken);
```
- playerId： 游戏用户ID，请传入CP方自己的player ID
- openId： Facebook open ID
- openToken： Facebook token

示例代码：

```java
HolaLogin.facebookLogin("player_id", "facebook_open_id", "facebook_token");
```

## Twitter登录日志打点

```java
void twitterLogin(String playerId, long twitterId, String twitterUserName, String twitterAuthToken);
```
- playerId： 游戏用户ID，请传入CP方自己的player ID
- twitterId： twitter登录返回的用户ID
- twitterUserName： twitter登录返回的用户名
- twitterAuthToken： twitter登录返回的AuthToken，用toString()方法转换即可

示例代码：

```java
HolaLogin.twitterLogin("player_id", "twitter_id", "twitter_user_name", "twitter_authtoken");
```

## 平台账号登录日志打点

```java
void portalLogin(String playerId, String portalId); 
```
- playerId： 游戏用户ID，请传入CP方自己的player ID
- portalId： 游戏自身的平台账号ID

示例代码：

```java
HolaLogin.portalLogin("player_id", "portal_id");
```

