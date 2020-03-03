>mysql版本:5.6.11  
环境:xampp  
系统:win7  

首先, 我们先要看日志有未开启，及保存位置是否有设置。

    SHOW GLOBAL variables LIKE "%log%";

1. 慢日志记录(比预期执行时间久的请求):
    
        slow_query_log = 1        #慢日志记录是否开启
        long_query_time = 0.1    #查询超时时间0-10秒

    (文件记录支持微秒；表记录用整数，微秒将被忽略)
    
        slow_query_log_file = "slow_log.log"    #慢日志的保存地址
        log_slow_admin_statements = 1    #记录管理员命令

    (命令包括:ALTER TABLE, ANALYZE TABLE, CHECK TABLE, CREATE INDEX, 
DROP INDEX, OPTIMIZE TABLE, and REPAIR TABLE. *只有当此项开启或者未使用管理员命令时记录慢日志*)

        log_queries_not_using_indexes = 1    #对未用索引的行查找【@1】 (0:关闭;1:打开)
        log_throttle_queries_not_using_indexes = 60    #对符合【@1】条件的记录进行写限制，每分种写入条数(默认为0)
        min_examined_row_limit = 300    #查询行数少于此项设置，不会写入慢日志log-short-format
2. 常规日志记录(展示可用链接从客户端收到的查询):
    
        general_log = 1    #常规sql日志是否开启(1、开启，0、关闭)
        general_log_file = "xun-PC.log"    #常规日志保存位置

3. 错误记录日志(记录mysql起始、运行、结束时的产生的错误信息):
    
        log_error = "mysql_error.log"

    其他需要注意相关设置:
    
        log_output = "FILE"    #格式:FILE/TABLE/NONE,默认为FILE
        [FILE::记录到日志文件, TABLE::记录到mysql::**_log中(比如mysql的general_log表中), NONE当然表示无记录]
        datadir = "D:/Program Files/xampp/mysql/data"    #当前工作目录


    变量动态设置
    
        set global [变量名]=[变量值]
    例 : 
        
        SET GLOBAL general_log=on
