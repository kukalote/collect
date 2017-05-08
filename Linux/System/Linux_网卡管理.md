# Linux_网卡管理

## ifup 与 ifdown 命令启用与停止网卡配置

```bash
#启动网卡 eth0
$ ifup eth0
$ ifconfig eth0 up
#关闭网卡 eth0
$ ifdown eth0
```

> ifup 与 ifdown 仅能就 /etc/sysconfig/network-scripts 内的 ifcfg-ethx 进行启动或关闭，并不能直接修改网络的参数，除非手动的调整 ifcfg-ethx 文件才行。

## ifconfig 网卡配置

ifconfig 则可以直接手动给予某个界面 IP 或修改网络参数。

```bash
$ ifconfig -a 	# 展示所有活动的网卡
$ ifconfig {interface} {up|dowon}	#启动或关闭某个网卡
$ ifconfig interface {options}		#配置某个网卡
```

> 参数 :

> interface(网卡名称),包括 eth0,eth1,ppp0等
> options : 可以接收参数如下 :
> - up, down : 启动(up) 和 关闭(down) 某个网上
> - mtu		 : 可设定不同的 MTU 数值，例如 mtu 1500(单位为 byte)
> - netmask	 : 子网掩码
> - broadcast: 广播地址

```bash
#将 eth0 网卡设置ip 为 192.168.56.100
$ ifconfig eth0 192.168.56.100

#将 eth0 网卡设置ip 为 192.168.56.100至300之间的ip
$ ifconfig eth0 192.168.56.100/130

#手动定义子网掩码
$ ifconfig eth0 192.168.56.100 netmask 255.255.255.0
```

## route 路由修改

**Gateway / Router** ：`网关/路由器的功能就是在负责不同网域之间的封包转递 (IP Forwarding)，由于路由器具有 IP Forwarding 的功能，并且具有管理路由的能力， 所以可以将来自不同网域之间的封包进行转递的功能`。此外，你的主机与你主机设定的 Gateway 必定是在同一个网段内喔！

```bash
# 查看路由表
$ route
$ route -n

# 增加路由
$ route add -net 192.168.100.0 netmask 255.255.255.0 dev eth0

# 删除路由
$ route del -net 169.254.0.0 netmask 255.255.0.0 dev eth0
```

## ip 网络综合指令

基本 ip 指令是整合了 ifconfig 和 route 这两指令。还有 ifup 就是使用 ip 指令来达成的。

```bash
$ ip [option] [动作] [指令]
选项与参数：
option ：设定的参数，主要有：
    -s ：显示出该装置的统计数据(statistics)，例如总接受封包数等；
动作：亦即是可以针对哪些网络参数进行动作，包括有：
    link  ：关于装置 (device) 的相关设定，包括 MTU, MAC 地址等等
    addr/address ：关于额外的 IP 协议，例如多 IP 的达成等等；
    route ：与路由有关的相关设定
```

#### 1. 关于装置接口 (device) 的相关设定： ip link

ip link 可以设定与装置 (device) 有关的相关参数，包括 MTU 以及该网络接口的 MAC 等等，当然也可以启动 (up) 或关闭 (down) 某个网络接口。

```bash
# 显示出所有的接口信息
$ ip link show
$ ip -s link show eth0
```

使用 ip 指令设置参数 :

```bash
# 启动/关闭与设置装置相关信息
$ ip link set eth0 up
$ ip link set eth0 down
$ ip link set eth0 mtu 1000

# 修改网卡代号，mac地址等
$ ip link set eth0 name vbird
$ ip link set eth0 address aa:aa:aa:aa:aa:aa
```

#### 2. 关于额外的 IP 相关设定： ip address

ip address (ip addr) 就是与第三层网络层有关的参数啦！ 主要是在设定与 IP 有关的各项参数，包括 netmask, broadcast 等等。

```bash
# 显示出所有的接口之 IP 参数：
$ ip address show
```

```bash
# 新增一个接口，名称假设为 eth0:vbird
$ ip address add 192.168.50.50/24 broadcast + dev eth0 label eth0:vbird

# 将刚刚的界面删除
$ ip address del 192.168.50.50/24 dev eth0
```

#### 3. 关于路由的相关设定： ip route

事实上， ip route 的功能几乎与 route 这个指令差不多，但是，他还可以进行额外的参数设计，例如 MTU 的规划等等，相当的强悍啊！

```bash
# 展示路由表
$ ip route show

# 路由添加
$ ip route add 192.168.10.0/24 via 192.168.5.100 dev eth0
$  ip route add default via 192.168.1.254 dev eth0	# 增加预设路由
# 路由删除
$ ip route del 192.168.10.0/24
```


## 网络配置相关文件
1. /etc/sysconfig/network-scripts/*	#网卡配置文件，网卡命令
2. /etc/resolv.conf			#配置DNS
3. /etc/sysconfig/network	#设置主机名，网关，域名
4. /etc/hosts				#本地主机ip地址映射,可以有多个别名
5. /etc/network/interfaces	# Dabian/Ubuntu 网络配置