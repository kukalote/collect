# MySQL的导出与导入



## 1. mysqldump 导出命令

mysqldump 是 mysql 用于转存储数据库的实用程序。它主要产生一个 SQL 脚本，其中包含从头重新创建数据库所必需的命令 CREATE TABLE INSERT 等。

```bash
mysqldump [OPTIONS] database [tables]
OR     mysqldump [OPTIONS] --databases [OPTIONS] DB1 [DB2 DB3...]
OR     mysqldump [OPTIONS] --all-databases [OPTIONS]
```


常用参数：

> **--add-drop-database** create前增加 DROP DATABASE 命令
>
> **--add-drop-table**    create前增加 DROP TABLE 命令（默认启用; use --skip-add-drop-table to disable.）
>
> --skip-add-locks  --skip-comments --skip-disable-keys --skip-set-charset.
>
> **-T, --tab=name**      为每个表创建制表分隔符式的txt文件(创建sql和txt文件)NOTE: 只在 mysqldump 和mysqld 服务为同一台机器时可用。
>
> **--fields-terminated-by=name**  字段间分隔符(逗号)
>
> **--fields-enclosed-by=name** 字段包围符(双引号)
>
> **--fields-optionally-enclosed-by=name**  字段包围符(双引号,不对字符之外起作用)
>
> **--fields-escaped-by=name** 转义字符
>
> **--lines-terminated-by=name**  记录结束符。
>
> **--ignore-table=name**  忽略指定表(--ignore-table=database.table)
>
> **--insert-ignore**     使用 INSERT IGNORE插入
>
> **--replace**           使用 REPLACE INTO 替换 INSERT INTO
>
> **-t, --no-create-info** 禁用表创建。
>
> **-d, --no-data**       禁用行数据。
>
> **--set-charset**       输出中增加 'SET NAMES default_character_set' 
>
> **--default-character-set=name** 设置默认字符集
>
> **-w, --where=name**    只输出符合条件的行. 强制使用双引号()
>
> **-p, --password[=name]**  指定密码
>
> **-P, --port=#**  指定端口
>
> **-u, --user=name**    指定用户
>
> 


```bash
mysqldump -u root -p --no-create-info --tab=/var/lib/mysql-files db_name table_name;
mysqldump -u root -p -h 127.0.0.1 --default-character-set=utf8mb4 db_name table_name > /tmp/table_name.sql;
mysqldump -u root -p -h 127.0.0.1 --where="name='张三'" db_name table_name > /tmp/table_name.sql;
```

## 2. Mysql 中的导出命令

```mysql
mysql> select * from user into outfile '/tmp/user.csv' fields terminated by ',' optionally enclosed by '"' lines terminated by '\r\n';
```

参数说明 : 
> - **into outfile** ‘导出的目录和文件名’ 
  > 指定导出的目录和文件名
> - **fields terminated by** ‘字段间分隔符’ 
  > 定义字段间的分隔符
> - **optionally enclosed by** ‘字段包围符’ 
  > 定义包围字段的字符（数值型字段无效）
> - **lines terminated by** ‘行间分隔符’ 
  > 定义每行的分隔符 



## 3. MySQL的导入

**导入sql文件**
```bash
mysql -uusername -ppassword db1 <tb1tb2.sql
```

```mysql
mysql> user db1;
mysql> source tb1_tb2.sql;
```

**导入csv格式数据**
```mysql
mysql> LOAD DATA LOCAL INFILE '/var/lib/mysql-files/user.txt' \
INTO TABLE user character set utf8 \
fields terminated by ',' enclosed by '"' \
(`id`,`name`, `create_time`);

mysql> LOAD DATA LOCAL INFILE '/var/lib/mysql-files/user.txt' \
INTO TABLE student character set gb2312 \
fields terminated by ',' optionally enclosed by '"' escaped by '"' \
lines terminated by '\r\n';
```

## 总结

**csv与sql比较**：

1. csv格式文件比sql文件要小很多
2. csv格式导出要比sql导出快很多
3. csv格式导入比sql导入也要快
4. csv的导入与导出需要在mysqld服务器上进行
5. csv格式导入导出对于非标准字符集的处理的支持没有sql效果好

## 异常处理

>  mysqldump: Got error: 1290: The MySQL server is running with the --secure-file-priv option so it cannot execute this statement when executing 'SELECT INTO OUTFILE'

  查看官方文档，secure_file_priv参数用于限制LOAD DATA, SELECT …OUTFILE, LOAD_FILE()传到哪个指定目录。

- secure_file_priv 为 **NULL** 时，表示限制mysqld不允许导入或导出。
- secure_file_priv 为 **/tmp** 时，表示限制mysqld只能在/tmp目录中执行导入导出，其他目录不能执行。
- secure_file_priv 没有值时 **''**，表示不限制mysqld在任意目录的导入导出。

解决方案：

```mysql
mysql> show global variables like '%secure_file_priv%';
mysql> set global secure_file_priv=''; -- 取消导出限制
```



