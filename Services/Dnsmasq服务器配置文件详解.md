# Dnsmasq 服务器配置文件详解

来源 : http://www.freeoa.net/osuport/servap/dnsmasq-use-intro-refer_2480.html

**Dnsmasq提供DNS缓存和DHCP服务功能。作为域名解析服务器(DNS)，dnsmasq可以通过缓存DNS请求来提高对访问过的网址的连接速度。作为DHCP服务器，dnsmasq可以为局域网电脑提供内网ip地址和路由，DNS和DHCP两个功能可以同时或分别单独实现。**dnsmasq轻量且易配置，此外它还自带了一个PXE服务器以及对邮件服务器的mx记录的支持，jabber的srv记录的支持等。它提供了DNS功能和可选择的DHCP功能可以取代dhcpd服务和bind等服务，配置起来更简单，更适用于虚拟化和大数据环境的部署。笔者在上海某机房中使用它的50台实体机和1000台虚拟机的各类爬虫应用提供解析服务，Dnsmasq配置文件是/etc/dnsmasq.conf，在它的配件文件中的注释已经给出了非常详细的解释。

本文对这两项功能做比较实用的使用说明，有自己也有他人的经验。

Dnsmasq的默认的配置文件中有许多选项，而且在设置上有很当灵活。dns与dhcp的许多功能它都具备。它可服务于那些只在本地适用的域名，这些域名是不会在全球DNS服务器中出现的(私有域名)。DHCP服务器和DNS服务器结合，并且允许DHCP分配的地址能在DNS中正常解析，而这些DHCP分配的地址和相关命令可以配置到每台主机中，也可以配置到一台核心设备中(比如路由器)，DNSmasq支持静态和动态两种DHCP配置方式。

一般情况下，我们可以用bind解决dns的问题，dhcpd解决dhcp的问题，可用dnsmasq解决下面的一些维护问题：

> 1、局域网有很多机器希望使用一致的hosts文件，你需要经常维护这份列表。
>
> 2、你希望局域网的人访问某个域名时，拦截下来到指定的ip，做缓存节省带宽或者其它用途都可以。优先使用本地自定义dns。
>
> 3、阻止对某个域名的正常解析。

---------------------------
**公用配置**

```bash
# 设置想监听的地址，要在本机上以守护进程方式启动dnsmasq做DNS缓存或DHCP地址分配服务器，如果仅为本机使用则写上127.0.0.1，编辑/etc/dnsmasq.conf，添加监听地址：
listen-address=127.0.0.1

# 如果用此计算机作为一组主机的默认 DNS，就需要使用固定 IP 地址：
listen-address=192.168.8.1

# 其它主机的dns设置使用这个ip为dns服务器(/etc/resolv.conf)。

# 服务监听的网络接口地址
listen-address=192.168.8.132,127.0.0.1

# 改变Dnsmasq默认的uid和gid
#user=
#group=

# 如果你想Dnsmasq监听某个端口为dhcp、dns提供服务
#interface=

# 你还可以指定哪个端口你不想监听
#except-interface=

# 关于其日志记录的几个选项
log-queries

# Log lots of extra information about DHCP transactions.
#log-dhcp

# Log to this syslog facility or file. (defaults to DAEMON)
log-facility=/var/log/dnsmasq.log

# 异步log，缓解阻塞。
log-async=20

# 自动加载其它目录下的配置文件
#conf-dir=/etc/dnsmasq.d
```


---------------------------
DNS配置

**dnsmasq能够缓存外部DNS记录，同时提供本地DNS解析或者作为外部DNS的代理，即dnsmasq会首先查找/etc/hosts等本地解析文件，然后再查找/etc/resolv.conf等外部nameserver配置文件中定义的外部DNS**。所以说dnsmasq是一个DNS中继。DNS配置同样写入dnsmasq.conf配置文件里。

同时在/etc/hosts文件中加入本地内网解析，这样一来每当内网机器查询时就会优先查询hosts文件，这就等于将/etc/hosts共享给全内网机器使用，从而解决内网机器互相识别的问题，比如像hadoop添加datanode节点时。相比逐台机器编辑hosts文件或者添加Bind记录，仅需要编辑一个/etc/hosts文件。


