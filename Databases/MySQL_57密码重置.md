# Mysql5.7修改roo密码

环境 : ubuntu 16.04
数据库版本 : mysql  Ver 14.14 Distrib 5.7.22

#### Stop MySQL
```bash
sudo service mysql stop
```
#### Make MySQL service directory.
```bash
sudo mkdir /var/run/mysqld
```
#### Give MySQL user permission to write to the service directory.
```bash
sudo chown mysql: /var/run/mysqld
```
#### Start MySQL manually, without permission checks or networking.
```bash
sudo mysqld_safe --skip-grant-tables --skip-networking &
```
#### Log in without a password.
```bash
# 以 root 用户登陆，并直接调用 mysql 数据库
mysql -uroot mysql
```
#### update password to nothing
```sql
update user set authentication_string=PASSWORD("") where User='root'; 
```
#### set password resolving to default mechanism for root user
```sql
update user set plugin="mysql_native_password" where User='root'; 
flush privileges;	# refresh power list
quit;
```
#### reset mysql service and login in 
```bash
sudo /etc/init.d/mysql stop 
sudo /etc/init.d/mysql start # reset mysql 
mysql -u root -p   # try login to database
```

环境 : CentOS Linux release 7.5.1804 (Core)
数据库版本 : mysql  Ver 14.14 Distrib 5.7.22

#### 查找 mysql 密码
mysql5.7.6以后的版本在启动数据库的时候，会生成密码放到日志文件里，可从从日志获取：
```bash
$ cat /var/log/mysqld.log | grep 'password'
```

#### 设置密码
```sql
$ mysql -u root -p
$ set password='Rootroot123@';
```
 
####  查看mysql 密码设置
```sql
$ show variables like 'vali%';
$ set global validate_password_length=8; -- 设置密码最小长度
```
变量 | 默认值 | 含义
----|----| ---
validate_password_length             | 8 | 密码长度
validate_password_mixed_case_count   | 1 |  大小写混写最小长度
validate_password_number_count       | 1 | 数字最小长度
validate_password_policy             | MEDIUM | 密码等级
validate_password_special_char_count | 1 | 特殊符号最小长度
