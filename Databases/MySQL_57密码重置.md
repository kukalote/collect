# Mysql5.7修改roo密码

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