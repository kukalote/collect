# Nmap商品扫描



Nmap可以完成以下任务：

> - 主机发现（Host Discovery）
> - 端口扫描（Port Scanning）
> - 版本侦测（Version Detection）
> - 操作系统侦测（Operating System Detection）
> - 支持探测脚本的编写

端口扫描工具，即借助工具，试图了解所扫描IP提供的计算机网络服务类型（网络服务均与端口号相关），从而发现攻击弱点。

##### 确定目标主机在线情况及端口基本状况

```bash
nmap targetHost
```

##### 完整全面的扫描

```bash
nmap –T4 –A –v targetHost
```

>1. -T4指定扫描过程使用的时序（Timing），总有6个级别（0-5），级别越高，扫描速度越快，但也容易被防火墙或IDS检测并屏蔽掉，在网络通讯状况良好的情况推荐使用T4；
>2. 在未指定扫描端口时，Nmap默认扫描1000个最有可能开放的端口。

### 主机发现

主机发现（Host Discovery），即用于发现目标主机是否在线（Alive，处于开启状态）。

#### 1. 主机发现原理

主机发现发现的原理与Ping命令类似，发送探测包到目标主机，如果收到回复，那么说明目标主机是开启的。Nmap支持十多种不同的主机探测方式，比如发送`ICMP ECHO`/`TIMESTAMP`/`NETMASK`报文、发送`TCPSYN/ACK`包、发送`SCTP INIT`/`COOKIE-ECHO`包，用户可以在不同的条件下灵活选用不同的方式来探测目标机。

主机发现基本原理：（以`ICMP echo`方式为例）

![nmap_1_1](./images/nmap_1_1.jpg)

如果该请求报文没有被防火墙拦截掉，那么目标机会回复ICMP Echo Reply包回来。以此来确定目标主机是否在线。

默认情况下，Nmap会发送四种不同类型的数据包来探测目标主机是否在线。
> 1.  ICMP echo request
> 2.  a TCP SYN packet to port 443
> 3.  a TCP ACK packet to port 80
> 4.  an ICMP timestamp request

依次发送四个报文探测目标机是否开启。只要收到其中一个包的回复，那就证明目标机开启。使用四种不同类型的数据包可以避免因防火墙或丢包造成的判断错误。

#### 2. 主机发现的用法
通常主机发现并不单独使用，而只是作为端口扫描、版本侦测、OS侦测先行步骤。而在某些特殊应用（例如确定大型局域网内活动主机的数量），可能会单独专门适用主机发现功能来完成。

不管是作为辅助用法还是专门用途，用户都可以使用Nmap提供的丰富的选项来定制主机发现的探测方式。
>`-sL`        : List Scan 列表扫描，仅将指定的目标的IP列举出来，不进行主机发现。
>`-sn`        : Ping Scan 只进行主机发现，不进行端口扫描。
>`-Pn`        : 将所有指定的主机视作开启的，跳过主机发现的过程。
>`-PS/PA/PU/PY[portlist]`       : 使用TCPSYN/ACK或SCTP INIT/ECHO方式进行发现。
>`-PE/PP/PM`  		: 使用ICMP echo, timestamp, and netmask 请求包发现主机。
>`-PO[protocollist]`            : 使用IP协议包探测对方主机是否开启。
>`-n/-R`      		: -n表示不进行DNS解析；-R表示总是进行DNS解析。
>`--dns-servers <serv1[,serv2],...>`	: 指定DNS服务器。
>`--system-dns` 	: 指定使用系统的DNS服务器
>`--traceroute`    : 追踪每个路由节点

其中，比较常用的使用的是`-sn`，表示只单独进行主机发现过程；`-Pn`表示直接跳过主机发现而进行端口扫描等高级操作（如果已经确知目标主机已经开启，可用该选项）；`-n`，如果不想使用DNS或reverse DNS解析，那么可以使用该选项。

#### 3. 使用演示

**探测 scanme.nmap.org 主机**

```bash
nmap –sn –PE –PS80,135 –PU53 scanme.nmap.org
```
命令向 scanme.nmap.org 的IP地址182.140.147.57发送了四个探测包：ICMPEcho，80和135端口的TCP SYN包，53端口的UDP包（DNS domain）。

**探测局域网内活动主机**

```bash
nmap –sn 192.168.1.100-120
```

Nmap是通过ARP包来询问IP地址上的主机是否活动的，如果收到ARP回复包，那么说明主机在线。

### 端口扫描

端口扫描是Nmap最基本最核心的功能，用于确定目标主机的TCP/UDP端口的开放情况。
默认情况下，Nmap会扫描1000个最有可能开放的TCP端口。
Nmap通过探测将端口划分为6个状态：

>`open`   ：端口是开放的。
>`closed`   ：端口是关闭的。
>`filtered`   ：端口被防火墙IDS/IPS屏蔽，无法确定其状态。
>`unfiltered`   ：端口没有被屏蔽，但是否开放需要进一步确定。
>`open|filtered`   ：端口是开放的或被屏蔽。
>`closed|filtered`   ：端口是关闭的或被屏蔽。

#### 1. 端口扫描用法

端口扫描用法比较简单，Nmap提供丰富的命令行参数来指定扫描方式和扫描端口。

**扫描方式选项**

