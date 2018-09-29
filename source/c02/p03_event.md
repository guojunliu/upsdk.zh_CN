# 事件打点


## 事件打点
```objective-c
+ (void)logWithKey:(NSString *)key value:(id)value;
```
- key：事件id
- value：可以为nil，NSString，数组与字典

## 示例说明：
1. value为nil
相当于无参数打点
```objective-c
[HolaAnalysis logWithKey:@"your key" value:nil];
```

2. value为NSString
相当于单参数打点
```objective-c
[HolaAnalysis logWithKey:@"your key" value:@"your str value"]; 
```

3. value为NSDictionary
多参数打点，NSDictionary的 &lt;key,value>仅支持 &lt;NSString*, NSString*>类型。
```objective-c
NSDictionary *dic = [NSDictionary dictionaryWithObjectsAndKeys:
                            @"Tom", @"name",
                            @"Man", @"sex",
                            @"18", @"age",
                            nil];
  [HolaAnalysis logWithKey:@"your key" value: dic];
```

4. value为NSArray
value为NSArray&lt;NSString>
```objective-c
  NSArray *arrA = [NSArray arrayWithObjects:@"arrValue1", @"arrValue2",nil];
  [HolaAnalysis logWithKey:@"your key" value: arrA];
```
value为NSArray&lt;NSDictionary>
```objective-c
  NSDictionary *dic1 = [NSDictionary dictionaryWithObjectsAndKeys:
                            @"Tom", @"name",
                            @"Man", @"sex",
                            @"18", @"age",
                            nil];
  NSDictionary *dic2 = [NSDictionary dictionaryWithObjectsAndKeys:
                            @"Emily", @"name",
                            @"Woman", @"sex",
                            @"20", @"age",
                            nil];
  NSArray *arrB = [NSArray arrayWithObjects:dic1, dic2, nil];
  [HolaAnalysis logWithKey:@"your key" value:arrB];
```

## 计数打点
```objective-c
+ (void)countWithKey:(NSString *)key;
```
示例代码：
```objective-c
[HolaAnalysis countWithKey:@"your key"];
```



