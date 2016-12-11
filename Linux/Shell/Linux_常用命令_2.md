### Linux_常用命令_2

**[命令目录](id:toc)**

命令 | 介绍
----|---
[basename](#basename) | 返回程序名称
[mktemp](#mktemp) | 创建临时文件
[tee](#tee) | 记录消息(可以输出监视器的同时保存至文件)
[sleep](#sleep) | 延迟指定时间
[date](#date) | 时间函数


#### [basename](id:basename) : 返回程序名称

`basename` 命令只返回程序名称, 不带路径。如下 : 

    $ basename /home/rich/testfile
    testfile

[返回目录](#toc)

#### [mktemp](id:mktemp) : 创建临时文件 

Linux 系统保留了一个特殊的目录位置, 以供临时文件使用。 Linux 使用 `/tmp` 目录处理不需要永久保存的文件。大部分 Linux 发行版的系统配置都是在启动时自动删除 `/tmp` 目录中的任何文件。

系统上的任何用户账户都有权读取和写入 `/tmp` 目录中的文件。该特性可以帮助您轻松创建临时文件, 而无需担心它们的清理问题。

创建临时文件还有一个特殊的命令。`mktemp` 命令可以轻松在 `/tmp` 文件夹中创建一个唯一的临时文件。 shell 创建该文件但是不使用默认的 `umask` 值。相反, 它公向文件所有者分配读取和写入权限, 并使您成为文件的所有者。 创建文件之后, 就可以通过脚本进行完整权限的读取和定入操作, 但其他人都不能访问它 (根用户除外)。

**参数分析**

参数 | 解释
----|---
`-t` | 强迫 mktemp 在系统临时文件夹中创建文件
`-d` | 让 mktemp 命令创建一个临时目录而不是一个文件



**创建本地临时文件**

默认情况下, `mktemp` 在当前目录创建文件。要使用 `mktemp` 命令在当前目录中创建临时文件, 只需要指定一个文件名模板即可。模板包括文件名以及附加到文件名后面的 6 个 X :  

    $ mktemp testing.XXXXXX
    $ ls -al testing*
    -rw-------  1 xun  staff     0B  8 10 16:26 test.GYIFt1
`mktemp` 命令使用一个 6 个字符代码替换 6 个 X, 以确保文件名在目录中的唯一性。每次创建 6 个 X 创建的字符代码都为随机的不同值。

在脚本中使用 `mktemp` 命令时, 需要使用一个变量保存该文件名, 以便稍后在脚本中引用 :   

    #!/bin/bash
    
    tempfile=`mktemp test.XXXXXX`
    exec 3>$tempfile    # 通过 文件描述符 3 写入 tempfile 文件

    echo "This script writes to temp file $tempfile"
    
    echo "This is the first line" >&3
    echo "This is the second line" >&3
    echo "This is the last line." >&3
    exec 3>&-            #关闭 文件描述符 3
    
    echo "Done creating temp file.The contents are:"
    cat $tempfile
    rm -f $tempfile 2> /dev/null    #删除 tempfile 文件，错误信息不输出
    
    $ ./testfile
    This script writes to temp file test.u2ivG1
    Done creating temp file.The contents are:
    This is the first line
    This is the second line
    This is the last line.

[返回目录](#toc)

**在 /temp 中创建临时文件**

`-t` 选项强迫 `mktemp` 在系统临时文件夹中创建文件。但使用该特性时, `mktemp` 命令返回用于创建临时文件的完整路径名, 而不仅仅是文件名 :   

    $ mktemp -t test.XXXXXX
    /temp/test.xG3374
    
由于 `mktemp` 命令返回完整路径名, 因此可以从 Linux 系统的任何目录引用临时文件, 无论系统将临时目录放置在哪个位置 :  

    #!/bin/bash
    
    tempfile=`mktemp -t temp.XXXXXX`
    echo "This is a test file." > $tempfile
    echo "This is the second line of the test." >> $tempfile
    
    echo "The temp file is located at: $tempfile"
    cat $tempfile
    rm -f $tempfile
    
    //我用的是苹果系统
    $ ./testfile 
    The temp file is located at: /var/folders/wn/z5wcd0415sn2n_4wz3gsk4zh0000gn/T/temp.XXXXXX.S11g9JI3    
    This is a test file.
    This is the second line of the test.


[返回目录](#toc)

**创建临时目录**

`-d` 选项让 `mktemp` 命令创建一个临时目录而不是一个文件。然后可以将该目录用于任何目的, 比如创建更多临时文件 :   

    #!/bin/bash
    
    tempdir=`mktemp -d dir.XXXXXX`
    cd $tempdir
    tempfile1=`mktemp temp.XXXXXX`
    tempfile2=`mktemp temp.XXXXXX`
    exec 7> $tempfile1
    exec 8> $tempfile2
    
    echo "Sending data to directory $tempdir"
    echo "This is a test line of data for $tempfile1"
    echo "This is a test line of data for $tempfile2"
    echo "This is a test line of data for $tempfile1" >&7
    echo "This is a test line of data for $tempfile2" >&8
    
    $ ./testfile
    Sending data to directory dir.czRW3k
    This is a test line of data for temp.ghMozH
    This is a test line of data for temp.ilsgUy
    
[返回目录](#toc)

#### [tee](id:tee) : 记录消息 

有时很有必要将输出同时发送到监视器和日志文件。这种情况下不需要使用两次重定向, 只需要使用特殊的 `tee` 命令即可。

`tee` 命令就像管道的 T 型接头。它将 `STDIN` 的数据同时发送到两个目的地。一个是 `STDOUT`, 另一个是`tee` 命令行指定的文件名 :   

    tee filename
由于 `tee` 重定向来自 `STDIN` 的数据, 因此可以与管道命令配置使用重定向任何命令的输出 :  

    $ date | tee testfile
    2015年 8月10日 星期一 17时30分19秒 CST
    
    $ cat testfile
    2015年 8月10日 星期一 17时30分19秒 CST
**`Tips : `** 默认情况下, `tee` 命令每次使用时都会覆盖输出文件 "testfile" 中的内容。

参数 `-a`, 可以向输出文件追加数据。

    $ who | tee -a testfile
    xun      console  Aug 10 08:58
    xun      ttys000  Aug 10 10:18
    
    $ cat testfile
    2015年 8月10日 星期一 17时30分19秒 CST
    xun      console  Aug 10 08:58
    xun      ttys000  Aug 10 10:18

[返回目录](#toc)

#### [sleep](id:sleep) : 延迟指定时间

在有的shell（比如linux中的bash）中sleep还支持睡眠（分，小时）  
>sleep 1   睡眠1秒  
sleep 1s   睡眠1秒  
sleep 1m   睡眠1分  
sleep 1h   睡眠1小时   

    $ date; sleep 1.5m; date
    2011年 04月 17日 星期日 19:50:06 CST
    2011年 04月 17日 星期日 19:51:36 CST


[返回目录](#toc)

#### [trap](id:trap) : 延迟指定时间

`trap` 命令只可以指定能够通过 shell 脚本监控和拦截的 Linux 信号。如果脚本收到在 `trap` 命令中列出的信号, 它将保护信号不被 shell 处理, 并在本地处理它。

`trap` 命令的格式如下 : 

    trap commands signals
    
以下是一个使用 `trap` 命令忽略 `SIGINT` 和 `SIGTERM` 信号的简单示例 :   


    #!/bin/bash
    
    trap "echo you want to kill me ?" SIGINT SIGTERM
    echo "This is a test program"
    count=1
    while [ $count -le 10 ]
    do
        echo "Loop #$count"
        sleep 3
        count=$[ $count + 1 ]
    done
    echo "This is the end of the test program"
    
    $ ./testfile
    This is a test program
    Loop #1
    you want to kill me ?    //当试图使用 ^C 时，会有提示文字输出，但未中断执行
    Loop #2

[返回目录](#toc)

#### [date](id:date) : 时间函数

`date` 可以用来对时间进行显示、设置、转换格式的操作。

1. 显示时间

    - date 显示时间
    - date +%s 显示时间戳  
    时间日期格式字符串列表
    
    日期内容 | 格式
    ----|---
    星期 | %a (如 : Sat)
    ----| %A (如 : Saturday)
    月 | %b (如 : Nov)
    ----| %B (如 : November)
    日 | %d (如 : 31)
    固定格式日期(mm/dd/yy) | %D (如 : 10/18/10)
    年 | %y (如 : 10)
    ----| %Y  (如 : 2010)
    小时 | %I 或 %H (如 : 08)
    分钟 | %M (如 : 33)
    秒 | %S (如 : 10)
    纳秒 | %N (如 : 695208515)
    UNIX纪元时(以秒为单位) | %s (如 : 1290039386)
    
2. 转换格式

        $ date --date "THu Nov 18 08:07:21 IST 2010" +%s
        1290047841
        
        $ date --date "Jan 20 2001" +%A
        Saturday
        
3. 设置日期和时间

    date -s "格式化的日期字符串"
    
        $ date -s "21 June 2009 11:01:22"
        
    如果我们检查一组指定花费的时间 : 

        #!/bin/bash
        start=$(date +%s)
        commands;
        statements;
        
        end=$(date +%s)
        difference=$((end - start))
        echo Time taken to execute commands is $difference seconds.

4. 显示两天前的时间

    date -v [-+]val(ymwdHMS) +format
    
        $ date +%Y%m%d
        20151027
        $ date -v-2d +%Y%m%d
        20151025
        
 
   
[返回目录](#toc)

#### [sleep](id:sleep) : 延迟指定时间

[返回目录](#toc)

#### [sleep](id:sleep) : 延迟指定时间


[返回目录](#toc)


