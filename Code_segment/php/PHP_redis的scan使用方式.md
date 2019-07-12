# PHP_redis的scan使用方式

## redis的scan命令


```bash
[SCAN cursor [MATCH pattern] [COUNT count]
```

[*SCAN*](http://doc.redisfans.com/key/scan.html#scan) 命令及其相关的 [*SSCAN*](http://doc.redisfans.com/set/sscan.html#sscan) 命令、 [*HSCAN*](http://doc.redisfans.com/hash/hscan.html#hscan) 命令和 [*ZSCAN*](http://doc.redisfans.com/sorted_set/zscan.html#zscan) 命令都用于增量地迭代（incrementally iterate）一集元素（a collection of elements）：

- [*SCAN*](http://doc.redisfans.com/key/scan.html#scan) 命令用于迭代当前数据库中的数据库键。
- [*SSCAN*](http://doc.redisfans.com/set/sscan.html#sscan) 命令用于迭代集合键中的元素。
- [*HSCAN*](http://doc.redisfans.com/hash/hscan.html#hscan) 命令用于迭代哈希键中的键值对。
- [*ZSCAN*](http://doc.redisfans.com/sorted_set/zscan.html#zscan) 命令用于迭代有序集合中的元素（包括元素成员和元素分值）。

以上列出的四个命令都支持增量式迭代，它们每次执行都只会返回少量元素，所以这些命令可以用于生产环境，而不会出现像 [*KEYS*](http://doc.redisfans.com/key/keys.html#keys) 命令、 [*SMEMBERS*](http://doc.redisfans.com/set/smembers.html#smembers) 命令带来的问题 ——当 [*KEYS*](http://doc.redisfans.com/key/keys.html#keys) 命令被用于处理一个大的数据库时，又或者 [*SMEMBERS*](http://doc.redisfans.com/set/smembers.html#smembers) 命令被用于处理一个大的集合键时，它们可能会阻塞服务器达数秒之久。

```bash
redis 127.0.0.1:6379> scan 0 match 'table:filed:*key*' count 100
1) "0"
2) 1) "table:filed:key01"
   2) "table:filed:key02"
```

上面的命令中，搜索匹配 `table:field:*key*` 规则的键，按redis规定的顺序读取100个key，进行匹配。返回下次迭代用游标和匹配到的key数组。

这里有两点需要知晓：

> 1. 第一次迭代使用 `0` 作为游标，接下来的游标则以上一次返回的第一个参数为准。因为游标由于数据结构设计，并不是递增返回的。
> 2. 每次以游标0开始，搜索count指定的条数。搜索完count条数据，则返回下次迭代用游标和相应数据。搜索完全部

## PHP中使用scan

Scan 遍历keys。

##### *参数*

***LONG (引用)***:  迭代器, 初始为NULL； ***STRING***:  match的匹配项； ***LONG***: 每次遍历的key数量(only a suggestion to Redis)

##### *返回值*

*Array, boolean*:  返回匹配keys数组或者无匹配时返回false(This function will return an array of keys or FALSE if Redis returned zero keys)

这里用的是phpredis插件，官网的写法是获取有效值，却不一定返回全部匹配：

```php
<?php
// 未匹配到数据，继续匹配下一组，直至有匹配项或结束
$redis->setOption(Redis::OPT_SCAN, Redis::SCAN_RETRY); 

$it = NULL;

while ($arr_keys = $redis->scan($it, 'key:*')) {
    foreach ($arr_keys as $str_key) {
        echo "Here is a key: $str_key\n";
    }
}
//Here is a key: table:star_nickname:key:01
//Here is a key: table:star_nickname:key:01
//...
```



查明原因，应是第一个参数为 **LONG(引用)**，所以对方法进行如下改进:

```php
<?php
$iterator = NULL;
// 未匹配到数据，继续匹配下一组，直至有匹配项或结束
$redis->setOption(\Redis::OPT_SCAN, \Redis::SCAN_RETRY);

while ($arr_keys = call_user_func_array(array($redis, 'scan'), array(&$iterator, 'key:*', 100))) {
    foreach ($arr_keys as $str_key) {
        echo "Here is a key: $str_key\n";
    }
}
//Here is a key: table:star_nickname:key:01
//Here is a key: table:star_nickname:key:02
//...
```

> Redis::OPT_SCAN 为数字 4 , Redis::SCAN_RETRY 为数字1