```bash
# 如果你不想使用/etc/hosts，则取消下面的注释
#no-hosts

# 本地解析文件，如果你项读取其他类似/etc/hosts文件，则进行配置。hosts文件的强大之处还在于能够劫持解析，譬如mirror.debian.org是Debian仓库所在，可将它解析成一个内网地址，搭建一个内网镜像站，可以很快的安装软件源。
#addn-hosts=/etc/banner_add_hosts

# 主机名扩展，自动的给hosts中的name增加一个域名
# 例如，/etc/hosts中的db1将扩展成db1.freeoa.net
expand-hosts
# Add local-only domains here, queries in these domains are answered from /etc/hosts or DHCP only.
local=/freeoa.net/
 
# 强制使用完整的解析名，从不转发格式错误的域名
domain-needed

# 如果只使用dhcp服务，完全禁止DNS功能，则可将设置为0。
#port=5353

# 以下两个参数告诉Dnsmasq过滤一些查询：1.哪些公共DNS没有回答 2.哪些root根域不可达。

# 从不转发不在路由地址中的域名
#bogus-priv

# 添加额外的上级DNS主机（nameserver）配置文件(默认用/etc/resolv.conf)，resolv-file配置Dnsmasq额外的向流的DNS服务器，如果不开启就使用linux主机默认的/etc/resolv.conf里的nameserver，通过下面的选项指定其他文件。
#resolv-file=/etc/dnsmasq.d/upstream_dns.conf

# 默认情况下Dnsmasq会发送查询到它的任何上游DNS服务器上，如果取消注释，则Dnsmasq则会严格按照/etc/resolv.conf中的DNS Server顺序进行查询。
#strict-order

# 以下两个参数控制是否通过/etc/resolv.conf确定上游服务器，是否检测/etc/resolv.conf的变化，则取消注释。

# 如果你不想Dnsmasq读取/etc/resolv.conf文件或者其他文件，获得它的servers。即不使用上级DNS主机配置文件(/etc/resolv.conf和resolv-file）
#no-resolv
```

```bash
# 如果你不允许Dnsmasq通过轮询/etc/resolv.conf或者其他文件来获取配置的改变，则取消注释。
#no-poll

# 可以为特定的域名指定解析它专用的nameserver。一般是内部dns name server
# server=/myserver.com/192.168.8.1

# 增加一个name server，一般用于内网域名
#server=/localnet/192.168.8.1

# 设置一个反向解析，所有192.168.3.0/24的地址都到10.1.2.3去解析
#server=/3.168.192.in-addr.arpa/10.1.2.3

# 增加一个本地域名，会在/etc/hosts中进行查询
#local=/localnet/

# 同上，还支持ipv6
#address=/www.thekelleys.org.uk/fe80::20d:60ff:fe36:f83

# 增加查询yahoo google和它们的子域名到ff、search查找
# Add the IPs of all queries to yahoo.com, google.com, and their
# subdomains to the ff and search ipsets:
#ipset=/yahoo.com/google.com/ff,search

# 控制Dnsmasq和Server之间的查询从哪个网卡出去
# server=10.1.2.3@eth1

# 指定dnsmasq默认查询的上游服务器，此处以Google Public DNS为例。
server=8.8.8.8
server=8.8.4.4

# 把所有.cn的域名全部通过114.114.114.114这台国内DNS服务器来解析
server=/cn/114.114.114.114

# 给*.apple.com和taobao.com使用专用的DNS
server=/taobao.com/223.5.5.5
server=/.apple.com/223.6.6.6

# 指定源地址携带10.1.2.3地址和192.168.8.1的55端口进行通讯
# and this sets the source (ie local) address used to talk to
# 10.1.2.3 to 192.168.8.1 port 55 (there must be a interface with that
# IP on the machine, obviously).
# server=10.1.2.3@192.168.8.1#55

# 如果你想在某个端口只提供dns服务，则可以进行配置禁止dhcp服务
#no-dhcp-interface=

# 给设定一个默认的或给dhcp服务指定一个域名
#domain=freeoa.net

# 给dhcp的一个子域赋一个不同的域名
#domain=wireless.thekelleys.org.uk,192.168.2.0/24

# 同上，不过子域是一个范围了
#domain=reserved.thekelleys.org.uk,192.68.3.100,192.168.3.200

# 设置DNS缓存大小(单位：DNS解析条数)
cache-size=500

# 增加一个域名，强制解析到所指定的地址上，强行指定domain的IP地址，dns欺骗
address=/doubleclick.net/127.0.0.1
address=/.phobos.apple.com/202.175.5.114
```

默认情况下：
resolv-file指定dnsmasq从哪里获取上行DNS Server， 默认是从/etc/resolv.conf获取。配置 dnsmasq 的上游 dns 服务器，(因为这是一个 dns 缓存, 那么其还是需要有上级服务器进行一次域名解析的来源)

