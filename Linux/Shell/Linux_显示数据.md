### Linux_显示数据

[目录](id:toc)

- 12.1 [了解输入和输出](#:了解输入和输出)
    - 12.1.1 [标准文件描述符](#:标准文件描述符)
    - 12.1.2 [重定向错误](#:重定向错误)
- 12.2 [在脚本中重定向输出](#:在脚本中重定向输出)
    - 12.2.1 [临时重定向](#:临时重定向)
    - 12.2.2 [永久重定向](#:永久重定向)
- 12.3 [在脚本中重定向输入](#:在脚本中重定向输入)
- 12.4 [创建自己的重定向](#:创建自己的重定向)
    - 12.4.1 [创建输出文件描述符](#:创建输出文件描述符)
    - 12.4.2 [重定向文件描述符](#:重定向文件描述符)
    - 12.4.3 [创建输入文件描述符](#:创建输入文件描述符)
    - 12.4.4 [创建读取/写入文件描述符](#:创建读取/写入文件描述符)
    - 12.4.5 [关闭文件描述符](#:关闭文件描述符)
- 12.5 [列出开放文件描述符](#:列出开放文件描述符)
- 12.6 [禁止命令输出](#:禁止命令输出)
- 12.7 [使用临时文件](#:使用临时文件)
- 12.8 [记录消息](#:记录消息)




#### 12.1 [了解输入和输出](id:了解输入和输出)

##### 12.1.1 [标准文件描述符](id:标准文件描述符)

Linux 系统将每个对象当作文件处理。这包括输入和输出过程。 Linux  **使用文件描述符** (file descriptor) 标识每个文件对象。文件描述符是一个非负整数, 可以唯一地标识会话中打开的文件。每个进程中最多可以有 9 个打开文件的描述符。bash shell 为特殊需要保留前 3 个文件描述符 (0、1 和 2) : 

**Linux 标准文件描述符**

文件描述符 | 缩写 | 描述
----|---|---
0 | STDIN | 标准输入
1 | STDOUT | 标准输出
2 | STDERR | 标准错误

这 3 个特殊文件描述符处理脚本的输入和输出。 shell 使用它们将 shell 中的默认输入和输出定向到相应位置( 默认位置通常是监视器 )。

1. **STDIN**    
`STDIN` 文件描述符引用 shell 的标准输入。对于终端接口, 标准输入是键盘。shell 通过 `STDIN` 文件描述符从键盘接受输入, 并在键入时处理每个字符。   

    使用输入生定向符号 (`<`) 时, Linux 将使用重定向引用的文件替换标准的输入文件描述符。它读取文件并获取数据, 就像是通过键盘键入的一样。    
    
    许多 bash 命令通过 `STDIN` 接收输入, 尤其在命令行没有指定文件时。以下是使用 `cat` 命令通过 `STDIN` 输入数据的示例 :  
    
        $ cat 
        one 
        one
        two 
        two
    在命令行输入 `cat` 命令时, 它将通过 `STDIN` 接收输入。输入每行命令时, `cat` 命令将回显该行。  
    
    但也可以使用 `STDIN` 重定向符号强制 `cat` 命令从 `STDIN` 以处的其他文件接收输入 : 
    
        $ cat < testfile
    现在 `cat` 命令使用包含在 `testfile` 文件中的行作为输入。可以使用该技术将数据输入到任何从 `STDIN` 接收数据的 shell 命令。

2. **STDOUT**  

    `STDOUT` 文件描述符引用 shell 的标准输出。在终端接口, 标准输出是终端监视器。 shell 的所有输出 ( 包括 shell 中运行的程序和脚本 ) 都定向到标准输出, 也就是监视器。
    
    许多 bash 命令默认将输出定向到 `STDOUT` 文件描述符。可以使用输出重定向更改默认设置 :  
    
        $ ls -l > testfile
    使用输出重定向符号, 所有通常定向到监视器的输出现在都重定向到 shell 指定的重定向文件。还可以使用 `>>` 符号向文件添加数据。
        
        $ who >> test2
    但是, 如果使用脚本的标准输出重定向, 您可能会遇到麻烦。脚本中可能发生的问题示例如下 :   
    
        $ ls -al badfile > test3
        ls : cannot access badfile : No such file or directory
        $ cat test3
        $
    当命令生成错误消息时, shell 不会将错误消息重定向到输出重定向文件。 shell 创建了输出重定向文件, 但是错误消息显示在监视器屏幕上。
    
    shell 将错误消息与正常输出分开处理。如果创建一个以后台模式运行的 shell 脚本, 则通常必须依赖发送到日志文件的输出消息。如果使用这项技术, 那么在生成错误消息时, 将无法在日志文件中显示。因此必须换一种方法。

3. **STDERR**   

    shell 使用特殊的 `STDERR` 文件描述符处理错误消息。 `STDERR` 文件描述符引用 shell 的标准错误输出。这是 shell 发送错误消息 ( shell 或 shell 中运行的程序和脚本所生成 ) 的目的地。
    
    默认情况下, `STDERR` 文件描述符与 `STDOUT` 文件描述符指向相同位置 ( 即使它们分配了不同的文件描述符值 ) 。 这意味着, 默认情况下, 所有错误消息都显示在监视器上。
    
    但是, 从示例中可以看到, 重定向 `STDOUT` 不会自动重定向 `STDERR` 。处理脚本时, 常常需要更改该行为, 尤其是需要在日志文件中记录错误消息时。

[返回目录](#toc)

##### 12.1.2 [重定向错误](id:重定向错误)

1. **仅重定向错误**    

    `STDERR` 文件描述符的值设置为 2。通过替换位于重定向符号正前方的这个文件描述符, 可以仅重定向错误消息。该值必须位于重定向符号的正前方, 否则将无效 :  
    
        $ ls -al badfile 2> testfile
        
        $ cat testfile
        ls : cannot access badfile : No such file or directory
    这种方法, shell 仅重定向错误消息, 不包括普通数据。这里还有一个示例, 该示例在同一输出中混合了 `STDOUT` 和 `STDERR` 消息 :   

        $ ls -al badtest test2 2> testfile
        -rw-rw-r-- 1 rich rich 158 2007-10-26 11:32 test2
        
        $ cat testfile
        ls : cannot access badtest : No such file or directory
        
2. **重定向错误和数据**   

    如果需要同时重定向错误和普通数据, 则必须使用两个重定向符号。必须在希望重定向的数据前面放置相应的文件描述符, 然后将它们指向相应的输出文件以保存数据 :   
    
        $ ls -al test badtest 2> test3 1> test5
        
        $ cat test3
        ls : cannot access badtest : No such file or directory
        
        $ cat test5
        -rw-rw-r-- 1 rich rich 158 2007-10-26 11:32 test
    通过 `1>` 符号, shell 将定向到 `STDOUT` 的 `ls` 命令的普通输出重定向 test5 文件。通过 `2>` 符号, 将应该定向到 `STDERR` 的错误消息重定向到 test3 文件。

    此处, 如果愿意, 也可以将 `STDERR` 和 `STDOUT` 输出重定向到同一个输出文件。为此, bash shell 提供了一个特殊的重定向符号, 即 `&>` 符号 :   
    
        $ ls -al badtest test badtest2 &> test7
        
        $ cat test7
        ls : cannot access badtest : No such file or directory
        ls : cannot access badtest2 : No such file or directory
        -rw-rw-r-- 1 rich rich 158 2007-10-26 11:32 test
    使用 `&>` 符号, 命令生成的所有输出都发送到同一位置, 包括数据和错误。您会发现其中一个错误的消息的顺序与期望的不同。badtest2 文件 ( 列出的最后一个文件 ) 的错误消息排在输出文件的第二位。bash shell 自动使错误消息的优先级高于标准输出。这使您能够一起查看错误消息, 而不用在整个输出文件中查找。


[返回目录](#toc)


#### 12.2 [在脚本中重定向输出](id:在脚本中重定向输出)
##### 12.2.1 [临时重定向](id:临时重定向)

如果想故意在脚本中生成错误消息, 可以将单个输出行重定向到 `STDERR`。需要做的只是输出重定向符号将输出重定向到 `STDERR` 文件描述符。重定向到某个文件描述符时, 必须在文件描述符编号前面添加 `&` 号。

    echo "This is an error message" >&2
该行在脚本的 `STDERR` 文件描述符指向的地方而不是普通的 `STDOUR` 上显示以上文本。以下是使用这项特性的脚本示例 :  

    #!/bin/bash
    
    echo "This is an error" >&2
    echo "This is normal output"

    $ ./testfile
    This is an error
    This is normal output
    
    $ ./testfile 2> testfile
    This is normal output
    $ cat test9    
    This is an error
**`Tips : `**默认情况下, Linux 将 `STDOUT` 输出定向到 `STDOUT`。但是, 如果运行脚本时重定向 `STDERR`, 任何使用脚本定向到 `STDERR` 的文本将被重定向。


[返回目录](#toc)

##### 12.2.2 [永久重定向](id:永久重定向)

使用 `exec` 命令通知 shell 在脚本执行期间重定向特定的文件描述符 :   
    
    #!/bin/bash
    exec 1>testout
    
    echo "This is a test of redirecting all output"
    
    $ ./testfile
    $ cat testout
    This is a test of redirecting all output
`exec` 命令启动一个新 shell, 并将 `STDOUT` 文件描述符重定向到一个文件。所有定向到 `STDOUT` 的脚本输出都将重定向到这个文件。

    #!/bin/bash
    exec 2>testerror
    
    echo "This is the start of the script"
    
    exec 1>testout
    
    echo "This output should goto the testout file"
    echo "but this should go to the testerror file" >&2
    
    $ ./testfile
    This is the start of the script
    
    $ cat testout
    This output should go to the testout file
    
    $ cat testerror 
    but this should go to the testerror file
    
脚本使用 `exec` 命令将任何定向到 `STDERR` 的输出重定向到文件 testerror。接下来, 脚本使用 `echo` 重定向 `STDOUT` 显示几行。之后, 再次使用 `exec` 命令将 `STDOUT` 重定向到 testout 文件。注意, 即使重定向了 `STDOUT` , 也可以指定 `echo` 语句的输出定向到 `STDERR` , 在本例中 `STDERR` 仍然重定向到 testerror 文件。

[返回目录](#toc)

#### 12.3 [在脚本中重定向输入](id:在脚本中重定向输入)

重定向 `STDOUT` 和 `STDERR` 的技术同样可以用来重定向键盘的 `STDIN` 。`exec` 命令可以帮助您将 `STDIN` 重定向到 Linux 系统的文件 :  

    exec 0< testfile
该命令告诉 shell 它应该从文件 testfile 而不是 `STDIN` 获取输入。在脚本请求输出的任何时间都可以使用这种重定向方式。

    #!/bin/bash
    exec 0< testin
    count=1
    
    while read line
    do
        echo "Line #$count: $line"
        count=$[ $count + 1 ]
    done
    
    $ ./testfile
    Line #1: this is the first line.
    Line #2: this is the second line.
    Line #3: this is the third line.
这项技术非常好, 可以读取文件的数据并使用脚本处理。 Linux 系统管理员常见的一个任务是, 从日志文件读取数据并进行处理。这是执行该任务最简单的方式。


[返回目录](#toc)

#### 12.4 [创建自己的重定向](id:创建自己的重定向)

在脚本中重定向输入和输出时, 并不局限于 3 种默认的文件描述符。如前所述, 在 shell 中最多可以有 9 个打开的文件描述符。其他 6 个文件描述符的编号22  3～8。 可以将这些文件描述符应用到任何文件, 然后在脚本中使用它们。

##### 12.4.1 [创建输出文件描述符](id:创建输出文件描述符)

使用 `exec` 命令为输出分配文件描述符。与标准的文件描述符一样, 向文件位置分配了备选文件描述符之后, 该重定向将是永久性的, 除非重新分配。以下是一个在脚本中使用备选文件描述符的简单示例 :  

    #!/bin/bash
    
    exec 3>testout
    
    echo "This should display on the monitor"
    echo "and this should be stored in the file" >&3
    
    $ ./testfile
    This should display on the monitor
    
    $ cat testout
    and this should be stored in the file
    
脚本使用 `exec` 命令将文件描述符 3 重定向到一个备选文件位置。当脚本执行 `echo` 语句时, 它们将显示在 `STDOUT` 上。但是, 重定向到文件描述符 3 的 `echo` 语句定向到备选文件。这使您能够将普通输出定向到监视器, 将特殊信息重定向到文件 ( 如日志文件 )。

[返回目录](#toc)

##### 12.4.2 [重定向文件描述符](id:重定向文件描述符)

现在可以将 `STDOUT` 的原位置定向到备选文件描述符, 然后将该文件描述符重定向回 `STDOUT`。

    #!/bin/bash
    exec 3>&1         #指向 文件描述符 3 转为普通输出
    exec 1>testout    #普通输出定向到 testout 文件
    
    echo "This should store in the output file" >&3     #普通输出转到 文件描述符 3(普通输出)
    echo "along with this line."                        #普通输出 1> 重定向到文件 testout
    
    exec 1>&3         #普通输出转到 文件描述符 3 (普通输出)
    
    echo "Now things should be back to normal" 
    
    $ ./testfile
    This should store in the output file
    Now things should be back to normal
    
    $ cat testout
    along with this line.


[返回目录](#toc)

##### 12.4.3 [创建输入文件描述符](id:创建输入文件描述符)

可以使用重定向输出描述符的方式重定向输入文件描述符。在重定向到文件之前, 将 `STDIN` 文件描述符位置保存到另一个文件描述符, 然后在完成了文件读取操作之后, 可以将 `STDIN` 恢复为原来的位置 :   

    #!/bin/bash
    
    exec 6<&0        # 先将 STDIN 的位置保存到 文件描述符6中
    
    exec 0< testfile    # 将 STDIN 设为文件输入
    
    count=1
    while read line
    do
        echo "Line #$count: $line"
        count=$[ $count + 1 ]
    done
    exec 0<&6            # 将 STDIN 恢复为终端输入
    
    red -p "Are you done now " answer
    case $answer in 
    Y | y ) echo "Goodbye";;
    N | n ) echo "Sorry, this is the end";;
    esac


[返回目录](#toc)

##### 12.4.4 [创建读取/写入文件描述符](id:创建读取/写入文件描述符)

可以使用同一个文件描述符从一个文件读取数据, 同时向这个文件写入数据。

读取文件和向该文件写入数据时, shell 维护一个内部指针, 指示文件内部的位置。读取和写入操作都发生在指针上次放置的地方。如果不小心, 很可能造成非常有趣的结果 :  

    #!/bin/bash
    
    exec 3<> test
    read line <&3 
    echo "Read: $line"
    echo "This is a test line" >&3
    
    $ cat test
    This is the first line.
    This is the second line.
    This is the third line.
    
    $ ./testfile
    Read: This is the first line.
    
    $ cat test
    This is the first line.
    This is a test line
    ine.
    This is the third line.
脚本刚开始执行时, 输出显示读取了 test 文件的第一行。但是, 读取第一行后, 脚本指针应该指向了第二行开始位置。随后写入文件内容, 可以看到内容将原来数据覆盖。此处写入文件内容结尾位置有换行符, 所以这里才会将剩下内容换到下一行。


[返回目录](#toc)

##### 12.4.5 [关闭文件描述符](id:关闭文件描述符)

如果创建新的输入或输出文件描述符, shell 将在脚本退出时自动关闭它们。但有时需要在脚本结束前手动关闭文件描述符。

要关闭文件描述符, 将它重定向到特殊符号 `&-`。以下是它的示例 :   

    exec 3>&-
    该语句关闭文件描述符 3, 使之不再在脚本中使用。

当试图使用关闭的文件描述符时, 会出现错误提示 :  

    #!/bin/bash
    
    exec 3> test
    echo "This is a test line of data" >&3
    exec 3>&-
    echo "This won't work " >&3
    
    $ ./testfile
    ./testfile : 3: Bad file descriptor
    
如果稍后在脚本中打开同一个输出文件, shell 将使用新文件替换现有文件。这意味着如果您输出任何数据, 它将覆盖现有文件。

    #!/bin/bash
    
    exec 3> test
    echo "This is a test line of data" >&3
    exec 3>&-
    
    cat test
    
    exec 3> test
    echo "This'll be bad " >&3
    
    $ ./testfile
    This is a test line of data
    
    $ cat test
    This'll be bad     

[返回目录](#toc)

#### 12.5 [列出开放文件描述符](id:列出开放文件描述符)

因为只有 9 个文件描述符可用, 您可能认为让所有内容条理清晰是件很容易的事情。但有时很难跟踪哪个文件描述符重定向到了哪个位置。为了帮助您清理线索, bash shell 提供了 `lsof` 命令。

`lsof` 命令列出整个 Linux 系统上所有的开放文件描述符。某种程度上说, 这是一项有争议的功能, 因为它向非系统管理员提供有关 Linux 系统的信息。因此, 许多 Linux 系统都隐藏了该命令, 以防用户无意中使用它。

要使用普通用户账户运行它, 必须使用完全路径名引用它 :   


    $ /usr/sbin/lsof
这会生成大量输出。 它显示 Linux 系统上当前开放的每个文件的相关信息, 包括所有后台运行的进程, 以及所有登录到系统的用户账户。

参数 `-p`, 该参数可以指定进程 ID (PID), 还有 `-d`, 该参数可以指定显示的文件描述符编号。`-a` 选项, 可以用来连接其他两个选项的结果。   
为了轻松确定进程的当前 PID, 可以使用特殊环境变量 `$$`, shell 将该变量设置为当前 PID。以生成内容 :   

    $ /usr/sbin/lsof -a -p $$ -d 0,1,2
    COMMAND PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
    zsh     486  xun    0u   CHR   16,0  0t47138  683 /dev/ttys000
    zsh     486  xun    1u   CHR   16,0  0t47138  683 /dev/ttys000
    zsh     486  xun    2u   CHR   16,0  0t47138  683 /dev/ttys000

**默认 lsof 输出**

列 | 描述
----|---
COMMAND | 进程中命令名称的前 9 个字符
PID | 进程的进程ID
USER | 拥有进程的用户的登录名
FD | 文件描述符号和访问类型 (r-读取, w-写入, u-读取/写入)
TYPE | 文件类型( CHR-字符, BLK-块, DIR-目录, REG-常规文件 )
DEVICE | 设备的设备编号 (主要和次要)
SIZE | 如果可用, 则为文件大小 
NODE | 本地文件的节点编号
NAME | 文件名

与 `STDIN`、`STDOUT` 和 `STDERR` 关联的文件类型是字符模式。由于 `STDIN`、`STDOUT` 和 `STDERR` 文件描述符都指向终端, 则输出文件的名称是终端的设备名。所有 3 个标准文件都可以用于读取和写入。

从打开几个备选文件描述符的脚本内部查看 `lsof` 命令的结果 :  

    #!/bin/bash
    
    exec 3> test1
    exec 6> test2
    exec 7< test
    
    /usr/sbin/lsof -a -p $$ -d0,1,2,3,6,7
    
    $ ./testfile
    COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF    NODE NAME
    bash    1182  xun    0u   CHR   16,0 0t124531     683 /dev/ttys000
    bash    1182  xun    1u   CHR   16,0 0t124531     683 /dev/ttys000
    bash    1182  xun    2u   CHR   16,0 0t124531     683 /dev/ttys000
    bash    1182  xun    3w   REG    1,2        0 2630593 /Users/xun/Documents/language_bash/test1
    bash    1182  xun    6w   REG    1,2        0 2630594 /Users/xun/Documents/language_bash/test2
    bash    1182  xun    7r   REG    1,2       40 2622851 /Users/xun/Documents/language_bash/test
    
该脚本创建了 3 个备选文件描述符, 两个用于输出 (3 和 6), 一个用于输入 (7)。 当脚本运行 `lsof` 命令时, 可以看到输出中的新文件描述符。它显示每个文件的类型都是 REG, 表示它们是文件系统的常规文件。

[返回目录](#toc)

#### 12.6 [禁止命令输出](id:禁止命令输出)

有时候不希望显示任何脚本输出, 要解决该问题, 可以将 `STDERR` 重定向到称为 **空文件** (null file) 的特殊文件。顾名思义, 该文件不包含任何内容。shell 输出到空文件的任何数据都不会保存, 即全部丢失。

该文件在 Linux 系统的标准位置是 `/dev/null`。任何重定向到该位置的数据都将丢失且不会显示 :    

    $ ls -al > /dev/null
    $ cat /dev/null
    
常见用法 :    

禁止错误消息而不保存它们的方法 :   

    $ ls -al badfile test 2> /dev/null    //将错误信息删除
    -rw-r--r--  1 xun  staff  40  8 10 14:46 test

快速将文件数据移除 :  

    $ /dev/null >testfile    //将空内容写入 testfile
    $ cat ./testfile
    $
这是一种常见的清除日志文件 (该文件必须保留在原地以供应用程序运行) 的方法。


[返回目录](#toc)

#### 12.7 [使用临时文件](id:使用临时文件)

Linux 系统保留了一个特殊的目录位置, 以供临时文件使用。 Linux 使用 `/tmp` 目录处理不需要永久保存的文件。大部分 Linux 发行版的系统配置都是在启动时自动删除 `/tmp` 目录中的任何文件。

系统上的任何用户账户都有权读取和写入 `/tmp` 目录中的文件。该特性可以帮助您轻松创建临时文件, 而无需担心它们的清理问题。

创建临时文件还有一个特殊的命令。`mktemp` 命令可以轻松在 `/tmp` 文件夹中创建一个唯一的临时文件。 shell 创建该文件但是不使用默认的 `umask` 值。相反, 它公向文件所有者分配读取和写入权限, 并使您成为文件的所有者。 创建文件之后, 就可以通过脚本进行完整权限的读取和定入操作, 但其他人都不能访问它 (根用户除外)。

##### 12.7.1 创建本地临时文件

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

##### 12.7.2 在 /temp 中创建临时文件

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

##### 12.7.3 创建临时目录

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

#### 12.8 [记录消息](id:记录消息)

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


































