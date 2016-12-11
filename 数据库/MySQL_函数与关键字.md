# MySQL_函数

<!-- create time: 2016-03-16 14:54:37  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->


#### 时间函数 : 

    SELECT NOW();
    2016-03-16 14:52:35
    
#### 区分大小写关键字 : 
>除非你使用 `LIKE` 来比较字符串，否则MySQL的`WHERE`子句的字符串比较是不区分大小写的。   
你可以使用 `BINARY` 关键字来设定`WHERE`子句的字符串比较是区分大小写的。 

    SELECT * from runoob_tbl 
    WHERE BINARY runoob_author='sanjay';
    
#### NULL关键字

    IS NULL: 当列的值是NULL,此运算符返回true。
    IS NOT NULL: 当列的值不为NULL, 运算符返回true。
    
#### 正则表达式 REGEXP

查找name字段中以'ok'为结尾的所有数据：

    mysql> SELECT name FROM person_tbl WHERE name REGEXP 'ok$';
    
#### 唯一关键字 DISTINCT

    mysql> SELECT DISTINCT last_name, first_name
        -> FROM person_tbl
        -> ORDER BY last_name;
        
#### 忽略关键字 IGNORE

忽略错误提示

    mysql> ALTER IGNORE TABLE person_tbl
    -> ADD PRIMARY KEY (last_name, first_name);

忽略重复插入 
    
    INSERT IGNORE INTO table (last_name, first_name)
    -> VALUES( 'Jay', 'Thomas');
    
#### 时间函数 FROM_UNIXTIME

    mysql> SELECT FROM_UNIXTIME( 1249488000, '%Y-%m-%d' );
    2009-08-06
    mysql> SELECT FROM_UNIXTIME( 1249488000, '%Y-%m-%d' );
    2009-08-06 00:00:00
    
#### 时间函数2 UNIX_TIMESTAMP

    mysql> SELECT UNIX_TIMESTAMP('2009-08-06') ; 
    1249488000