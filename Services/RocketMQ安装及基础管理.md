# RocketMQ 安装及基础管理

消息队列 RocketMQ 是阿里巴巴自研消息产品，服务于整个集团已超过 13 年，经过阿里巴巴交易核心链路反复打磨与历年双十一购物狂欢节的严苛考验，是一个真正具备低延迟、高并发、高可用、高可靠，可支撑万亿级数据洪峰的分布式消息中间件。
下载地址：https://rocketmq.apache.org/docs/quick-start/

### 下载及安装
环境依赖 : java,maven(此处环境安装忽略)
```bash
> unzip rocketmq-all-4.3.2-source-release.zip
> cd rocketmq-all-4.3.2/
> mvn -Prelease-all -DskipTests clean install -U
> cd distribution/target/apache-rocketmq
```

### 启用服务
```bash
> nohup sh bin/mqnamesrv &
> tail -f ~/logs/rocketmqlogs/namesrv.log
The Name Server boot success...
```

### 启动中间件
```bash
> nohup sh bin/mqbroker -n localhost:9876 &
> tail -f ~/logs/rocketmqlogs/broker.log 
The broker[%s, 172.30.30.233:10911] boot success...
```

### 消息发送与接收
```bash
 > export NAMESRV_ADDR=localhost:9876
 > sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
 SendResult [sendStatus=SEND_OK, msgId= ...

 > sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
 ConsumeMessageThread_%d Receive New Messages: [MessageExt...
```

### 关闭服务
```bash
> sh bin/mqshutdown broker
The mqbroker(36695) is running...
Send shutdown request to mqbroker(36695) OK

> sh bin/mqshutdown namesrv
The mqnamesrv(36664) is running...
Send shutdown request to mqnamesrv(36664) OK

```