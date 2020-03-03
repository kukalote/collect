### MySQL初使用(brew安装法)

MySQL 安装目录为 `/usr/local/Cellar/mysql55`  

启动 MySQL  :  

    /usr/local/Cellar/mysql55/5.5.40/bin/mysql.server start
    Starting MySQL
    .. SUCCESS!
    
打开 MySQL 终端 :  

    /usr/local/Cellar/mysql55/5.5.40/bin/mysql
    
**Mysql 命令行参数**

参数 |  描述
----|---
`-A` | 禁用自动重散列
`-b` | 禁用错误声音警告
`-B` | 不使用历史文件
`-C` | 压缩所有在客户端和服务器之间发送信息
`-D` | 指定要使用的数据库
`-e` | 执行指定的语句并退出
`-E` | 垂直显示查询输出, 每行一个数据段
`-f` | 如果发生 SQL 错误继续执行
`-G` | 启用指定的 mysql 命令
`-h` | 指定 Mysql 服务器的主机名 ( 默认为 localhost )
`-H` | 以 HTML 代码形式显示查询输出
`-i` | 忽略函数后面的空格
`-N` | 结果中不显示列名
`-o` | 忽略除命令行中指定的默认数据库的语句以外的相关语句
`-p` | 指定用户账户密码
`-P` | 指定网络连接使用的 TCP 编号
`-q` | 不缓存查询结果
`-r` | 显示列值, 但不显示转义
`-s` | 静音模式
`-S` | 为本地连接指定套接字
`-t` | 以表格形式显示输出
`-T` | 程序退出后显示调试信息、内存和 CPU 统计信息
`-u` | 指定登录用户名
`-U` | 只允许指定键值的 UPDATE 和 DELETE 语句
`-v` | 冗余模式
`-w` | 如果建立链接失败, 等待并重试
`-X` | 以 XHTML 代码形式显示查询结果

MySQL 程序拥有自己的命令集, 使用这些命令能够轻松地控制环境和获取 MySQL 服务器的相关信息。

**mysql 命令**

命令 | 快捷命令 | 描述
----|---|---
`?` | `\?` | 帮助
`clear` | `\c` | 清除命令
`connect` | `\r` | 链接数据库和服务器
`delimiter` | `\d` | 设置 SQL 语句分隔符
`edit` | `e` | 使用命令行编辑器编辑命令
`ego` | `\G` | 向 MySQL 服务器发送命令, 并垂直显示结果
`exit` | `\q` | 退出 mysql 程序
`go` | `\g` | 向 MySQL 服务器发送命令
`help` | `\h` | 显示帮助
`nopager` | `\n` | 禁用输出分页程序, 将输出发送到 STDOUT
`note` | `\t` | 不向输出文件发送输出
`pager` | `\P` | 为特定程序设置分页程序命令 ( 默认使用 more )
`print` | `\p` | 打印当前命令
`prompt` | `\R` | 更改 mysql 命令提示
`quit` | `\q` | 退出 mysql 程序 ( 同 exit )
`rehash` | `\#` | 重新生成命令完成散列表
`source` | `\.` | 执行指定文件中的 SQL 脚本
`status` | `\s` | 获取 MySQL 服务器的状态信息
`system` | `\!` | 在系统中执行 shell 命令
`tee` | `\T` | 将所有输出附加到指定文件中
`use` | `\u` | 使用其他数据库
`charset` | `\C` | 更改到其他字符集
`warnings` | `\W` | 在每个语句后面显示警告
`nowarning` | `\w` | 在每个语句后面不显示警告


#### 创建数据库对象

新建数据库的 SQL 语句为 : 

    CREATE DATEBASE name;
    
查看数据库列表 :  
    
    SHOW DATABASES;
    
连接到数据库 :  

    USE database_name;
    
查看数据库下表文件 : 

    SHOW TABLES;

