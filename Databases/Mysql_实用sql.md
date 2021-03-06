### 查看mysql表结构:

```mysql
mysql> desc tablename; 
mysql> show create table tablename; 
mysql> use information_schema; 
    select * from columns where table_name='jos_modules'; 
mysql> show full columns from table_name;
```
    
    
### 创建数据库

```mysql
mysql> CREATE DATABASE IF NOT EXISTS my_database DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

### 创建数据表:

```mysql
mysql> DROP TABLE IF EXISTS tbl_name;
mysql> CREATE TABLE IF NOT EXISTS person2
 (
    `peopleid` smallint(6) NOT NULL AUTO_INCREMENT,
    `firstname` char(50) NOT NULL,
    `lastname` char(50) NOT NULL,
    `age` smallint(6) DEFAULT 0 NOT NULL comment '字段的注释',
    `townid` smallint(6) NOT NULL,
    PRIMARY KEY (`peopleid`),
    UNIQUE KEY `unique_fname_lname`(`firstname`,`lastname`),
    KEY `fname_lname_age` (`firstname`,`lastname`,`age`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8_general_ci auto_increment=1; comment '表注释';  
```

### 查看全局变量:
    
```mysql
mysql> SHOW GLOBAL variables LIKE "%log_%";
```

### 解释SQL执行

```mysql
mysql> EXPLAIN SELECT * FROM table WHERE id=1;
```
    
###  查看数据表属性

```mysql
mysql> SHOW TABLE STATUS LIKE 'table'\G
```
    
### 查看数据表的索引

```mysql
mysql> SHOW INDEX FROM table_name\G
```
    
### 复制表

```mysql
mysql> INSERT INTO clone_tbl (id,title,date)  \
-> SELECT id,title,date FROM runoob_tbl;
```

### 获取服务器元数据

以下命令语句可以在MySQL的命令提示符使用，也可以在脚本中 使用，如PHP脚本。

命令	| 描述
----|---
SELECT VERSION() | 服务器版本信息
SELECT DATABASE() | 当前数据库名 (或者返回空)
SELECT USER() | 当前用户名
SHOW STATUS | 服务器状态
SHOW VARIABLES | 服务器配置变量

### 处理重复插入

你可以在MySQL数据表中设置指定的字段为 `PRIMARY KEY`（主键） 或者 `UNIQUE`（唯一） 索引来保证数据的唯一性。

`INSERT IGNORE INTO`与`INSERT INTO`的区别就是`INSERT IGNORE`会忽略数据库中已经存在的数据，如果数据库没有数据，就插入新的数据，如果有数据的话就跳过这条数据。这样就可以保留数据库中已经存在数据，达到在间隙中插入数据的目的。 

```mysql
mysql> INSERT IGNORE INTO table (last_name, first_name)
    -> VALUES( 'Jay', 'Thomas');
```
    
`REPLACE`的运行与`INSERT`很相像,但是如果旧记录与新记录有相同的值，则在新记录被插入之前，旧记录被删除，`REPLACE`语句会返回一个数，来指示受影响的行的数目。该数是被删除和被插入的行数的和。

```mysql
mysql> REPLACE INTO table (last_name, first_name)
    -> VALUES( 'Jay', 'Thomas');
```
    
### 数据导出

用 sql 导出至文件 : 

```mysql
mysql> SELECT * FROM table 
    -> INTO OUTFILE '/tmp/tutorials.txt';
```

### 清空表数据

```mysql
mysql> DELETE FROM table1 //可以清空部分
mysql> TRUNCATE TABLE table1 //清空全部
```

### 清除表中重复数据 

```mysql
//goods 是销售商品与城市的关系表
mysql>  delete from goods 
    ->  where  
    ->  (sale_citys,sale_id) in ( 
    ->      select sale_citys,sale_id from (
    ->          SELECT sale_citys,sale_id FROM `goods` GROUP by sale_citys,sale_id HAVING count(1)>1
    ->  ) remove) 
    ->  and id not in (
    ->  select * from (
    ->          SELECT min(id) FROM `goods` GROUP by sale_citys,sale_id HAVING count(1)>1
    ->  ) servive);
```

### 查询所有表数据量
```mysql
mysql> use information_schema;

mysql>  select table_name,table_rows from tables
    ->  where TABLE_SCHEMA = 'testdb'
    ->  order by table_rows desc;
```
