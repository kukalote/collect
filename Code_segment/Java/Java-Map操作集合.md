# Java-Map操作集合


### Map与HashMap 操作

```java
// 创建我们打的map集合
Map map = new HashMap();

// 使用子类hashmap存储多组国家的数据（键值对）
map.put(1,"中国");
// 显示1对应的国家中文名称
String name = (String) map.get(1);   // 子类转父类
// 显示集合元素的个数
System.out.println("map中共有元素个数为"+map.size());
// 两次判断map中是否存在3键
System.out.println("是否存在3键"+map.containsKey(3));
System.out.println();
map.remove(3);   // 删除key3
System.out.println("key3已经删除");
System.out.println("是否存在3键"+map.containsKey(3));

// 分别显示键集，值集，及键值对集
System.out.println("键集为："+map.keySet());
System.out.println("键值为："+map.values());
System.out.println("键值对为："+map);
System.out.println();

Set set = map.entrySet();
//得到集合的迭代器
Iterator iterator = set.iterator();
//遍历迭代器
while (iterator.hasNext()){
    //遍历出的键值放进entry集合里
    entry=(Map.Entry) iterator.next();
    //得到entry的key
    String key = (String) entry.getKey();
    //得到entry的value
    String value = (String) entry.getValue();
    //输出key和value
    System.out.println("得到的key为"+key);
    System.out.println("得到的value为"+value); 
}

// 清空map集合并判断
map.clear();
if(map.isEmpty()){
    System.out.println("已清空map集合");
}else{
    System.out.println("map集合依然存在");
}

```

