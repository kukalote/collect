MySql_子查询小结

##MySQL 子查询

子查询是将一个 SELECT 语句的查询结果作为中间结果，供另一个 SQL 语句调用。MySQL 支持 SQL 标准要求的所有子查询格式和操作，也扩展了特有的几种特性。

##MySQL 子查询分类

上面演示了一个简单的子查询例子，根据子查询的返回数据形式（如上例中返回的 uid 序列），可以分为如下几类：

- **标量子查询**：返回单一值的标量，最简单的形式。
- **列子查询**：返回的结果集是 N 行一列。
- **行子查询**：返回的结果集是一行 N 列。
- **表子查询**：返回的结果集是 N 行 N 列。

##MySQL 子查询操作符

在主查询中，可以使用比较操作符以使用操作符来对子查询返回的结果进行比较，从而确定查询的条件。如上面例子使用了 IN 操作符。

可以使用的操作符有：`=` `>` `<` `>=` `<=` `<>` `ANY` `IN` `SOME` `ALL` `EXISTS` ，我们将在本节余下的教程中学习这些操作符在子查询中的使用方法。

### 1. MySQL 标量子查询

**标量子查询**是指子查询返回的是单一值的标量，如一个数字或一个字符串，也是子查询中最简单的返回形式。

一个标量子查询的例子如下：

	SELECT * FROM article WHERE uid = (SELECT uid FROM user WHERE status=1 ORDER BY uid DESC LIMIT 1)

在该例子中，子查询语句：

	SELECT uid FROM user WHERE status = 1 ORDER BY uid DESC LIMIT 1

返回的是单一的数字（如 2），实际的查询语句为：

	SELECT * FROM article WHERE uid = 2
1. **使用子查询进行比较**

 可以使用 `=` `>` `<` `>=` `<=` `<>` 这些操作符对子查询的标量结果进行比较，通常子查询的位置在比较式的右侧：

	SELECT * FROM t1 WHERE column1 = (SELECT MAX(column2) FROM t2)

2. **子查询与表连接**

 在很多情况下，子查询的效果与 JOIN 表连接很类似，但一些特殊情况下，是必须用子查询而不能用表连接的，如：

		SELECT * FROM t1 WHERE column1 = (SELECT MAX(column2) FROM t2)

 以及下例：

		SELECT * FROM article AS t WHERE 2 = (SELECT COUNT(*) FROM article WHERE article.uid = t.uid)

### 2. MySQL 列子查询

**列子查询**是指子查询返回的结果集是 N 行一列，该结果通常来自对表的某个字段查询返回。

一个列子查询的例子如下：

	SELECT * FROM article WHERE uid IN(SELECT uid FROM user WHERE status=1)

列子查询中使用 IN、ANY、SOME 和 ALL 操作符

由于列子查询返回的结果集是 N 行一列，因此不能直接使用 = > < >= <= <> 这些比较标量结果的操作符。在列子查询中可以使用 IN、ANY、SOME 和 ALL 操作符：

- **IN**：在指定项内，同 IN(项1,项2,…)。
- **ANY**：与比较操作符联合使用，表示与子查询返回的任何值比较为 TRUE ，则返回 TRUE 。
- **SOME**：ANY 的别名，较少使用。
- **ALL**：与比较操作符联合使用，表示与子查询返回的所有值比较都为 TRUE ，则返回 TRUE 。

1. **ANY 操作符**

 **ANY** 关键字必须接在一个比较操作符的后面，表示与子查询返回的任何值比较为 TRUE ，则返回 TRUE 。一个 **ANY** 例子如下：

		SELECT s1 FROM table1 WHERE s1 > ANY (SELECT s2 FROM table2)

 在子查询中，返回的是 table2 的所有 s2 列结果（5,12,20），然后将 table1 中的 s1 的值与之进行比较，只要大于 s2 的任何值即表示为 TRUE，符合查询条件。

 `IN` 是 `= ANY` 的别名，二者相同，但 `NOT IN` 的别名却不是 `<> ANY` 而是 `<> SOME`。
特殊情况

 > 如果 table2 为空表，则 ANY 后的结果为 FALSE；
    如果子查询返回如 (NULL,NULL,NULL) 列为空的结果，则 ANY 后的结果为 UNKNOWN 。

2. **ALL 操作符**

 **ALL** 关键字必须接在一个比较操作符的后面，表示与子查询返回的所有值比较为 TRUE ，则返回 TRUE 。一个 **ALL** 例子如下：

		SELECT s1 FROM table1 WHERE s1 > ALL (SELECT s2 FROM table2)

 该查询不会返回任何结果，因为 s1 中没有比 s2 所有值都大的值。

 当然在该例子查询中，返回了 s2 的所有值，您可以在该子查询中添加任何条件以限制返回的查询结果而无需全部返回。

 `NOT IN` 是 `<> ALL` 的别名，二者相同。
 特殊情况

 > 如果 table2 为空表，则 ALL 后的结果为 TRUE；
    如果子查询返回如 (0,NULL,1) 这种尽管 s1 比返回结果都大，但有空行的结果，则 ALL 后的结果为 UNKNOWN 。

 注意：对于 table2 空表的情况，下面的语句均返回 NULL：

		SELECT s1 FROM table1 WHERE s1 > (SELECT s2 FROM table2)
		SELECT s1 FROM table1 WHERE s1 > ALL (SELECT MAX(s1) FROM table2)

