# 用docker配置开发环境

## Dockerfile 制作Centos7基础镜像

```dockerfile
FROM centos:7
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
```

Dockerfile 删除一些可能导致问题的单元文件。再执行下面命令便可以生成基础镜像。

```bash
$ docker build --rm -t local/c7-systemd .
$ docker run -ti --privileged=true -v /sys/fs/cgroup:/sys/fs/cgroup:ro -p 80:80 local/c7-systemd /bin/bash
```

## systemd啟用的應用程序容器


为了使之前容器systemd可用，需要使用如下命令。

```dockerfile
FROM local/c7-systemd
RUN yum -y install nginx; yum clean all; systemctl enable nginx.service
EXPOSE 80
CMD ["/usr/sbin/init"]
```

生成镜像:

```bash
$ docker build --rm -t local/c7-systemd-nginx .
```

## 启动应用程序容器

```bash
$ docker run -ti -v /var/www:/var/www -p 80:80 local/c7-systemd-nginx
$ docker run -ti --privileged=true -v /sys/fs/cgroup:/sys/fs/cgroup:ro -p 80:80 local/c7-systemd-nginx
```