addn-hosts指定dnsmasq从哪个文件中读取“地址 域名”记录， 默认是系统文件/etc/hosts。配置系统的 dns 服务器, 将 dnsmasq 设置在首位寻找。

listen-address默认是监控在所有网卡上的，设置 dnsmasq 需要监听的 IP 地址。

1、首先配置 resolv-file=/etc/resolv.dnsmasq.conf 这个参数表示 dnsmasq 会从这个指定的文件中寻找上级 dns 服务器列表，而不是从本机的(resolv.conf)中读取dns服务器列表，如果机器的地址是通过dhcp取得的话，该文件容易受到影响从而影响dnsmasq。

系统首先寻找本地的 dnsmasq 服务器 取消注释的 strict-order 表示严格安装 resolv-file 文件中的顺序从上到下进行 DNS 解析, 直到第一个成功解析成功为止

2、no-hosts, 默认情况下这是注释掉的, dnsmasq 会首先寻找本地的 hosts 文件，再去寻找缓存下来的域名, 最后去上级 dns 服务器中寻找；而addn-hosts可以使用额外的hosts文件。所以说dnsmasq是一个很不错的外部DNS中继。

3、设置 listen-address=127.0.0.1,192.168.0.1 表示该 dnsmasq 服务可以在哪些地址上侦听，127那个地址即本机，对外提供服务的话要写上对应的网口所有的地址。

4、其他配置项:
cache-size=1024 设置缓存大小

log-queries 开启debug模式，记录客户端查询记录到/var/log/debug中

5、客户端机器配置，编辑/etc/resolv.conf ，调整内容为 'nameserver 192.168.0.1' (其中该IP是内部dns的IP，也即dnsmasq的地址)

客户端测试域名是否生效：nslookup www.freeoa.net检查解析的IP即可，或使用dig指令。

自定义主机名的ip地址指向
先在'/etc/hosts'文件里加入两行：

```bash
192.168.0.1 gateway
192.168.0.8 home.freeoa.net
```

编辑dnsmasq.conf，找到如下配置行：

```bash
# Add local-only domains here, queries in these domains are answered
# from /etc/hosts or DHCP only.
local=/localnet/

# Add domains which you want to force to an IP address here.
# The example below send any host in doubleclick.net to a local
# webserver.
#address=/doubleclick.net/127.0.0.1
address=/163.com/192.168.0.2
```

重启dnsmasq即可，我们可在局域网另外一个机器用dig或nslookup命令测试。

```bash
$ dig gateway

; <<>> DiG 9.8.4-rpz2+rl005.12-P1 <<>> gateway
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 43215
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;gateway.            IN    A

;; ANSWER SECTION:
gateway.        0    IN    A    192.168.0.1

;; Query time: 2 msec
```

由于默认的本机所使用的dns服务是dnsmasq所的机器，所以上面的查询是有效的。

```bash
$ dig gateway @8.8.8.8

; <<>> DiG 9.8.4-rpz2+rl005.12-P1 <<>> gateway @8.8.8.8
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 31552
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 0

;; QUESTION SECTION:
;gateway.            IN    A

;; AUTHORITY SECTION:
.            910    IN    SOA    a.root-servers.net. nstld.verisign-grs.com. 2014041000 1800 900 604800 86400

;; Query time: 35 msec
```

上面是使用google的dns所返回的结果，明显是没有找到，另外从'Query time'也可看出，使用了dnsmasq后性能提高了不少。

在来看一下拦截并修改过的dns记录。

```bash
$ dig home.freeoa.net
; <<>> DiG 9.8.4-rpz2+rl005.12-P1 <<>> home.freeoa.net

;; QUESTION SECTION:
;home.freeoa.net.        IN    A

;; ANSWER SECTION:
home.freeoa.net.    0    IN    A    192.168.0.8

$ dig home.freeoa.net @8.8.4.4

; <<>> DiG 9.8.4-rpz2+rl005.12-P1 <<>> home.freeoa.net @8.8.4.4

;; QUESTION SECTION:
;home.freeoa.net.        IN    A

;; ANSWER SECTION:
home.freeoa.net.    199    IN    A    180.158.255.10
```

上面是从内外部机器上查询的记录结果，后面的公网ip地址其实是我的公网地址，前者是内部nat地址，在内外部访问出来的结果是一样的，因为它们就是出自于同一台机器。

