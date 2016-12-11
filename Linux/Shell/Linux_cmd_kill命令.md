# Linux_cmd_kill命令.md

<!-- create time: 2016-05-25 10:16:18  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

Linux中的`kill`命令用来终止指定的进程（terminate a process）的运行，是Linux下进程管理的常用命令。通常，终止一个前台进程可以使用`Ctrl+C`键，但是，对于一个后台进程就须用`kill`命令来终止，我们就需要先使用`ps`/`pidof`/`pstree`/`top`等工具获取进程**PID**，然后使用`kill`命令来杀掉该进程。`kill`命令是通过向进程发送指定的信号来结束相应进程的。

在默认情况下，采用编号为15的**TERM**信号。**TERM**信号将终止所有不能捕获该信号的进程。对于那些可以捕获该信号的进程就要用编号为9的`kill`信号，强行“杀掉”该进程。 




### 命令功能：

发送指定的信号到相应进程。不指定型号将发送SIGTERM（15）终止指定进程。如果任无法终止该程序可用“-KILL” 参数，其发送的信号为SIGKILL(9) ，将强制结束进程，使用ps命令或者jobs 命令可以查看进程号。root用户将影响用户的进程，非root用户只能影响自己的进程。

### 管理背景当中的工作: kill

    [root@www ~]# kill -signal %jobnumber    [root@www ~]# kill -l    选项与参数:
    命令功能：
    -a  当处理当前进程时，不限制命令名和进程号的对应关系
    -p  指定kill 命令只打印相关进程的进程号，而不发送任何信号
    -s  指定发送信号
    -u  指定用户     -l :这个是 L 的小写,列出目前 kill 能够使用的讯号 (signal) 有哪些?    signal :代表给予后面接的那个工作什么样的指示啰!用 man 7 signal 可知:    -1 :重新读取一次参数的配置文件 (类似 reload);    -2 :代表与由键盘输入 [ctrl]-c 同样的动作;    -9 :立刻强制删除一个工作;     -15:以正常的程序方式终止一项工作。与 -9 是不一样的。
原文
     Some of the more commonly used signals:
     1       HUP (hang up)
     2       INT (interrupt)
     3       QUIT (quit)
     6       ABRT (abort)
     9       KILL (non-catchable, non-ignorable kill)
     14      ALRM (alarm clock)
     15      TERM (software termination signal)
     
### 实例

#### 1. 列出所有信息

    # kill -l
    
#### 2. 得到指定信号的数值
    
    # kill -l KILL
    # kill -l SIGKILL
    # kill -l SIGTERM
    
#### 3. 先用ps查找进程，然后用kill杀掉

    # ps -ef|grep vim
    # kill 3268
    
#### 4. 彻底杀死进程
    
    # kill –9 3268 

#### 5. 杀死指定用户所有进程

    //方法一: 将含有用户进程的全杀死
    # kill -9 $(ps -ef | grep peidalinux) 

    //方法二: 使用系统命令
    # kill -u peidalinux    