### 3. MySQL 行子查询

**行子查询**是指子查询返回的结果集是一行 N 列，该子查询的结果通常是对表的某行数据进行查询而返回的结果集。

一个行子查询的例子如下：

	SELECT * FROM table1 WHERE (1,2) = (SELECT column1, column2 FROM table2)

在该例子中，在保证子查询返回单一行数据的前提下，如果 column1=1 且 column2=2 ，则该查询结果为 TRUE。
1. **MySQL 行构造符**

 在上面的例子中，WHERE 后面的 (1,2) 被称为行构造符，也可以写作 ROW(1，2)。行构造符通常用于与对能返回两个或两个以上列的子查询进行比较。

		 SELECT * FROM article WHERE (title,content,uid) = (SELECT title,content,uid FROM blog WHERE bid=2)

 在该行子查询例子中，将 article 表 title,content,uid 字段逐一与子查询返回的行记录作比较，如果相等则列出这些相等的记录（理论上可能不止一条）。

### 4. MySQL 表子查询

**表子查询**是指子查询返回的结果集是 N 行 N 列的一个表数据。

	SELECT * FROM article WHERE (title,content,uid) IN (SELECT title,content,uid FROM blog)

对比前面行子查询的例子，将行子查询中的 WHERE bid=2 条件限制去掉之后，其返回的数据就是一个表记录（当然如果符合条件的记录只有一条，而成为行子查询记录，但我们认为这是一个表子查询）。

该 SQL 的意义在于查找 article 表中指定的字段同时也存在于 blog 表中的所有的行（注意 `=` 比较操作符换成了 **IN**），实际上等同于下面的条件语句：

		SELECT * FROM article,blog WHERE (article.title=blog.title AND article.content=blog.content AND article.uid=blog.uid)

实际上，后面的语句是经过 MySQL 优化的而效率更高，或者也可以使用 MySQL **JOIN** 表连接来实现。在此使用该例子只是为了便于描述表子查询的用法。

### 5. FROM 子句中的子查询

MySQL **FROM** 子查询是指 **FROM** 的子句作为子查询语句，主查询再到子查询结果中获取需要的数据。**FROM** 子查询语法如下：

	SELECT ... FROM (subquery) AS name ...

子查询会生成一个**临时表**，由于 **FROM** 子句中的每个表必须有一个名称，因此 **AS name** 是必须的。**FROM** 子查询也称为**衍生数据表子查询**。

	SELECT s1,s2 FROM (SELECT s1, s2*2 AS s2 FROM table1) AS temp WHERE s1 > 1

**提示**

MySQL **FROM** 子句中的子查询可以返回标量、列、行或表，但不能为有关联的子查询。

### 6. MySQL EXISTS 和 NOT EXISTS 子查询

MySQL **EXISTS** 和 **NOT EXISTS** 子查询语法如下：

	SELECT ... FROM table WHERE  EXISTS (subquery)

该语法可以理解为：将主查询的数据，放到子查询中做条件验证，根据验证结果（TRUE 或 FALSE）来决定主查询的数据结果是否得以保留。

	SELECT * FROM article WHERE EXISTS (SELECT * FROM user WHERE article.uid = user.uid)

**提示**

- **EXISTS** (subquery) 只返回 TRUE 或 FALSE，因此子查询中的 `SELECT *` 也可以是 `SELECT 1` 或其他，官方说法是实际执行时会忽略 **SELECT** 清单，因此没有区别。
- **EXISTS** 子查询的实际执行过程可能经过了优化而不是我们理解上的逐条对比，如果担忧效率问题，可进行实际检验以确定是否有效率问题。
- **EXISTS** 子查询往往也可以用条件表达式、其他子查询或者 **JOIN** 来替代，何种最优需要具体问题具体分析。


### 7. MySQL 关联子查询

**关联子查询**是指一个包含对表的引用的子查询，该表也显示在外部查询中。通俗一点来讲，就是子查询引用到了主查询的数据数据。

以一个实际的例子来理解关联子查询：

	SELECT * FROM article WHERE uid IN(SELECT uid FROM user WHERE article.uid = user.uid)

 将该例 SQL 与如下语句比较更能看出关联子查询与普通子查询的区别：

	SELECT * FROM article WHERE uid IN(SELECT uid FROM user)

在本实例中，虽然两个 SQL 执行后的返回结果都一样，但它们的实现过程是完全不一样的。后者（普通子查询）实际被执行为：

	SELECT * FROM article WHERE uid IN(1,2,3)

但在关联子查询中，是无法单独执行子查询语句的。其实际流程大致为：

1. 先做外部主查询；
2. 将主查询的值传入子查询并执行；
3. 子查询再将查询结果返回主查询，主查询根据返回结果完成最终的查询。

这个执行流程类似于 **EXISTS** 子查询，实际上某些情况下 MySQL 就是将关联子查询重写为 **EXISTS** 子查询来执行的。

**MySQL 关联子查询效率**

很明显，一般情况下关联子查询的效率是比较低下的，实际上本例中的关联子查询例子也仅是为了演示关联子查询的原理及用法。如果可以的话，关联子查询尽量使用 **JOIN** 或其他查询来代替。如本例中，使用 **INNER JOIN** 来替换的 SQL 为：

	SELECT article.* FROM article INNER JOIN user ON article.uid = user.uid

注意：此处只是为了演示用 **INNER JOIN** 替换关联子查询的样例，并非表名这种处理是最优处理。