# Java之Json处理

#### 1.List转JSONArray

```java
List<T> list = new ArrayList<T>();
JSONArray array= JSONArray.parseArray(JSON.toJSONString(list))；
```

#### 2.JSONArray转List

```java
JSONArray array = new JSONArray();
List<EventColAttr> list = JSONObject.parseArray(array.toJSONString(), EventColAttr.class);
```

#### 3.String转JSONArray

```java
String st = "[{name:Tim,age:25,sex:male},{name:Tom,age:28,sex:male},{name:Lily,age:15,sex:female}]";
JSONArray tableData = JSONArray.parseArray(st); 

String sts = "{name:Tim,age:25,sex:male}";
JSONObject tableObject = JSONObject.parseObject(sts);
```