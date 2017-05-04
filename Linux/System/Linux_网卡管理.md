# Linux_网卡管理

## ifup 与 ifdown 命令启用与停止网卡配置

```bash
//启动网卡 eth0
$ ifup eth0
//关闭网卡 eth0
$ ifdown eth0
```

> ifup 与 ifdown 仅能就 /etc/sysconfig/network-scripts 内的 ifcfg-ethx 进行启动或关闭，并不能直接修改网络的参数，除非手动的调整 ifcfg-ethx 文件才行。

## ifconfig 网卡配置

ifconfig 则可以直接手动给予某个界面 IP 或修改网络参数。

```bash
$ ifconfig -a 	# 展示所有活动的网卡
$ ifconfig {interface} {up|dowon} #启动或关闭某个网卡
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
$ ifconfig eth0 192.168.56.100
#将 eth0 网卡设置ip 为 192.168.56.100
$ ifconfig eth0 192.168.56.100/130
#将 eth0 网卡设置ip 为 192.168.56.100至300之间的ip
```