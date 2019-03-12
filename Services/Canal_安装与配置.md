# Canal 安装与配置

canal是应阿里巴巴存在杭州和美国的双机房部署,存在跨机房同步的业务需求而提出的。早期,阿里巴巴B2B公司因为存在杭州和美国双机房部署,存在跨机房同步的业务需求。
从2010年开始,阿里系公司开始逐步的尝试基于数据库的日志解析,获取增量变更进行同步,由此衍生出了增量订阅&;消费的业务,从此开启了一段新纪元。
简单来说，就是模拟从库，从主库拉取bin-log日志，然后再异步进行处理。异步操作包括数据同步，跨表写入，非mysql数据库写入。
ps : 
> canal 原理是分析bin-log，这里只支持以 mysql 为主库。

## 主库设置
首先需要让主库支持bin-log，主从同步。

1. 修改 my.cnf
```mysql
[mysqld]
log-bin=mysql-bin #添加这一行就ok
binlog-format=ROW #选择row模式
server_id=1 #配置mysql replaction需要定义，不能和canal的slaveId重复
```
2. 设置mysql 远程权限
```mysql
CREATE USER canal IDENTIFIED BY 'canal';  
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'canal'@'%';
-- GRANT ALL PRIVILEGES ON *.* TO 'canal'@'%' ;
FLUSH PRIVILEGES;
```

## 安装canal
1. 下载canal 并编译
```bash
$ git clone git@github.com:alibaba/canal.git 
$ cd canal; 
$ mvn clean install -Dmaven.test.skip -Denv=release
```
编译完成后，会在根目录下产生target/canal.deployer-version.tar.gz 
2. 安装
```bash
$ mkdir /usr/local/share/canal
$ tar zxvf canal.deployer-version.tar.gz  -C /usr/local/share/canal
```
3. 修改配置
```bash
$ cd /usr/local/share/canal
$ vim conf/example/instance.properties
```
```
## mysql serverId
canal.instance.mysql.slaveId = 1234

#position info，需要改成自己的数据库信息
canal.instance.master.address = 127.0.0.1:3306

#username/password，需要改成自己的数据库信息
canal.instance.dbUsername = canal
canal.instance.dbPassword = canal
canal.instance.defaultDatabaseName = test
canal.instance.connectionCharset = UTF-8

#table regex
canal.instance.filter.regex = .\..
```

## canal 的使用与记录
1. 启动服务 
```bash
$ sh bin/startup.sh
```
2. 查看日志
```bash
$ vi logs/canal/canal.log
```
具体instance的日志：
```bash
$ vi logs/example/example.log
```

3. 关闭服务
```bash
$ sh bin/stop.sh
```

## docker 启动canal
```bash
$ docker run -d -it -e canal.instance.master.address=172.17.0.1:3306 \
--name=canal-server  \
-p 8000:8000 -p 2222:2222 -p 11111:11111 -p 11112:11112  \
canal/canal-server:v2  \
/bin/bash -c "sh app.sh"
```