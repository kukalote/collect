# Linux 中 终端发送消息命令 wall / write
引自 : http://blog.csdn.net/cloudeagle_bupt/article/details/28933281
系统 : centos x64

### 登陆服务器用户信息
通常我们了天需要通过QQ或页面的ajax机制，但如果我们是在同一服务器上登陆的不同终端，却是可以直接联系其他人的。

首先我们需要了解都是谁登陆了服务器,
```bash
$ w
 16:51:00 up 15:15,  3 users,  load average: 0.13, 0.04, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    192.168.56.1     Tue10   28.00s  3:31   3.47s -zsh
root     pts/1    192.168.56.1     Tue11    4.00s  7.13s  0.00s w
root     pts/2    192.168.56.1     Tue11    3:40   0.17s  0.17s -zsh
```
还有一个显示较简洁的命令 :
```bash
$ who
root     pts/0        2017-12-05 10:50 (192.168.56.1)
root     pts/1        2017-12-05 11:19 (192.168.56.1)
root     pts/2        2017-12-05 11:20 (192.168.56.1)
```
如果不清楚哪个是自己的终端，可以试用此命令 :
```bash
$ tty
/dev/pts/1
```

上面我们要的主要信息是 用户名，登陆终端号(tty7, pts/0).

### 广播命令
广播，就是向所有在线的用户发送消息，所以这里是不需要用户名和终端号的，格式如下 :
```bash
$ wall "hello good morning"
Broadcast message from root@localhost.localdomain (pts/0) (Wed Dec  6 16:27:56 2017):

hello good morning
```
与此同时其他终端将有如下消息提示 :
```bash
Broadcast message from root@localhost.localdomain (pts/0) (Wed Dec  6 16:27:56 2017):

hello good morning
```

### 定向发送消息
定向发送消息是指我们可以指定接收消息终端，此处用到了用户名和终端号，其格式如下 :
```bash
$ write root pts/0
hello this is the message
^C#
```
此刻 root 用户的 pts/0终端将会显示如下内容 :
```bash
Message from root@localhost.localdomain on pts/1 at 16:05 ...
hello this is the message
EOF
```
此命令的其他用法
```bash
$ write user [tty] #终端为可选项
```

### 禁收消息
很烦，烦死了，我不想接收消息怎么办?给你个方案，用 **mesg** 命令吧
```bash
$ mesg n	# 禁收消息
$ mesg y	# 接收消息
```

### 注意事项
消息虽好，但这里有需要注意的事项 :
1. 不能发送中文，非常可惜;
2. 接收者在终端打开文件，有可能会将接收消息插入到文件中;