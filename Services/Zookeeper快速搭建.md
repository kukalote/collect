# Zookeeper快速搭建
ZooKeeper是一个分布式的，开放源码的分布式应用程序协调服务，是Google的Chubby一个开源的实现，是Hadoop和Hbase的重要组件。它是一个为分布式应用提供一致性服务的软件，提供的功能包括：配置维护、域名服务、分布式同步、组服务等。
下载地址 : http://www.apache.org/dyn/closer.cgi/zookeeper
### 安装zookeeper
环境依赖:java版本: jdk1.8
```bash
> wget http://mirror.olnevhost.net/pub/apache/zookeeper/zookeeper-3.4.11/zookeeper-3.4.11.tar.gz
> tar zxvf zookeeper-3.4.11.tar.gz  
> mv zookeeper-3.4.11 /usr/local/share/zookeeper
```

### 修改环境变量

编辑 **/etc/profile** 文件, 在文件末尾添加以下环境变量配置:

```bash
# ZooKeeper Env
> export ZOOKEEPER_HOME=/usr/local/share/zookeeper
> export PATH=$PATH:$ZOOKEEPER_HOME/bin
```
运行以下命令使环境变量生效: `source /etc/profile`

### 重命名配置文件

初次使用 ZooKeeper 时,需要将**$ZOOKEEPER_HOME/conf** 目录下的 **zoo_sample.cfg** 重命名为 **zoo.cfg, zoo.cfg**
```bash
> mv  $ZOOKEEPER_HOME/conf/zoo_sample.cfg $ZOOKEEPER_HOME/conf/zoo.cfg
```

### 单机模式--修改配置文件

创建目录**/usr/local/zookeeper/data** 和**/usr/local/zookeeper/logs** 修改配置文件
```bash
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/usr/local/share/zookeeper/data
dataLogDir=/usr/local/share/zookeeper/logs
clientPort=2181
```

### 启动 ZooKeeper 服务
```bash
# cd /usr/local/zookeeper/zookeeper-3.4.11/bin
# ./zkServer.sh  start
> ZooKeeper JMX enabled by default
Using config: /usr/local/zookeeper/zookeeper-3.4.11/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED

> zkServer.sh  status
ZooKeeper JMX enabled by default
Using config: /usr/local/zookeeper/bin/../conf/zoo.cfg
Mode: follower
```