> `-sS/sT/sA/sW/sM`   :指定使用 TCP SYN/Connect()/ACK/Window/Maimon scans的方式来对目标主机进行扫描。 
> `-sU`   : 指定使用UDP扫描方式确定目标主机的UDP端口状况。 
> `-sN/sF/sX`   : 指定使用TCP Null, FIN, and Xmas scans秘密扫描方式来协助探测对方的TCP端口状态。 
> `--scanflags <flags>`   : 定制TCP包的flags。 
> `-sI <zombiehost[:probeport]>`   : 指定使用idle scan方式来扫描目标主机（前提需要找到合适的zombie host） 
> `-sY/sZ`   : 使用SCTP INIT/COOKIE-ECHO来扫描SCTP协议端口的开放的情况。 
> `-sO`   : 使用IP protocol 扫描确定目标机支持的协议类型。 
> `-b <FTP relay host>`   : 使用FTP bounce scan扫描方式

**端口参数与扫描顺序**

>`-p <port ranges>`   : 扫描指定的端口 
>实例: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9（其中T代表TCP协议、U代表UDP协议、S代表SCTP协议） 
>`-F`   : Fast mode – 快速模式，仅扫描TOP 100的端口 
>`-r`   : 不进行端口随机打乱的操作（如无该参数，nmap会将要扫描的端口以随机顺序方式扫描，以让nmap的扫描不易被对方防火墙检测到）。 
>`--top-ports <number>`   :扫描开放概率最高的number个端口（nmap的作者曾经做过大规模地互联网扫描，以此统计出网络上各种端口可能开放的概率。以此排列出最有可能开放端口的列表，具体可以参见文件：nmap-services。默认情况下，nmap会扫描最有可能的1000个TCP端口） 
>`--port-ratio <ratio>`   : 扫描指定频率以上的端口。与上述--top-ports类似，这里以概率作为参数，让概率大于--port-ratio的端口才被扫描。显然参数必须在在0到1之间，具体范围概率情况可以查看nmap-services文件。

#### 2. 端口扫描演示

```bash
nmap –sS –sU –T4 –top-ports 300 192.168.1.100
```

参数`-sS`表示使用TCP SYN方式扫描TCP端口；`-sU`表示扫描UDP端口；`-T4`表示时间级别配置4级；`--top-ports 300`表示扫描最有可能开放的300个端口（TCP和UDP分别有300个端口）。

### 版本侦测

版本侦测，用于确定目标主机开放端口上运行的具体的应用程序及版本信息。

Nmap提供的版本侦测具有如下的优点：

>高速。  并行地进行套接字操作，实现一组高效的探测匹配定义语法。
>尽可能地确定应用名字与版本名字。
>支持TCP/UDP协议，支持文本格式与二进制格式。
>支持多种平台服务的侦测，包括Linux/Windows/Mac OS/FreeBSD等系统。
>如果检测到SSL，会调用openSSL继续侦测运行在SSL上的具体协议（如HTTPS/POP3S/IMAPS）。
>如果检测到SunRPC服务，那么会调用brute-force RPC grinder进一步确定RPC程序编号、名字、版本号。
>支持完整的IPv6功能，包括TCP/UDP，基于TCP的SSL。
>通用平台枚举功能（CPE）
>广泛的应用程序数据库（nmap-services-probes）。目前Nmap可以识别几千种服务的签名，包含了180多种不同的协议。

#### 1. 版本侦测原理

版本侦测主要分为以下几个步骤：

> 1. 首先检查open与open|filtered状态的端口是否在排除端口列表内。如果在排除列表，将该端口剔除。
> 2. 如果是TCP端口，尝试建立TCP连接。尝试等待片刻（通常6秒或更多，具体时间可以查询文件nmap-services-probes中Probe TCP NULL q||对应的totalwaitms）。通常在等待时间内，会接收到目标机发送的“WelcomeBanner”信息。nmap将接收到的Banner与nmap-services-probes中NULL probe中的签名进行对比。查找对应应用程序的名字与版本信息。
> 3. 如果通过“Welcome Banner”无法确定应用程序版本，那么nmap再尝试发送其他的探测包（即从nmap-services-probes中挑选合适的probe），将probe得到回复包与数据库中的签名进行对比。如果反复探测都无法得出具体应用，那么打印出应用返回报文，让用户自行进一步判定。
> 4. 如果是UDP端口，那么直接使用nmap-services-probes中探测包进行探测匹配。根据结果对比分析出UDP应用服务类型。
> 5. 如果探测到应用程序是SSL，那么调用openSSL进一步的侦查运行在SSL之上的具体的应用类型。
> 6. 如果探测到应用程序是SunRPC，那么调用brute-force RPC grinder进一步探测具体服务。


#### 2. 版本侦测的用法

版本侦测方面的命令行选项比较简单。

>`-sV`   : 指定让Nmap进行版本侦测
>`--version-intensity <level>`   : 指定版本侦测强度（0-9），默认为7。数值越高，探测出的服务越准确，但是运行时间会比较长。
>`--version-light`   : 指定使用轻量侦测方式 (intensity 2)
>`--version-all`   : 尝试使用所有的probes进行侦测 (intensity 9)
>`--version-trace`   : 显示出详细的版本侦测过程信息。

#### 3. 版本侦测演示

```bash
nmap –sV 192.168.1.100
```




常见服务对应端口号：

| 服务                                         | 端口号 |
| -------------------------------------------- | ------ |
| HTTP                                         | 80     |
| HTTPS                                        | 443    |
| Telnet                                       | 23     |
| FTP                                          | 21     |
| SSH（安全登录）、SCP（文件传输）、端口重定向 | 22     |
| SMTP                                         | 25     |
| POP3                                         | 110    |
| WebLogic                                     | 7001   |
| MySQL 数据库sever                            | 3306   |