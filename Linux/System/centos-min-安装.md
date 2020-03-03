# CentOS7 服务器配置与管理

>
**第三方软件手动下载目录**
`/usr/local/src/`
**第三方软件手动安装目录**
`/usr/local/share/`
`/usr/share/`
**日志目录**
`/var/log/`
**数据目录**
`/var/lib/`
**项目目录**
`/var/www/`

### 安装完系统后初次处理
```
# 联网设置
$ vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
# 修改为 ONBOOT=yes
$ systemctl restart network
# 更新yum 源
$ yum update
$ yum upgrade
# 安装 epel 源
yum -y install epel-release
```

### 安装常用软件及工具
```
$ yum install -y wget zsh gcc ncurses-devel libzip bzip2   net-tools yum-utils.noarch ntpdate
$ yum install -y python-devel python34-devel ruby lua perl
$ yum install -y nginx.x86_64  redis git
#mysql php72
```

### 安装重量级软件或服务

**oh-my-zsh**
```bash
yum install zsh 
chsh -s /bin/zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

**vim8**
```
# vim8
$ git clone https://github.com/vim/vim.git
$ cd vim
$ /bin/bash  vim.configure
```
```bash
#!/bin/bash
./configure
--with-features=huge
--enable-pythoninterp
--enable-python3interp
--enable-multibyte
--enable-cscope
#--enable-rubyinterp
#--enable-luainterp
#--enable-perlinterp
--with-python-config-dir=/usr/lib64/python2.7/config/
--with-python3-config-dir=/usr/lib64/python3.4/config-3.4m/
--prefix=/usr/local/
make && make install
```
安装其他周边扩展
```bash
$ yum install ctags
```
**安装mysql**
mysql库安装引导 : `https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/`
```bash
$ cd /usr/local/src
# 官方推荐
$ rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-2.noarch.rpm
$ yum-config-manager --disable mysql80-community
$ yum-config-manager --enable mysql57-community
$ yum repolist enabled | grep mysql
$ yum install mysql-community-server
```
官方推荐库安装异常，推荐
```bash
$ rpm -Uvh  http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
$ yum -y install mysql57-community-release-el7-10.noarch.rpm 
$ yum -y install mysql-community-server
$ yum -y remove mysql57-community-release-el7-10.noarch  #因为安装了Yum Repository，以后每次yum操作都会自动更新，需要把这个卸载掉
```
**PHP72**
```bash
$ rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
$ yum install php72w  php72w-fpm.x86_64
$ yum install php72w-cli.x86_64 php72w-common.x86_64 php72w-devel.x86_64 php72w-bcmath.x86_64 php72w-dba.x86_64 php72w-imap.x86_64 php72w-mbstring.x86_64 php72w-mysql.x86_64 php72w-opcache.x86_64 php72w-pdo.x86_64 php72w-xml.x86_64 php72w-pecl-memcached.x86_64 php72w-pecl-redis.x86_64 php72w-pecl-xdebug.x86_64
# 安装 composer
$ curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```

### 环境配置
**Nginx环境配置**
```bash
$ firewall-cmd --zone=public --add-port=80/tcp --permanent # 防火墙允许80端口开放
```


**MySQL 环境配置**
```bash
$ systemctl start  mysqld.service
$ systemctl status mysqld.service
 # 修改mysql 密码
$ grep "password" /var/log/mysqld.log
$ mysql -uroot -p     # 回车后会提示输入密码
mysql-$ alter user user() identified by 'new_mysql_password';
```
```mysql
mysql-$ use mysql;
mysql-$ insert into user (host, user, password) values('%', 'manager', password('localpassword'));
mysql-$ grant all on *.* to manager@'%' identified by 'newpassword';    # 授权远程登陆
mysql-$ flush privileges;
```
**Redis 环境配置**
Redis 密码 `/etc/redis.conf`
>requirepass abcd1234
```bash
$ redis-cli -a abcd1234
# 或者
$ redis-cli
redis-$ auth abcd1234
```