对'*.163.com'(像news.163.com、money.163.com)的请求都将内部的地址上(依据定义而定)。这个跟泛解析的概念相类似。

---------------------------
DHCP配置
dnsmasq 配置文件(/etc/dnsmasq.conf)，必要的配置如下：

```bash
#选定需要侦听的网口
# Only listen to routers' LAN NIC.  Doing so opens up tcp/udp port 53 to
# localhost and udp port 67 to world:
interface=<LAN-NIC>

# dnsmasq will open tcp/udp port 53 and udp port 67 to world to help with
# dynamic interfaces (assigning dynamic ips). Dnsmasq will discard world
# requests to them, but the paranoid might like to close them and let the
# kernel handle them:
bind-interfaces

#设定可分配的ip地址段和租约时间
# Dynamic range of IPs to make available to LAN pc
dhcp-range=192.168.1.50,192.168.1.100,12h

#绑定某些机器的ip-mac地址对，使其具有固定的ip地址
# If you’d like to have dnsmasq assign static IPs, bind the LAN computer's
# NIC MAC address:
dhcp-host=aa:bb:cc:dd:ee:ff,192.168.1.50
dhcp-host=00:0e:7b:ca:1c:6e,daunbook,192.168.0.12
#为192.168.0.12设置主机名：dannbook

# dhcp动态分配的地址范围
dhcp-range=192.168.8.50,192.168.8.150,24h

# 同上，不过给出了掩码
#dhcp-range=192.168.8.50,192.168.8.150,255.255.255.0,12h

# dhcp服务的静态绑定
# dhcp-host=08:00:27:D1:CF:E2,192.168.8.201,infinite 无限租期
dhcp-host=08:00:27:D1:CF:E2,192.168.8.201,db2
dhcp-host=08:00:27:D6:F0:9F,192.168.8.202,db3

# 设置默认租期
# Set the limit on DHCP leases, the default is 150
#dhcp-lease-max=150

# 租期保存文件
#dhcp-leasefile=/var/lib/dnsmasq/dnsmasq.leases

# 通过/etc/hosts来分配对应的hostname
#dhcp-host=judge

# 忽略下面MAC地址的DHCP请求
#dhcp-host=11:22:33:44:55:66,ignore
 
# dhcp所在的domain
domain=freeoa.net
 
# 设置默认路由出口
# dhcp-option遵循RFC 2132（Options and BOOTP Vendor Extensions),可以通过dnsmasq --help dhcp来查看具体的配置
# 很多高级的配置，如iSCSI连接配置等同样可以由RFC 2132定义的dhcp-option中给出。
# option 3为default route
dhcp-option=3,192.168.8.1

# 设置NTP Server.这是使用option name而非选项名来进行设置
#dhcp-option=option:ntp-server,192.168.8.4,10.10.0.5

注意:当为某一MAC地址同时静态分配主机名和IP时，如果写到两条dhcp-host选项里（如下所示），则只会生效后面的一条。正确的选项写法如上配置。
dhcp-host=08:00:27:D1:CF:E2,192.168.8.201 dhcp-host=08:00:27:D1:CF:E2,db2
dhcp-host=08:00:27:D1:CF:E2,192.168.8.201
dhcp-host=08:00:27:D1:CF:E2,db2
```

重新启动客户端网卡。由于之前测试中客户端网卡已经申请了DHCP租期。所以这里需要修改租期文件，让客户端重新获得IP和hostname。


总结相关的配置选项如下：
> expand-hosts
domain=freeoa.net
dhcp-range=192.168.0.20,192.168.0.100,12h
dhcp-option=3,192.168.0.1

以上配置选项开启了DHCP服务，并且设置domain为“freeoa.net”。DHCP服务提供地址范围为 '192.168.0.20到 192.168.0.100' 续订期为12个小时。最后的一个选项指定了默认网关。

如果要配置静态地址，可以对dhcp-host选项作以下设置：
dhcp-host=00:0e:7b:ca:1c:6e,daunbook,192.168.0.12

这样就会对MAC地址 11:22:33:44:55:66 赋主机名为 daunbook (.freeoa.net) IP 地址 192.168.0.12。

dnsmasq另外一个特性是能够提供tftp服务，让网络启动(PXE)也得以实现。它可以设定默认MX记录，多种caching。提LDAP使用的SRV记录信息，PTR、SPF甚至是zeroconf记录等。


测试
测试一下 DNS 查询然后测量响应时间：

```bash
$ dig archlinux.org | grep "Query time"
```