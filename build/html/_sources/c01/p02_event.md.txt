# 事件打点


统计包扩展多种打点API，以满足不同业务的打点需要。

## 1.无参数事件
```java
void log(String key);
```
- key：统计事件的键（名称）

示例代码：
```java
HolaAnalysis.log("hello_key0");
```

## 2.单参数事件
```java
void log(String key, String value);
```
- key：统计事件的键（名称）；
- value：统计事件的内容；

示例代码：
```java
HolaAnalysis.log("hello_key1", "hola_value");
```

## 3.多参数事件
```java
void log(String key, Map<String, String> value);
```
- key：统计事件的键（名称）；
- value：统计事件的内容，内容的格式是Key-Value形式的Map；

示例代码：
```java
HashMap<String, String> map = new HashMap<>();
map.put("key1", "value1");
map.put("key2", "value2");
map.put("key3", "value3");
HolaAnalysis.log("hello_key2", map);
```

## 3.计数事件
```java
void count(String key);
```
- key：统计事件的键（名称）；

示例代码：
```java
HolaAnalysis.count("hola_count_key");
```
