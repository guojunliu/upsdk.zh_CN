## A/B Test接口调用
### 1. A/B Test初始化
进行A/B测试时，请在初始化SDK之后，调用此方法完成A/B Test初始化。
```javascript
 /*
     A/B Testing初始化广告配置
     参数 gameAccountId ：        string类型， 用户在游戏中的帐号id（必填）
     参数 isCompleteTask  ：      boolean类型，是否完成了游戏中的新手任务
     参数 isPaid ：               boolean类型，是否付费用户
     参数 promotionChannelName ： string类型，推广渠道，没有可以传 ""
     参数 gender ：               string类型，"M", "F"，未知可以传""
     参数 age ：                  number类型，未知可以传-1
     参数 tags ：                 Array类型（string数组），标签，没有可以传 []
    */
    static initAbtConfigjson(gameAccountId:string, 
                            isCompleteTask:boolean, 
                            isPaid:boolean, 
                            promotionChannelName?:string, 
                            gender?:string, 
                            age?:number, 
                            tags?:Array<string>)
```
示例代码：
```javascript
upltv.initAbtConfigjson("u89731",true,false,"Facebook","M",-1,["First","Second","Third"]);
```

### 2. 获取A/B Test的结果
完成A/B Test初始化后，通过此方法获取结果。
```javascript
/**
     完成A/B Testing初始化后，通过此方法获取结果
     为了保证正确获取结果，调用时间建议延后initAbtConfigJson() 2秒以上
     cpPlaceId ：        string类型， 广告位
	 callback ：回调函数
    */
    static getAbtConfig(cpPlaceId:string,callback:(config:any)=>void)
```
示例代码：
```javascript
upltv.getAbtConfig("pass",function(config:any){
});
```