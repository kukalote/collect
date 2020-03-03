### Linux_管理员使用的 shell 脚本

### 27 [管理员使用的 shell 脚本](id:toc)

- 27.1 [监视系统统计信息](#监视系统统计信息)
    - 27.1.1 [监视磁盘空闲空间](#监视磁盘空闲空间)
    - 27.1.2 [谁在霸占磁盘资源](#谁在霸占磁盘资源)
    - 27.1.3 [监视 CPU 和内存使用情况](#监视 CPU 和内存使用情况)
- 27.2 [执行备份](#执行备份)
    - 27.2.1 [归档数据文件](#归档数据文件)
    - 27.2.2 [脱机存储备份文件](#脱机存储备份文件)


#### 27.1 [监视系统统计信息](id:监视系统统计信息)

##### 27.1.1 [监视磁盘空闲空间](id:监视磁盘空闲空间)

shell 脚本将监视特定卷上的可用磁盘空间, 并在可用磁盘空间低于设置的阈值时发送一则电子邮件消息。

1. **必需函数**

    要自动监视可用的磁盘空间, 首先需要使用可显示这些值的命令。最佳的监视磁盘空间命令是 `df` 命令。

        $ df
        Filesystem    512-blocks     Used Available Capacity  iused    ifree %iused  Mounted on
        /dev/disk0s3   878182944 88152096 789518848    11% 11083010 98689856   10%   /
        devfs                362      362         0   100%      628        0  100%   /dev
        map -hosts             0        0         0   100%        0        0  100%   /net
        map auto_home          0        0         0   100%        0        0  100%   /home
        /dev/disk0s2    97656256 88092344   9563912    91% 11011541  1195489   90%   /Volumes/Recovery HD
        
    `df` 命令显示系统上所有真实磁盘和虚拟磁盘的当前磁盘空间的统计信息。这里我们将监视根文件系统的大小。我们首先需要解析 `df` 命令的结果, 以便只提取要文件系统所在的行。
    
    最简单的我们可以使用 `sed` 命令搜索以正斜杠结尾的行。
    
        $ df | sed -n '/\/$/p'
        /dev/disk0s3   878182944 88156152 789514792    11% 11083517 98689349   10%   /
        
    下一步是分离出此行上的百分比值。我们可以使用 `gawk` 命令 : 
    
        $df | sed -n '/\/$/p' | gawk '{print $5}'
        11%
        
    再将百分号去掉, 可以使用 `sed` 命令 : 
    
        $df | sed -n '/\/$/p' | gawk '{print $5}' | sed 's/%//'                
        11
        
2. **创建脚本**

    下面是使用此技术的代码示例 : 
    
        #!/bin/bash
        
        SPACE=`df | sed -n '/\/$/p' | gawk '{print $5}' | sed 's/%//'`
        
        if [ $SPACE -ge 90 ]
        then 
            echo "Disk space on root at $SPACE% used" | mail -s "Disk warning" root
        fi
        
3. **运行脚本**

    现在可以设置 shell 脚本 的执行次数以监视磁盘活动。可以通过时间表完成此操作。
    
    运行此脚本的频率取决于文件服务器的活跃程序。对于空间较小的文件服务器, 只需要一天运行一次 : 
    
        30 0 * * * /home/rich/diskmon.sh
        
    此 cron 表项目在每天上午 12:30 运行 shell 脚本。对于大型文件服务器环境, 可能需要一天监视几次 : 
    
        30 0,8,12,16 * * * /home/rich/diskmon.sh
        
    此 cron 表项目每天运行 4 次, 分别是凌晨 12:30、上午 8:30、下午 12:30 和 下午 4:30.


[返回目录](#toc)

##### 27.1.2 [谁在霸占磁盘资源](id:谁在霸占磁盘资源)

如果 Linux 服务器上有许多用户, 则经常需要解决的一个问题就是谁在使用所有的磁盘空间。这个老掉牙的问题有时比其他问题更难以弄清。

不幸的是, 虽然跟踪用户磁盘空间使用情况非常重要, 但却没有一个 Linux 命令可以提供此信息。因此, 需要编写 shell 脚本来将其他命令拼凑起来以提取需要查找的信息。

1. **必需函数**

    第一个需要使用的工具是 `du` 命令。使用 `-s` 选项可显示目录级别的概况。这在计算单个用户的磁盘使用情况时非常便利。对 `/home` 目录内容使用此命令可以总结出每个用户的 `$HOME` 目录 : 
    
        $ du -s /home/*
        41	/home/host+found
        30	/home/rich
        48677584	/home/xun
        
    **`Tips : `** 要获取所有用户 `$HOME` 目录的统计信息, 在运行脚本时必须作为根用户登录。
    
    可以查看 `$HOME` 目录总计的列表了 ( 以 KB 为单位 )。根据挂载 `/home` 目录的方式, 也许能看到一个称为 `lost+found` 的特殊目录, 这不是一个用户账户。
    
    去掉这个目录, 可以使用带 `-v` 选项的 `grep` 命令, 将打印除了包含指定文本行以外的所有行 : 
    
        $ du -s /home/* | grep -v lost
        
    接下来, 去掉所有的路径名, 以便查看用户名。这可以用 `sed` 命令 : 
    
        $ du -s /home/* | grep -v lost | sed 's/\/home\///'
        30	/home/rich
        48677584	/home/xun
        
    现在, 我们可以对此输出按降序排序 : 
    
        $ du -s /home/* | grep -v lost | sed 's/\/home\///' | sort -g -r
        48677584	/home/xun
        30	/home/rich
        
    `sort` 命令在使用 `-g` 选项时将所有数字值排序, 并在包括 `-r` 选项时按降序排序。
    
    我们感兴趣的是所有用户使用的总磁盘量。下面是此值获取的方法 : 
    
        $ du -s /home
        48679464	/home
        
2. **创建脚本**

    处理用于报告的数据的最简单方法是使用 `gawk` 命令。报告由 3 部分构成。
    
    - 头部 : 用文字标识出各个列。
    - 报告主体 : 显示用户、用户使用的总磁盘空间和用户消耗占总量的百分比。
    - 尾部 : 显示所有用户的总磁盘使用情况。
    
    下面是将所有元素综合在一起的磁盘使用情况的跟踪信息脚本 : 
    
        #!/bin/bash
        
        TEMP=`mktemp -t tmp.XXXXXX`
        du -s /home/* | grep -v lost | sed 's/\/home\///' | sort -g -r > $TEMP
        TOTAL=`du -s /home | gawk '{print $1}'`
        cat $TEMP | gawk -v n="$TOTAL" '
        BEGIN {
            print "Total Disk Usage by User";
            print "User\tSpace\tPercent"
        }
        
        {
            printf "%s\t%d\t%6.2f%\n", $2, $1, ($1/n)*100
        }
        
        END{
            print "-----------------";
            printf "Total\t%d\n", n
        }'
        rm -f $TEMP
        
3. **运行脚本**  

    都放在一起后, 运行脚本时应该得到一个很好的报告 : 
    
        $ ./diskhogs.sh
        Total DiskUsage By user
        User    Space    Percent
        xun     48677584 99.99%
        rich    30       0.00%
        ------------------------
        Total    48677614


[返回目录](#toc)

##### 27.1.3 [监视 CPU 和内存使用情况](id:监视 CPU 和内存使用情况)

任何 Linux 系统的核心统计信息应该是 CPU 和内存使用情况。如果这些开始失控, 那么系统将很快崩溃。本节将演示如何使用几个基本的 shell 脚本来编写监视和跟踪 Linux 系统上的 CPU 和内存使用情况的脚本。

1. **必需函数**

    第一步是决定生成的数据。有几个不同的命令都可以用于从系统提取 CPU 和内存使用信息。
    
    最基本的系统统计信息命令是 `uptime` 命令 :  
    
        $ uptime
        15:13  up  7:22, 2 users, load averages: 0.47 0.67 0.67
        
    `uptime` 命令给出了一些我们可以使用的不同的基本信息片段 :  
    
    - 当前时间;
    - 系统运行的天、小时和分钟数;
    - 当前已登录到系统的用户数;
    - 1、5 和 15 分钟的装载平均值。
    
    另一个提取系统信息的命令是 `vmstat` 命令。下面是 `vmstat` 命令输出的示例 :  
    
        $ vmstat 
        process- -------memory---------- --swap- --io--- ---system-- ---cpu-----
        r b      swpd   free   buff cache  si so   bi bo   in  cs  us sy id wa st
        0 0         0 427700  16608 37292  13  0    0 89   25  33   0  0 98  1  0
    
    第一次运行 `vmstat` 命令时, 将显示自上次重新引导的平均值。要获取当前统计信息, 必须使用命令行参数运行 `vmstat` 命令 : 
    
        $ vmstat
        process- -------memory---------- --swap- --io--- ---system-- ---cpu-----
        r b      swpd   free   buff cache  si so   bi bo   in  cs  us sy id wa st
        0 0         0 427700  16608 37292  13  0    0 89   25  33   0  0 98  1  0
        0 0         0 426400  16578 37272  13  0    0 89   25  33   0  0 92  5  0
        
    第二行包含 Linux 系统的当前统计信息。如您所见, `vmstat` 命令的输出有一些隐含的意义。
    
    **vmstat 输出符号**
    
    符号 | 描述
    ----|----
    `r` | 等待 CPU 时间的进程数
    `b` | 不间断休眠中的进程数
    `swpd` | 使用的虚拟内存量 ( 以 MB 为单位 )
    `free` | 未使用的物理内存量 ( 以 MB 为单位 )
    `buff` | 用作缓存空间的内存量 ( 以 MB 为单位 )
    `cache` | 用作高速缓存空间的内存量 ( 以 MB 为单位 )
    `si` | 从磁盘交换的内存量 ( 以 MB 为单位 )
    `so` | 交换到磁盘的内存量 ( 以 MB 为单位 )
    `bi` | 从块设备收到的块数
    `bo` | 发送到块设备的块数
    `in` | 每秒 CPU 的中断数
    `cs` | 每秒 CPU 的上下文交换数
    `us` | CPU 消耗在运行非内核代码上的时间百分比
    `sy` | CPU 消耗在运行内核代码上的时间百分比
    `id` | CPU 空间的时间百分比
    `wa` | CPU 消耗在等待 I/O 上的时间百分比
    `st` | 从虚拟机窃取的 CPU 时间百分比
    
    信息量很大, 通常只要记录各别值。通过空闲内存和空闲的 CPU 时间百分比就可以了解系统状态的大致情况。
    
    去掉 vmstat 命令的输出包含的表头信息 : 
    
        $ vmstat 1 2 | sed -n '/[0-9]/p'
        
    现在我们只需要第二行数据 : 
    
        $ vmstat 1 2 | sed -n '/[0-9]/p' | sed -n '2p'
        0 0         0 426400  16578 37272  13  0    0 89   25  33   0  0 92  5  0
        
    最后, 还需要用日期和时间戳来标记每个数据记录, 以便指示抓取快照时间。 `date` 命令格式化后的输出很方便 : 
    
        $ date +"%m/%d/%Y %k:%M:%S"
        08/31/2015 16:25:57
        
2. **创建捕获脚本**
    
    由于需要在固定时间间隔对系统数据进行抽样, 所以需要两个独立的脚本。一个脚本用于捕获数据并将其保存到日志文件中。此脚本应该按时间表有规律地运行。其运行频率取决于 Linux 系统的繁忙程度。对于大多数系统, 每个小时运行一次即可。
    
    第二个脚本应该输出报告数据并以电子邮件方式发送给相应的接收人。通常不希望每次获取新数据样本时都发送新的电子邮件报告, 而是希望第二个脚本以较低的频率按 cron 作业运行, 如每天一次, 或者一天中的第一个任务。
    
    用于捕获数据的第一个脚本称为 `capstats`, 脚本如下所示 : 
    
        #!/bin/bash
        
        OuTFILE=/home/rich/capstats.csv
        DATE=`date +%m/%d/%Y`
        TIME=`date +%k:%M:%S`
        
        TIMEOUT=`uptime`
        VMOUT=`vmstat 1 2`
        
        USERS=`echo $TIMEOUT | gawk '{print $4}'`
        LOAD=`echo $TIMEOUT | gawk '{print $9}' | sed`s/,//``
        FREE=`echo $VMOUT | sed -n '/[0-9]/p' | sed -n '2p' | gawk '{print $4}'`
        IDLE=`echo $VMOUT | sed -n '/[0-9]/p' | sed -n '2p' | gawk '{print $15}'`
        
        echo "$DATE, $TIME, $USERS, $LOAD, $FREE, $IDLE" >> $OUTFILE
        
    现在可以将脚本按 cron 作业运行了, 要使它每小时运行一次, 可以创建此 cron 表项 :  
    
        0 * * * * /home/rich/capstats.sh
        
3. **生成报告脚本**  

    `gawk` 命令允许从文件中提取原数据并以任何形式显示。首先, 从命令行进行测试, 使用 capstats.sh 脚本创建的新 capstats.csv 文件 : 
    
        $ cat capstats.csv | gawk -F, '{printf "%s %s - %s\n", $1, $2, $4}'
        
    报告我们还可以转为 html 格式发送给管理人员 : 
    
        #!/bin/bash
        
        FILE=/home/rich/capstats.csv
        FILE=/home/rich/capstats.html
        MAIL=`which mutt`
        DATE=`date +"%A, %B %d, %Y"`
        
        echo "<html><body><h2>Report for $DATE</h2>" > $TEMP
        echo "<table border=\"1\">" >> $TEMP
        echo "<tr><>td>Date</td><td>Time</td><td>Users</td>" >> $TEMP
        echo "<td>Load</td><td>Free Memory</td><td>%CPU Idle</td></tr>" >> $TEMP
        
        cat $FILE | gawk -F, '{
            printf "<tr><td>%s</td><td>%s</td><td>%s</td>", $1, $2, $3;
            printf "<td>%s</td><td>%s</td><td>%s</td>\n</tr>\n", $4, $5, $6;
        }' >> $TEMP
        echo "</table></body></html>" >> $TEMP
        $MAIL -a $TEMP -s "Stat report for $DATE" rich < /dev/null
        rm -f $TEMP
        
    `mutt` 命令将附加文件的名称用作附件文件名, 因此使用 `mktemp` 命令创建报告文件并不好。脚本最后将删除文件内容。
    
    **`Tips : `** 大多数电子邮件客户端可以通过文件扩展名自动探测附件的类型, 因此, 应该让报告文件以 .html 结尾。


[返回目录](#toc)

#### 27.2 [执行备份](id:执行备份)

##### 27.2.1 [归档数据文件](id:归档数据文件)

如果正在使用 Linux 系统处理非常重要的项目, 则可以创建 shell 脚本以自动获取正在使用的工作目录的快照 ,然后将其保存到系统上一个安全的位置。虽然不能防止灾难性的硬盘崩溃, 便至少能防止文件被破坏或意外删除。

1. **必需函数**

    在 Linux 环境中, 归档数据时通常使用 `tar` 命令。`tar` 命令用于将整个目录归档为单个文件。
    
        $ tar -cf archive.tar /home/rich/test
        tar: Removing leading '/' from member names
        
    `tar` 命令返回了一条警告消息, 它删除了路径名开始处的正斜杠, 将其转换为绝对路径以进行检索。这允许在文件系统的任何位置提取 `tar` 归档文件。如果希望从脚本中删除那条消息, 可以通过将 STRERR 重定向到 `/dev/null` 文件完成操作 : 
    
        $ tar -cf archive.tar /home/rich/test 2> /dev/null
        
    创建归档文件后, 可以将其发送给最喜爱的压缩工具来缩减其大小 : 
    
        $ gzip archive.tar
        
    下一步是创建滚动的归档系统。如果按固定频率保存工作目录, 则可能不希望最近的归档副本立即覆盖以前的归档副本。
    
    要防止此类情况发生, 需要创建一个方法来自动为每个归档副本提供唯一的文件名。最简单的办法是在文件名由使用日期和时间。
    
        $ DATE=`date +%y%m%d`
        $ FILE=tmp$DATE
        $ echo $FILE
        tmp080206
        
2. **创建日常归档脚本**
    
    `archdaily` 脚本自动在单独的位置创建工作目录的归档版本, 同时使用日期来唯一标识文件。
    
        #!/bin/bash
        
        DATE=`date +%y%m%d`
        FILE=archive$DATE
        SOURCE=/home/rich/test
        DESTINATION=/home/rich/archive/$FILE
        
        tar -cf $DESTINATION $SOURCE 2> /dev/null
        gzip $DESTINATION
        
    archdaily 脚本基于运行时的年、月、日生成唯一的文件名。它还使用环境变量定义源目录以进行归档, 所以可以轻松更改此源目录。甚至可以使用命令行为参数为 archdaily 程序添加更多功能。
    
3. **创建每小时的归档脚本**
   
    如果您处在大容量的生产环境中, 而文件变更非常迅速, 那么每天的归档操作可能远远不够。如果希望将归档频率增加到每小时, 则需要考虑一件事。
    
    如果按每小时归档文件, 且使用 `date` 命令在文件名中添加时间戳, 那么可能发现它们非常难看。与其将所有归档文件放在同一文件夹下, 不如为归档的文件单独创建一个目录层次结构。
    
    归档目录包含每年的每个月的目录, 将月份用作目录名。每个月的目录依次包含该月每天的目录。这样可以只对各个归档文件添加时间戳, 然后将其放在相应月份和日期下的目录中。
    
        #!/bin/bash
        
        DAY=`date +%d`
        MONTH=`date +%m`
        TIME=`date +%k%M`
        SOURCE=/home/rich/test
        BASEDEST=/home/rich/archive
        
        mkdir -p $BASEDEST/$MONTH/$DAY
        
        DESTINATION=$BASEDEST/$MONTH/$DAY/archive$TiME
        tar -cf $DESTINATION $SOURCE 2> /dev/null
        gzip $DESTINATION


[返回目录](#toc)

##### 27.2.2 [脱机存储备份文件](id:脱机存储备份文件)

虽然对重要文件进行归档是一个很好的想法, 但这无法取代保存在单独位置的完整备份。在大型的商务 Linux 环境中, 系统管理员通常可以奢侈地使用磁带机将重要文件复制到服务器以外的位置进行安全储存。

对于小型环境, 这并非可行之举。无法负担一种新奇的备份系统方法并不意味着您 的重要数据就要承担风险。可以将 shell 脚本组合起来, 即自动创建归档文件, 然后发送到 Linux 系统以外的位置。
    
1. **必需函数**

    下面要将压缩归档文件放到 Linux 系统以外的位置。如果已经配置了电子邮件系统, 则可以将其作为通往外界的门户。
    
    虽然系统管理员通常不将电子邮件用作归档数据的方式, 但这确实是个归档数据保存在另一个安全位置的好方法。
    
2. **创建脚本**

        #!/bin/bash
        
        MAIL=`which mutt`
        DATE=`date +%y%m%d`
        FILE=archive$DDATE
        SOURCE=/home/rich/test
        DESTINATION=/home/rich/archive/$FILE
        ZIPFILE=$DESTINATION.zip
        
        tar -cf $DESTINATION $SOURCE 2> /dev/null
        zip $ZIPFILE $DESTINATION
        $MAIL -a $ZIPFILE -s "Archive for $DATE" rich@myhost.com < /dev/null
        
    在脚本中, 我使用了 `zip` 压缩工具, 而非 `gzip` 命令。这可以创建 `.zip` 文件, 这样在非 Linux 系统上也可以轻松处理这些文件。

[返回目录](#toc)
