# Linux_cmd_ps命令.md

<!-- create time: 2016-05-25 10:16:35  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

[基础知识](#kno)  
[参数列表](#param)  
[使用实例](#example)  
[其他参数形式](#other_param)

[Linux](id:kno) 中的`ps`命令是**Process Status**的缩写。`ps`命令用来列出系统中当前运行的那些进程。`ps`命令列出的是当前那些进程的**快照**，就是执行`ps`命令的那个时刻的那些进程，如果想要动态的显示进程信息，就可以使用`top`命令。


**linux上进程有5种状态** : 

1. 运行(正在运行或在运行队列中等待) 
2. 中断(休眠中, 受阻, 在等待某个条件的形成或接受到信号) 
3. 不可中断(收到信号不唤醒和不可运行, 进程必须等待直到有中断发生) 
4. 僵死(进程已终止, 但进程描述符存在, 直到父进程调用wait4()系统调用后释放) 
5. 停止(进程收到SIGSTOP, SIGSTP, SIGTIN, SIGTOU信号后停止运行运行) 

**ps工具标识进程的5种状态码**: 

- D 不可中断 uninterruptible sleep (usually IO)   
- R 运行 runnable (on run queue)   
- S 中断 sleeping   
- T 停止 traced or stopped   
- Z 僵死 a defunct (”zombie”) process   

[](id:param)
**ps 命令的 Unix参数 **  

参数 | 描述
----|---
`-A` | 显示所有进程
`-N` | 显示特定参数的结果的补集
`-a` | 显示除会话标题和无终端进程外的所有进程
`-d` | 显示除会话题外的所有进程
`-e` | 显示所有进程
`-C cmdlist` | 显示包含在 cmdlist 列表中的进程
`-G grplist` | 显示 grplist 列表中具有组 ID 的进程
`-U userlist` | 显示 userlist 列表中的 userid 拥有的进程
`-g grplist` | 根据会话或包含在 grplist 中的 groupid 来显示进程
`-p pidlist` | 显示 pidlist 列表中具有 PID 的进程
`-s sesslist` | 显示 sesslist 列表中具有会话 ID 的进程
`-t ttylist` | 显示 ttylist 列表中具有终端 ID 进程
`-u userlist` | 根据 userlist 列表占的有效 userid 来显示进程
`-F` | 使用额外完整输出
`-O format` | 显示 format 列表中的特定列和默认列
`-M` | 显示关于进程的安全信息
`-c` | 显示关于进程的额外调试信息
`-f` | 显示完整的格式列表 
`-j` | 显示作业信息
`-l` | 显示长列表
`-o format` | 只显示 format 中列出的特定列
`-y` | 不显示进程标记
`-Z` | 显示安全上下文信息
`-H` | 以层级格式显示进程(显示父进程)
`-n namelist` | 定义在 WCHAN 列中显示的值 
`-w` | 使用宽输出格式，使显示宽度不受限制
`-L` | 显示进程线程
`-V` | 显示 ps 的版本

**输出名词解释**
>- `UID` : 负责启动进程的用户；
- `PID` : 进程的 ID；
- `PPID` : 父进程的 PID (如果某个进程由另一个进程启动)
- `C` : 进程存续期的处理器利用率；
- `STIME` : 进程启动时的系统时间；
- `TTY` : 进程从中启动的终端设备；
- `TIME` : 运行进程所需的累计 CPU 时间；
- `CMD` : 启动程序的名称。
- `F` : 内核分配给进程的系统标记；
- `S` : 进程的状态 (O=在处理器上运行； S=睡眠； R=可运行，等待运行； Z=死进程，进程已禁止，但父进程不可用； T=进程已停止)；
- `PRI` : 进程的优先级 (数字越大表示优先级越低)；
- `NI` : nice value，用于判断优先级；
- `ADDR` : 进程的内存地址；
- `SZ` : 换出进程大致需要的交换空间；
- `WCHAN` : 进程睡眠时所在的内核函数的地址。



[](id:example)
**实用示例** : 

1. 显示所有进程信息

        $ ps -A 
        
2. 显示指定用户信息

        $ ps -u username
3. ps 与grep 常用组合用法，查找特定进程

        $ ps -ef | grep ssh
        
4. 将目前属于您自己这次登入的 PID 与相关信息列示出来

        $ ps -l 
        
5. 列出类似程序树的程序显示

        $ ps axjf
 
      
6. 列出目前所有的正在内存当中的程序

        $ ps aux
        
输出：

> USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND  
    root         1  0.0  0.0  10368   676 ?        Ss   Nov02   0:00 init [3]  
            
说明：  

> `USER`：该 process 属于那个使用者账号的    
    >`PID `：该 process 的号码  
    >`%CPU`：该 process 使用掉的 CPU 资源百分比  
    >`%MEM`：该 process 所占用的物理内存百分比  
    >`VSZ` ：该 process 使用掉的虚拟内存量 (Kbytes)  
    >`RSS` ：该 process 占用的固定的内存量 (Kbytes)  
    >`TTY` ：该 process 是在那个终端机上面运作，若与终端机无关，则显示 ?，另外,tty1-tty6 是本机上面的登入者程序，若为 pts/0 等等的，则表示为由网络连接进主机的程序。  
    >`STAT`：该程序目前的状态，主要的状态有  
    >`R `：该程序目前正在运作，或者是可被运作  
    >`S` ：该程序目前正在睡眠当中 (可说是 idle 状态)，但可被某些讯号 (signal) 唤醒。  
    >`T` ：该程序目前正在侦测或者是停止了  
    >`Z` ：该程序应该已经终止，但是其父程序却无法正常的终止他，造成 zombie (疆尸) 程序的状态  
    >`START`：该 process 被触发启动的时间  
    >`TIME` ：该 process 实际使用 CPU 运作的时间  
    >`COMMAND`：该程序的实际指令  



7. 找出与 cron 与 syslog 这两个服务有关的 PID 号码

        $ ps aux | egrep '(cron|syslog)'


其他实例：

1. 可以用 | 管道和 more 连接起来分页查看
    
       
        $ ps -aux |more
    
2. 把所有进程显示出来，并输出到ps001.txt文件

        $ ps -aux > ps001.txt
    
3. 输出指定的字段
    
        $ ps -o pid,ppid,pgrp,session,tpgid,comm	
    
    >输出：
    [root@localhost test6]# ps -o pid,ppid,pgrp,session,tpgid,comm      
    PID  PPID  PGRP  SESS TPGID COMMAND      
    17398 17394 17398 17398 17478 bash      
    17478 17398 17478 17398 17478 ps





[](id:other_param)
**ps 命令的 BSD 参数 **

参数 | 描述
----|---
`T` | 显示与该终端相关的所有进程
`a` | 显示与任何终端相关的所有进程
`g` | 显示包括会话标题在内的所有进程
`r` | 只显示正在运行的进程
`x` | 显示所有进程，包括未分配终端设备的进程
`U userlist` | 显示 userlist 列表中的 userid 拥有的进程
`p pidlist` | 显示在 pidlist 列表中的有 PID 的进程
`t ttylist` | 显示与 ttylist 列表中某个终端相关的进程
`O format` | 列出 format 中的具体列，与标准列一并显示
`X` | 以寄存器格式显示数据
`Z` | 在输出中包含安全信息
`j` | 显示作业信息
`l` | 显示长格式
`o` format | 只显示 format 中指定的列
`s` | 使用信号格式
`u` | 使用面向用户的格式
`v` | 使用虚拟内存格式
`N namelist` | 定义在 WCHAN 列中使用的值
`O order` | 定义显示信息列的顺序
`S` | 针对父进程中的子进程的合计数值信息，比如说 CPU 和内存使用率
`c` | 显示真实命令名称 (用于启动进程的程序名)
`e` | 显示命令使用的任何环境变量
`f` | 以层次结构显示进程，显示哪个进程启动了哪个进程
`h` | 不显示标题信息
`k sort` | 定义输出排序使用的列
`n` | 使用数值表示用户和组 ID ，以及 WCHAN 信息
`w` | 为较宽的终端生成宽输出 
`H` | 将线程作为进程显示
`m` | 在进程后显示线程
`L` | 列出所有格式说明符
`V` | 显示 ps 的版本

*名词解释*
> - `VSZ` : 进程在内存中的大小，以 KB 为单位；
- `RSS` : 进程使用过的且未被换出的物理内存；
- `STAT` : 由两个字符组成的状态码，用于表示当前进程的状态，第一个字符用与 Unix 类型的 S 输出列相同的值，显示进程正在睡眠、运行还是等待。第二个字符进一步定义进程的状态。
    - `<` : 进程正以高优先级运行。
    - `N` : 进程正以低优先级运行。
    - `L`  : 进程在内存中存在锁定页面。
    - `s` : 进程是会话领导者 (session leader)
    - `l` : 进程是多线程的。
    - `+` : 进程正在前台运行。

**ps 命令的 GNU 参数 **

参数 | 描述
----|---
`--deselect` | 显示在命令行中列出的进程之外的所有其他进程
`--Group grplist` | 显示组 ID 在 grplist 中列出的进程
`--User userlist` | 显示用户 ID 在 userlist 中列出的进程
`--group grplist` | 显示有效组 ID 在 grplist 中列出的进程
`--pid pidlist` | 显示进程 ID 在 pidlist 中列出的进程
`--ppid pidlist` | 显示父进程 ID 在 pidlist 中列出的进程
`--sid sidlist` | 显示会话 ID 在 sidlist 中列出的进程
`--tty ttylist` | 显示终端设备 ID 在 ttylist 中列出的进程
`--user userlist` | 显示有效用户 ID 在 userlist 中列出的进程
`--format format` | 只显示在 format 中指定的列
`--context` | 显示额外的安全信息
`--cols n` | 将屏幕宽度设置为 n 列
`--columns n` | 将屏幕高度设置为 n 行
`--cumulative` | 包含已停止子进程的信息
`--forest` | 在层次结构清单中显示进程，以显示父进程 (***)
`--headers` | 对每页输出重新列头 (column headers)
`--no-headers` | 不显示列头
`--lines n` | 将屏幕宽度设置为 n 列
`--rows n` | 将屏幕高度设置为 n 行
`--sort order` | 定义用于排序输出的列
`--width n` | 将屏幕宽度设置为 n 列
`--help` | 显示帮助信息
`--info` | 显示调试信息
`--version` | 显示 ps 程序的版本


