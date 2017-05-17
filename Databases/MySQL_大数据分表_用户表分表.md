# MySQL_大数据分表_用户表分表

<!-- create time: 2016-03-22 11:31:09  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

用户插入数据,如果数据量大, 需要分表要怎么在多个表之间确定id值呢?

我们又如何将用户分布地插入到不同的数据表中呢?

### 1. 对用户表有一个合理的预期 : 

如果确定的用户量, 就有确定的分表数, 比如10 个表。

**新用户插入** : md5(username) = 47bce5c74f589f4867dbd57e9ca9f808

按加密后末尾按求余方法 : 
    
    intval(08)%10 = 8    //由此求得插入第8个表




