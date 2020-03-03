# Linux_network_netstat网络系统的状态信息

来自: http://man.linuxde.net/netstat

**netstat命令用来打印Linux中网络系统的状态信息**，可让你得知整个Linux系统的网络情况。

## 选项

```bash
usage: netstat [-vWeenNcCF] [<Af>] -r         netstat {-V|--version|-h|--help}
       netstat [-vWnNcaeol] [<Socket> ...]
       netstat { [-vWeenNac] -i | [-cWnNe] -M | -s }

        -a, --all, --listening   显示所有连线中的Socket(default: connected)
        -c, --continuous         持续列出网络状态
        -C, --cache              显示路由器配置的快取信息
        -e, --extend             显示网络其他相关信息
        -F, --fib                显示 Forwarding Information Base (default)
        -g, --groups             显示多重广播功能群组组员名单
        -i, --interfaces         显示网络接口表(网络界面信息表单)
        -l, --listening          显示监控中的服务器的Socket
        -M, --masquerade         显示伪装的网络连线
        -n, --numeric            直接使用ip地址和端口号，而不通过域名服务器与端口服务名称
        -N, --symbolic           显示网络硬件外围设备的符号连接名称
        -o, --timers             显示计时器
        -p, --programs           显示正在使用Socket的程序识别码和程序名称
		-u, --udp				 显示UDP传输协议的连线状况
		-t, --tcp				 显示TCP传输协议的连线状况
        -r, --route              显示路由表
        -s, --statistics         显示网络工作信息统计表(like SNMP)
        -v, --verbose            显示版本
        -W, --wide               不使用截短 IP 地址
        --numeric-hosts          不使用解析 host 名称
        --numeric-ports          不使用解析 port 名称
        --numeric-users          不使用解析 user 名称

```

## 实例

### 网络列表
**列出所有端口 (包括监听和未监听的)**

```bash
$ netstat -a #列出所有端口
$ netstat -at #列出所有tcp端口
$ netstat -au #列出所有udp端口
```

**列出所有处于监听状态的 Sockets**

```bash
$ netstat -l #只显示监听端口
$ netstat -lt #只列出所有监听 tcp 端口
$ netstat -lu #只列出所有监听 udp 端口
$ netstat -lx #只列出所有监听 UNIX 端口
```

**显示每个协议的统计信息**

```bash
$ netstat -s #显示所有端口的统计信息
$ netstat -st #显示TCP端口的统计信息
$ netstat -su #显示UDP端口的统计信息
```

**在netstat输出中显示 PID 和进程名称**

```bash
$ netstat -pt
```

> **netstat -p 可以与其它开关一起使用，就可以添加“PID/进程名称”到netstat输出中**，这样debugging的时候可以很方便的发现特定端口运行的程序。

**显示当前正在监听的网络信息(输出包含端口号与程序名称)**
```bash
$ netstat -lnp	#以数字格式输出正在监听的pid和进程名称
```

### 查看路由表

```bash
$ netstat -r	#查看路由表
$ netstat -rn	#以数字形式查看路由表
```

### 查看网络表
```bash
$ netstat -i	#网络接口列表
$ netstat -ie	#网络接口列表,类似ifconfig
```