# Dnsmasq域名解析系统安装配置

**Dnsmasq使用上比bind要简便得多，可以做正向、反向dns解析，支持DHCP服务。也可以做内部dns服务器用**。

默认下，**dnsmasq使用系统的/etc/resolv.conf，并读取/etc/hosts文件**。

环境 : ubuntu 15.04
当前局域网中，本机ip : 192.168.1.115

## 1. 安装

```bash
$ sudo apt-get install dnsmasq
```

## 2. 配置

**安装完 dnsmasq 后，默认配置文件为 /etc/dnsmasq.conf**。 如果修改我们可以先做个备份, 再对原文件进行修改。

下面介绍常用的一些配置选项 :

```bash
# sudo vim /etc/dnsmasq.conf

addn-hosts=/etc/dnsmasq.hosts　　#本地域名配置文件（不支持泛域名）,自建文件
resolv-file=/etc/resolv.dnsmasq.conf	# 上一级dns,自建文件
strict-order	# 按顺序读取 dns 列表

listen-address=192.168.1.115,127.0.0.1	# 监听的ip

address=/www.taobao.com/127.0.0.1　　#正向解析
ptr-record=127.0.0.1.in-addr.arpa,www.taobao.com    #反向解析（可选）
address=/baidu.com/127.0.0.1    #泛域名解析
```

接下来是配置 dnsmasq.hosts 和 resolv.dnsmasq.conf

```bash
# sudo vim /etc/dnsmasq.hosts
nameserver 127.0.0.1
nameserver 8.8.8.8

# sudo vim /etc/dnsmasq.hosts
192.168.1.114	www.test.loc
```

> **补充一** : **配置：resolv-file=/etc/resolv.dnsmasq.conf，表示dnsmasq 会从这个指定的文件中寻找上游dns服务器**。原因是每次启动系统，原 dns 配置可能会被重写。
>
> **补充二** : **dnsmasq能够缓存外部DNS记录，同时提供本地DNS解析或者作为外部DNS的代理，即dnsmasq会首先查找/etc/hosts等本地解析文件，然后再查找/etc/resolv.conf等外部nameserver配置文件中定义的外部DNS**。所以说dnsmasq是一个DNS中继。DNS配置同样写入dnsmasq.conf配置文件里。
>
> 同时在/etc/hosts文件中加入本地内网解析，这样一来每当内网机器查询时就会优先查询hosts文件，这就等于将/etc/hosts共享给全内网机器使用，从而解决内网机器互相识别的问题。
>
> **补充三** : 设置：listen-address=127.0.0.1，表示这个 dnsmasq 本机自己使用有效。注意：如果你想让本机所在的局域网的其它电脑也能够使用上Dnsmasq，应该把本机的局域网IP加上去：listen-address=192.168.1.123,127.0.0.1。注意：如果想允许所有的用户使用你的DNS解析服务器，把listen-address去掉即可。

## 测试

配置已经 OK 了，那现在开始测试我们的 dns 服务器:

```bash
# 启动 dnsmasq 服务
$ sudo systemctl start dnsmasq.service
```

现在我们可以使用另一个电脑，比如也是linux系统的我的测试如下 :

```bash
$ nslookup www.test.loc 192.168.1.115	#使用指定的 dns 地址，解析 www.test.loc
# 如果是设置的 ip 地址，则配置成功
```

## 异常排除

我的调试过程中，有过两处异常，一个是dnsmasq的端口被占用，另一个是 重新安装时，配置文件未清理干净。

1. 53 端口被占用 :
我的是因为之前安装过bind服务，都是dns服务，端口才被占用。这里使用的是 netstat 命令，具体选项可以自己查询 :

```bash
$ netstat -nlpt | grep 53
```

2. 卸载 dnsmasq 服务
为了把软件及配置文件干净删除，可以重新下载原始默认的文件 :

```bash
$ sudo apt-get --purge autoremove dnsmasq
```

