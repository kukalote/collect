# Linux_目录分析

<!-- create time: 2016-07-18 23:39:28  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

- `/etc` : 包括绝大多数 Linux 系统引导所需要的配置文件, 系统引导时读取配置文件, 按照配置文件的选项进行不同情况的启动, 例如 fstab、host.conf 等。
- `/lib` : 包含 C 编译程序需要的函数库, 是一组二进制文件, 例如 glibc 等。
- `/usr` : 包括所有其他内容, 如 src、local。 Linux 的内核就在 `/usr/src` 中。其下有子目录 `/bin`, 存放所有安装语言的命令, 如 gcc、perl 等。
- `/var` : 包含系统定义表, 以便在系统运行改变时可以只备份该目录, 如 cache。
- `/tmp` : 用于临时性的存储。
- `/bin` : 大多数命令存放在这里。
- `/home` : 主要存放用户账号, 并且可以支持 ftp 的用户管理。系统管理员增加用户时, 系统在 home 目录下创建与用户同名的目录, 此目录下一般默认有 Desktop 目录。
- `/dev` : 这个目录下存放一种设备文件的特殊文件, 如果 fd0、had 等。
- `/mnt` : 在 Linux 系统中, 它是专门给外挂的文件系统使用的, 里面有两个文件 cdrom、floopy, 登录光驱、软驱时要用到。