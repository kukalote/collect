# Elasticsearch 

环境 : 
系统 : CentOS Linux 7 (Core)
Elasticsearch : elasticsearch-6.2.3

### 1. 安装 Elasticsearch
1. **下载安装包**
	版本列表 : https://www.elastic.co/downloads/past-releases
	这里下载的版本为 6.2.3 : https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.3.zip
2. **安装 Elasticsearch**
```shell
$ wget -P /usr/local/src  https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.3.zip
$ unzip /usr/local/src/elasticsearch-6.2.3.zip -d /usr/local
```
3. **调整权限**
```shell
# 增加新群组
$ groupadd elasticsearch
# 变更目录权限
$ chown elasticsearch:elasticsearch -R /usr/local/elasticsearch-6.2.3
```
4. **启动 Elasticsearch**
	由于 Elasticsearch 不支持 root 用户运行方式，需要专门指定非 root 用户运行。
```shell
$ su elasticsearch
# 运行 Elasticsearch
$ /usr/local/elasticsearch-6.2.3/bin/elasticsearch
# 停止需要 Ctrl-c
```
5. **安装插件 : IK Analysis**
```shell
#  在root用户下进行亦可
$   /usr/local/elasticsearch-6.2.3/bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.2.3/elasticsearch-analysis-ik-6.2.3.zip
# 重新启动服务-切换用户
$ su elasticsearch
# 后台运行
$  /usr/local/elasticsearch-6.2.3/bin/elasticsearch -d
# 退出 elasticsearch 账户
$ exit
# 查看库里结构
$ curl 'localhost:9200/_mapping?pretty=true'
```
6. **安装与配置 kibana**
```shell
# 下载代码
$ wget https://artifacts.elastic.co/downloads/kibana/kibana-6.2.3-linux-x86_64.tar.gz
$ tar zxvf kibana-6.2.3-linux-x86_64.tar.gz
$ mv kibana-6.2.3-linux-x86_64 /usr/local/
# 启动 kibana
$ /usr/local/kibana-6.2.3-linux-x86_64/bin/kibana
# 本机访问地址 : http://0.0.0.0:5601
```
如果需要外网访问 kibana 服务，需要修改其配置文件 :
```shell
$ vim /usr/local/kibana-6.2.3-linux-x86_64/config/kibana.yml
# 修改 server.host 为 server.host: "0.0.0.0"
```
x. **异常及处理**

1 . 外网无法链接 elasticsearch
```shell
# 结果curl "http://127.0.0.1:9200" 能够正常访问，可是使用外网ip就提示拒绝链接

# 解决办法：
$ vim config/elasticsearch.yml
# 增加：network.host: 0.0.0.0
$  /usr/local/elasticsearch-6.2.3/bin/elasticsearch -d
```
2 . 内存不够问题(通常虚拟机中会出现)
```shell
# max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

# 在root用户下
$ vim /etc/sysctl.conf
# 增加一行  vm.max_map_count=655360
$ sysctl -p
$ su elasticsearch
$  /usr/local/elasticsearch-6.2.3/bin/elasticsearch -d
```
3 .  文件描述符配置设置问题
```shell 
#  [1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]

# 解决方法 : 在 /etc/security/limits.conf 末尾加上两行
$ vim /etc/security/limits.conf
# 增加两行
# elasticsearch soft nofile 65536
# elasticsearch hard nofile 65536
```



