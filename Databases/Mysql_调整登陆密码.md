# Mysql_调整登陆密码

<!-- create time: 2016-03-09 15:53:02  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

### 1. 重置登陆密码

	> mysql -u root -p
	> Enter password: ******
	> use mysql;
	> update user set password=password('new_password') where user='root'; 
	
### 2. 忘记密码

1. 先确认 mysql 服务已经关闭
2. 在命令行`窗口1`中输入

		> mysqld -nt --skip-grant-tables
3. 重新打开另一个命令行`窗口2`, 输入
	
		> mysql -u root 
		> use mysql;
		> update user set password=password('new_password') where user='root';
		> flush privileges;
		
4. 重启 mysql 服务, 就可以使用新密码登陆


### 使用 mysqladmin

1.例如你的 root用户现在没有密码，你希望的密码修改为123456，那么命令是:

	mysqladmin -u root password 123456

2.如果你的root现在有密码了（123456），那么修改密码为new_password的命令是:

	mysqladmin -u root -p password new_password

>注意，命令回车后会问你旧密码，输入旧密码123456之后命令完成，密码修改成功。  

3.如果你的root现在有密码了（123456），那么修改密码为new_password的命令是:
>(注意-p和后面的密码中间不要有空格。应该写成如下所示)

	mysqladmin -u root -p123456 password new_password 

4.使用phpmyadmin，这是最简单的了，修改mysql库的user表,不过别忘了使用PASSWORD函数。


