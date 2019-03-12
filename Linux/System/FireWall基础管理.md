# firewall 基础管理

### 服务管理
```bash
# 禁用，禁止开机启动： 
$ systemctl disable firewalld
# 启用，开机启用： 
$ systemctl enable firewalld
# 启动：
$ systemctl start firewalld
# 查看状态： 
$ systemctl status firewalld 
# 停止运行： 
$ systemctl stop firewalld
# 查看启动失败的服务列表：
$ systemctl --failed 
```
### 基础管理
```bash
#查看版本：
$ firewall-cmd --version
# 查看帮助： 
$ firewall-cmd --help
# 显示状态： 
$ firewall-cmd --state
# 查看所有打开的端口： 
$ firewall-cmd --zone=public --list-ports
# 更新防火墙规则： 
$ firewall-cmd --reload
# 更新防火墙规则，重启服务： 
$ firewall-cmd --completely-reload
# 查看已激活的Zone信息: 
$  firewall-cmd --get-active-zones
# 查看指定接口所属区域：
$  firewall-cmd --get-zone-of-interface=eth0
# 拒绝所有包：
$ firewall-cmd --panic-on
# 取消拒绝状态： 
$ firewall-cmd --panic-off
# 查看是否拒绝：
$ firewall-cmd --query-panic
```

### 信任级别，通过Zone的值指定

域值 | 信任级别 
---|---
drop| 丢弃所有进入的包，而不给出任何响应 
block| 拒绝所有外部发起的连接，允许内部发起的连接 
public| 允许指定的进入连接 
externa| 同上，对伪装的进入连接，一般用于路由转发 
dmz| 允许受限制的进入连接 
work| 允许受信任的计算机被限制的进入连接，类似 workgroup 
home| 同上，类似 homegroup 
internal| 同上，范围针对所有互联网用户 
trusted| 信任所有连接

### 常用操作

#### firewall管理 端口
```bash
# 添加开放端口， --permanent永久生效，没有此参数重启后失效
$ firewall-cmd --zone=public --add-port=80/tcp --permanent
# 查看
$ firewall-cmd --zone=public --query-port=80/tcp
# 删除
$ firewall-cmd --zone=public --remove-port=80/tcp --permanent
```
#### firewall管理 服务
```bash
# 以smtp服务为例， 添加到work zone
$ firewall-cmd --zone=work --add-service=smtp
# 查看：
$ firewall-cmd --zone=work --query-service=smtp
# 删除
$ firewall-cmd --zone=work --remove-service=smtp
```
#### firewall管理 IP伪装
```bash
# 查看：
$ firewall-cmd --zone=external --query-masquerade
# 打开：
$ firewall-cmd --zone=external --add-masquerade
# 关闭：
$ firewall-cmd --zone=external --remove-masquerade
```

#### firewall管理 端口转发

打开端口转发，首先需要打开IP地址伪装`firewall-cmd --zone=external --add-masquerade`

```bash
# 转发 tcp 22 端口至 3753：
$ firewall-cmd --zone=external --add-forward-port=22:porto=tcp:toport=3753
# 转发端口数据至另一个IP的相同端口：
$ firewall-cmd --zone=external --add-forward-port=22:porto=tcp:toaddr=192.168.1.112
# 转发端口数据至另一个IP的 3753 端口：
$ firewall-cmd --zone=external --add-forward-port=22:porto=tcp:toport=3753:toaddr=192.168.1.112
```

