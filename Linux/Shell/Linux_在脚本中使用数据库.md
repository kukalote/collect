### Linux_在脚本中使用数据库


### 24 [使用数据库](id:toc)

- 24.4 [在脚本中使用数据库](#在脚本中使用数据库)  
    - 24.4.1 [连接到数据库](#连接到数据库)
    - 24.4.2 [向服务器发送命令](#向服务器发送命令)
    - 24.4.3 [格式化数据](#格式化数据)

#### 24.4 [在脚本中使用数据库](id:在脚本中使用数据库)


##### 24.4.1 [连接到数据库](id:连接到数据库)

1. **查找程序**

    有一个 `which` 命令。如果要从命令行执行一个命令, `which` 命令可以告诉 shell 能够在哪里找到这个命令 :  
    
        $ which mysql
        /usr/bin/mysql
        
2. **登录服务器**

    如果已经在 Mysql 中为 shell 脚本创建了一个特殊的用户账户, 那么需要在 mysql 命令行中指定该用户账户 :  
    
        #!/bin/bash
        
        MYSQL=`which mysql`
        
        $MYSQL test -u test -p
        
    上面并未将密码写到脚本里, 当然因为安全方面处理了。但每次都要手动输入密码, 却非常麻烦。要解决这个问题, 可以利用 mysql 程序使用的一个特殊配置文件。 mysql 程序使用 $HOME/.my.cnf 文件来读取特殊的启动命令和设置。其中一个设置是用账户启动 mysql 会话的默认密码。
    
    要在该文件中设置默认密码, 只需使用以下代码 : 
    
        $ cat .my.cnf
        [client]
        password = test
        $ chmod 400 .my.cnf    //只有设置者可以查看

[返回目录](#toc)   

   
##### 24.4.2 [向服务器发送命令](id:向服务器发送命令)

同服务器建立链接后, 就会希望发送命令同数据库进行交互。发送命令有两种方法 :  
    
- 发送单个命令并退出;
- 发送多个命令。

要发送单个命令, 必须将命令包含在 mysql 命令行中, 作为其中的一部分。对于 mysql 命令, 使用 -e 参数来实现 : 

    #!/bin/bash
    MYSQL=`which mysql`
    
    $MYSQL dbname -u user -e 'select * from person'
    
数据库服务器将 SQL 命令的结果返回给 shell 脚本, 在 STDOUT 中显示这些结果。

如果需要发送多个 SQL 命令, 可以使用文件重定向。要在 shell 脚本中重定向命令行, 必须定义一个文件结束字符串。文件结束字符串表示重定向数据的开始和结束。

    #!/bin/bash
    
    MYSQL="$(which mysql)"
    $MYSQL dbname -u user <<EOF
    show tables;
    select * from person where age>10;
    EOF
    
    $ ./testfile
    
shell 将所有以 `EOF` 分隔符结尾的内容重定向到 mysql 命令, mysql 命令会像在提示符中输入这些行一样执行它们 。使用这种方法, 可以根据需要发送任意数目的命令到 MySQL 服务器, 但在每个命令的输出之间没有分隔。

**`Tips : `** 还应注意当使用重定向的输入方法时, mysql 程序会更改默认的输出风格。由于 mysql 程序检测到输入被重定向, 它只返回原始数据, 而不是在数据周围创建 ASCII 符号框, 这使得抽取单个数据元素非常方便。

你还可以在脚本中使用任意类型的 SQL 命令, 如果 INSERT 语句 : 

    #!/bin/bash
    
    MYSQL=`which mysql`
    
    if [ $# -ne 4 ]
    then
        ehco "Usage: testfile id firstname lastname age"
    else
        statement="INSERT INTO person VALUES ($1, '$2', '$3', $4)"
        $MYSQL dbname -u username << EOF
        $statement
    EOF
    
        if [ $? -eq 0 ] 
        then
            echo Data successfully added
        else
            echo Problem adding data
        fi
    fi
    
    $ ./testfile 23 zhang qianyu 8
    Data added successfully

[返回目录](#toc)   

##### 24.4.3 [格式化数据](id:格式化数据)

1. **将输出赋值给变量**  

    尝试捕获数据库的第一步是将 mysql 的输出重定向到环境变量。这样就可以在其他命令中使用输出信息 :  
    
        #!/bin/bash
        
        MYSQL=`which mysql`
        
        dbs=`$MYSQL dbname -u username -Bse 'show databases'`
        for db in $dbs
        do
            echo $db
        done
        
        $ ./testfile
        information_schema
        test
        
    该例在 mysql 程序命令行中使用两个附加参数。 `-B` 参数指定 mysql 程序在批量模式下工作, 同时结合了 `-s` (silent) 参数, 不输出列头和格式化符号。

2. **使用格式化标签**

    mysql 程序可以使用 `-H` 选项, 使数据库返回 HTML 格式显示输出。
    
    mysql 程序还支持另一种流行格式, 称为可扩展标记语言 ( Extensible Markup Language, XML )。该语言使用类 HTML 标签标识数据名称和值。
    
        $ mysql dbname -u username -X -e 'select * from person limit 10;'
        
    使用 XML, 可以很容易地标识数据的每一行和每条记录中的各个数据值。
 
[返回目录](#toc)   

