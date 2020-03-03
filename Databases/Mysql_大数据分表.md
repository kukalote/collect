# Mysql_大数据分表

<!-- create time: 2016-03-24 16:15:16  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

### 概念二“分片”

输入图片说明

分片解决的是“数据量太大”的问题，也就是通常说的“水平切分”。一旦引入分片，势必有“数据路由”的概念，哪个数据访问哪个库。路由规则通常有3种方法：

    范围：range
         优点：简单，容易扩展
         缺点：各库压力不均（新号段更活跃）

    哈希：hash 【大部分互联网公司采用的方案二：哈希分库，哈希路由】
         优点：简单，数据均衡，负载均匀
         缺点：迁移麻烦（2库扩3库数据要迁移）

    路由服务：router-config-server
         优点：灵活性强，业务与路由算法解耦
         缺点：每次访问数据库前多一次查询
