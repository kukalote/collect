# MySQL_索引类型区分

<!-- create time: 2016-03-28 10:07:50  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->


### 按逻辑区分

**1. 普通索引**

**2. 唯一索引**

**3. 主索引**

**4. 外键索引**

**5. 复合索引**

**6. 短索引**

**7. 全文索引**



### 按应用区分

`PRIMARY`, `INDEX`, `UNIQUE` 这3种是一类

>`PRIMARY` 主键索引。 就是 唯一 且 不能为空。  
`INDEX` 索引，普通的  
`UNIQUE` 唯一索引。 不允许有重复。  
`FULLTEXT` 是全文索引，用于在一篇文章中，检索文本信息的。


### 按实际类型

`hash`  主要用于 *内存数据表*

`BTree`

`RTree`