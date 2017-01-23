# crond 服务

## crond 服务

1. 安装 crond 服务

		yum install crontabs

2. 查看是否安装服务

		service crond status

3. 服务操作

		service crond start
		service crond stop
		service crond restart
		service crond reload

## 目录与管理


**用户任务调度**：用户定期要执行的工作，比如用户数据备份、定时邮件提醒等。用户可以使用 crontab 工具来定制自己的计划任务。所有用户定义的crontab 文件都被保存在 `/var/spool/cron` 目录中。其文件名与用户名一致。


使用者权限文件|-
----|---
**文件**| `/etc/cron.deny`
**说明** | 该文件中所列用户不允许使用crontab命令
文件| `/etc/cron.allow`
说明| 该文件中所列用户允许使用crontab命令
**文件**| `/var/spool/cron/`
**说明**| 所有用户crontab文件存放的目录,以用户名命名