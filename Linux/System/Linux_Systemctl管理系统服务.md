# Linux systemctl 管理系统服务

**Systemd是一个Linux操作系统下的系统和服务管理器**。它被设计成向后兼容SysV启动脚本，并提供了大量的特性，如开机时平行启动系统服务，**按需启动守护进程，支持系统状态快照，或者基于依赖的服务控制逻辑**。
先前的使用SysV初始化或Upstart的红帽企业版Linux版本中，使用位于/etc/rc.d/init.d/目录中的bash初始化脚本进行管理。而在RHEL 7/CentOS 7中，这些启动脚本被服务单元取代了。服务单元以.service文件扩展结束，提供了与初始化脚本同样的用途。要查看、启动、停止、重启、启用或者禁用系统服务，**你要使用systemctl来代替旧的service命令**。

> Tips : 为了向后兼容，旧的service命令在CentOS 7中仍然可用，它会重定向所有命令到新的systemctl工具。

### 使用systemctl来启动/停止/重启服务

```bash
$ systemctl stop httpd.service	# 停止服务
$ systemctl start httpd.service	# 启动服务
$ systemctl restart httpd.service	# 重启服务
$ systemctl try-restart httpd.service	# 运行状态偿试重启服务
$ systemctl reload httpd.service	# 重载配置
```

### 检查服务状态

```bash
$ systemctl status httpd.service
$ systemctl status httpd.service -l # 非省略显示
```

### 使用启用/禁用服务来控制开机启动

```bash
$ systemctl enable httpd.service
$ systemctl disable httpd.service
```

### 服务展示

```bash
$ systemctl list-unit-files	# 列出全部服务
$ systemctl list-units --type service | grep running	# 列出正在跑的服务
```

###系统动作层级操作

```bash
$ systemctl get-default	#显示目前系统预设动作层级
$ systemctl isolate multi-user.target	# 切换动作层级至文字模式 runlevel 3
$ systemctl set-default multi-user.target	# 设定开机启动至文字模式 runlevel3
```
### 其他

```bash
$ systemctl kill httpd.service	# 杀死服务
$ systemctl show httpd.service	# 展示服务属性
```