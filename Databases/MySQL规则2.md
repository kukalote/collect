## 命名规范

- 库名、表名、字段名必须使用小写字母，“_”分割。
- 库名、表名、字段名不超过12个字符。
- 库名、表名、字段名禁止使用MySQL保留字
- 库名、表名、字段名见名知意,建议使用名词而不是动词。
- 数据对象、变量的命名都采用英文字符,禁止使用中文命名.
- 临时库、表名必须以tmp为前缀，并以日期为后缀.
- 备份库、表必须以bak为前缀，并以日期为后缀

## 基础规范

- 所有表统一使用InnoDB存储引擎
- 表字符集选择UTF8mb4
- 所有表和列原则上都需要添加注释
- SQL脚本中必须明确指定更新的数据库。即有use databae_name;语句。
- 禁止在数据库中存储图片、文件。
- 禁止在线上做数据库压力测试,如有特殊需要，需提前报备。
- 原则上禁止客户端直接操作测试，生产数据库
- 禁止SQL中有DROP table 语句。
- 表结构变更需要通知DBA审核。

## 字段设计规范

- 每张表必须有名称为id的整型主键，主键必须有如下属性（int或者bigint，无符号，不为空，自增）。如：id bigint(20) unsigned NOT NULL AUTO_INCREMENT。主键列无意义，不和业务发生联系。
- 禁止DEFAULT NULL，建议NOT NULL 设置默认值
- 存储精确浮点数必须使用DECIMAL替代FLOAT和DOUBLE，或者使用bigint(需要做转换)。
- 建议使用UNSIGNED存储非负数值。
- 不建议使用ENUM类型，存储状态，性别等，用TINYINT。
- 建议使用INT UNSIGNED存储IPV4，
- 禁止在数据库中存储明文密码
- 整形定义中建议采用INT(10),而不是INT(1),INT(11)或其他,bigint采用bigint(20)。
- 建议所有表中增加create_time 和update_time 字段。
- 将过大字段拆分到其他表中。尽可能不使用TEXT、BLOB类型。如果必须使用，业务表中的TEXT,BLOB中字段，必须要拆分到单独的表中存储。
- 需要根据实际的宽度来选择VARCHAR(N)类型的宽度。N表示的是字符数不是字节数. VARCHAR(N)，N尽可能小，进行排序和创建临时表一类的内存操作时，会使用N的长度申请内存。
- 存储年使用YEAR类型，存储日期使用DATE类型。
- 使用INT(10)类型存储时间，将时间转化成时间戳存入数据库中。

#### 一句话总结：

- 能NOT NULL 就NOT NULL，char、varchar用NOT NULL DEFAULT '',tinyint、smallint、int用NOT NULL DEFAULT 0。

char、varchar取值要吝啬，根据实际需求给，比如人名一般不超过5个，varchar(5)，不要varchar(200)。int、tinyint这类，int(1)和int(13)都是一样的，

我们统一用int(10),tinyint取值范围[-128,127],加了unsigned取值[0,255]，如果不需要存储负数，整型类型的加unsigned。

## 索引规范

- 单张表的索引数量控制在5个以内。
- 复合索引中的字段数建议不超过5个。
- 非唯一索引必须按照“idx_字段名称_字段名称[_字段名]”进行命名。
- 唯一索引必须按照“uniq_字段名称_字段名称[_字段名]”进行命名。
- 合理利用覆盖索引。不使用更新频繁的列做为索引
- 对长字符串考虑使用前缀索引，前缀索引长度不超过8个字符。
- 索引字段的顺序需要考虑字段值去重之后的个数，个数多的放在前面。
- 使用EXPLAIN判断SQL语句是否合理使用索引，尽量避免extra列出现：Using File Sort，UsingTemporary。
- UPDATE、DELETE语句需要根据WHERE条件添加索引。
- 合理创建联合索引（避免冗余），(a,b,c) 相当于 (a) 、(a,b) 、(a,b,c)，但（a,c）只能用到部分索引。

#### **索引禁忌**

- 不在选择性低的列上建立索引，例如“性别”, “状态”， “类型”
- 不在索引列进行数学运算和函数运算
- 尽量不使用外键
- 高并发场景不建议使用唯一索引
- 不使用前导查询，如like “%ab”,like “%ab%”

## SQL语句

- SQL语句中IN包含的值不应过多。
- UPDATE、DELETE语句不使用LIMIT。
- WHERE条件中必须使用合适的类型，避免MySQL进行隐式类型转化。
- SELECT语句只获取需要的字段。
- SELECT、INSERT语句必须显式的指明字段名称，不使用SELECT *，不使用INSERT INTO table()。
- 使用SELECT column_name1, column_name2 FROM table WHERE [condition]而不是SELECT column_name1 FROM table WHERE [condition]和SELECT column_name2 FROM table WHERE [condition]。
- WHERE条件中的非等值条件（IN、BETWEEN、<、<=、>、>=）会导致后面的条件使用不了索引。
- 避免在SQL语句进行数学运算或者函数运算，容易将业务逻辑和DB耦合在一起。
- INSERT语句使用batch提交（INSERT INTO table VALUES(),(),()……），values的个数不应过多。
- 避免使用存储过程、触发器、函数等，容易将业务逻辑和DB耦合在一起。
- 避免使用JOIN。
- 使用合理的SQL语句减少与数据库的交互次数。
- 不使用ORDER BY RAND()，使用其他方法替换。
- 建议使用合理的分页方式以提高分页的效率。
- 统计表中记录数时使用COUNT(*)，而不是COUNT(primary_key)和COUNT(1)。
- 避免使用union或者union all。如果使用建议使用union all，因为union会将结果去重并且排序，而union all则不会