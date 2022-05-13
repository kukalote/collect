### 增强工具

虚拟系统：Centos7

```bash
mkdir /media/cdrom

mount -t iso9660 /dev/cdrom /media/cdrom

cd /media/cdrom

./VBoxLinuxAdditions.run

# 问题1：看是否需要安装依赖库

yum install -y bzip2 gcc make

# 问题2：看是否需要更新系统
#查看内核版本
uname -a

yum update kernel -y

yum install kernel-headers-3.10.0-1160.59.1.el7.x86_64 kernel-devel-3.10.0-1160.59.1.el7.x86_64

# 重启-之后再进行绑定、执行安装脚本
init 6
```





