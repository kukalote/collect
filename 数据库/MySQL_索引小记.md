# MySQL_索引小记

<!-- create time: 2016-03-16 15:47:20  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

索引分`单列索引`和`组合索引`。

**单列索引**，即一个索引只包含单个列，一个表可以有多个单列索引，但这不是组合索引。  
**组合索引**，即一个索包含多个列。   

> 创建索引时，你需要确保该索引是应用在 SQL 查询语句的条件(一般作为 WHERE 子句的条件)。

**索引也会有它的缺点**
> 虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT、UPDATE和DELETE。  
> 因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件。    
> 
> 建立索引会占用磁盘空间的索引文件。 


### 普通索引
**创建索引**

这是最基本的索引，它没有任何限制。它有以下几种创建方式：

    CREATE INDEX indexName ON mytable(username(length)); 

> 如果是CHAR，VARCHAR类型，length可以小于字段实际长度；如果是BLOB和TEXT类型，必须指定 length。

**修改表结构**

    ALTER mytable ADD INDEX [indexName] ON (username(length)) 

**删除索引的语法**

    DROP INDEX [indexName] ON mytable; 
    
### 唯一索引

它与前面的普通索引类似，不同的就是：索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。它有以下几种创建方式： 

**创建索引**

    CREATE UNIQUE INDEX indexName ON mytable(username(length)) 

**修改表结构**

    ALTER mytable ADD UNIQUE [indexName] ON (username(length)) 
    
    
### 使用ALTER 命令添加和删除索引

有四种方式来添加数据表的索引：

    ALTER TABLE tbl_name ADD PRIMARY KEY (column_list): 该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL。

    ALTER TABLE tbl_name ADD UNIQUE index_name (column_list): 这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）。
    ALTER TABLE tbl_name ADD INDEX index_name (column_list): 添加普通索引，索引值可出现多次。
    ALTER TABLE tbl_name ADD FULLTEXT index_name (column_list):该语句指定了索引为 FULLTEXT ，用于全文索引。

