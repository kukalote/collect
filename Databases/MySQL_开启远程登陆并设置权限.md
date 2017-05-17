### MySQL_开启远程登陆并设置权限

开启 MySQL 的远程登陆帐号有两大步： 

1. 确定服务器上的防火墙没有阻止 3306 端口。

    MySQL 默认的端口是 3306 ，需要确定防火墙没有阻止 3306 端口，否则远程是无法通过 3306 端口连接到 MySQL 的。
    
    如果您在安装 MySQL 时指定了其他端口，请在防火墙中开启您指定的 MySQL 使用的端口号。
    
2. 增加允许远程连接 MySQL 用户并授权。
 
    1. 首先以 root 帐户登陆 MySQL
    
        在 Linux 主机中在命令提示行下输入下面的命令。
        
            mysql -uroot -p123456     #123456 为 root 用户的密码。 
    
    2. 创建远程登陆用户并授权
       
            grant all PRIVILEGES on discuz.* to ted@'123.123.123.123' identified by '123456';

        下面逐一分析所有的参数 : 
        
        `all PRIVILEGES `表示赋予所有的权限给指定用户，这里也可以替换为赋予某一具体的权限，例如： `select`,`insert`,`update`,`delete`,`create`,`drop` 等，具体权限间用“,”半角逗号分隔。  
        
        `discuz.*` 表示上面的权限是针对于哪个表的，discuz 指的是数据库，后面的 `*` 表示对于所有的表，由此可以推理出 : 对于全部数据库的全部表授权为“`*.*`”，对于某一数据库的全部表授权为“ 数据库名.*”，对于某一数据库的某一表授权为“`数据库名.表名`”。
        
        `ted` 表示你要给哪个用户授权，这个用户可以是存在的用户，也可以是不存在的用户。
        `123.123.123.123` 表示允许远程连接的 IP 地址，如果想不限制链接的 IP 则设置为“`%`”即可。
        `123456` 为用户的密码。
    
    3. 执行了上面的语句后，再执行重载授权表，方可立即生效。
    
            flush privileges;



远程登录mysql一些常用的代码段，下面举例说明 :

1. 允许root用户在任何地方进行远程登录，并具有所有库任何操作权限，具体操作如下 : 
   
    在本机先使用root用户登录mysql : 
   
        $ mysql -u root -p"youpassword"   
        mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'youpassword' WITH GRANT OPTION;                  --进行授权操作
        mysql> FLUSH PRIVILEGES;    --重载授权表
        mysql> exit ;               --退出mysql数据库

2. 允许root用户在一个特定的IP进行远程登录，并具有所有库任何操作权限，具体操作如下 : 
   
    在本机先使用root用户登录mysql : 
    
        $ mysql -u root -p"youpassword" 
        mysql> GRANT ALL PRIVILEGES ON *.* TO root@"172.16.16.152" IDENTIFIED BY "youpassword" WITH GRANT OPTION; 
        mysql> FLUSH PRIVILEGES;
        mysql> exit;

3. 允许root用户在一个特定的IP进行远程登录，并具有所有库特定操作权限，具体操作如下：
    
    在本机先使用root用户登录mysql： 

        $ mysql -u root -p"youpassword" 
        mysql> GRANT select,insert,update,delete ON *.* TO root@"172.16.16.152" IDENTIFIED BY "youpassword";
        mysql> FLUSH PRIVILEGES;
        mysql> exit;

4. 删除用户授权，需要使用REVOKE命令，具体命令格式为 : 
    
        REVOKE privileges ON 数据库[.表名] FROM user-name; 
        
    具体实例，先在本机登录mysql : 
    
        $ mysql -u root -p"youpassword" 
        mysql> GRANT select,insert,update,delete ON TEST-DB TO test-user@"172.16.16.152" IDENTIFIED BY "youpassword"; 
        mysql> REVOKE all on TEST-DB from test-user;    --删除授权操作    
        mysql> DELETE FROM user WHERE user="test-user"; --从用户表内清除用户
        mysql> FLUSH PRIVILEGES; 
        mysql> exit;

    
    **`Tips : `** 该操作只是清除了用户对于`TEST-DB`的相关授权权限，但是这个“test-user”这个用户还是存在。

