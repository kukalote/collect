### Linux_处理用户输入

### 11 [处理用户输入](id:toc)

- 11.1 [命令行参数](#命令行参数)
    - 11.1.1 [读取参数](#读取参数)
    - 11.1.2 [读取程序名称](#读取程序名称)
    - 11.1.3 [测试参数](#测试参数)
- 11.2 [特殊的参数变量](#特殊的参数变量)
    - 11.2.1 [参数计数](#参数计数)
    - 11.2.2 [获取所有数据](#获取所有数据)
- 11.3 [移位](#移位)
- 11.4 [处理选项](#处理选项)
    - 11.4.1 [找出选项](#找出选项)
    - 11.4.2 [使用 getopt 命令](#使用 getopt 命令)
    - 11.4.3 [更高级的 getopts 命令](#更高级的 getopts 命令)
- 11.5 [标准化选项](#标准化选项)
- 11.6 [获取用户输入](#获取用户输入)
    - 11.6.1 [基本读取](#基本读取)
    - 11.6.2 [计时](#计时)
    - 11.6.3 [默读](#默读)
    - 11.6.4 [读取文件](#读取文件)


#### 11.1 [命令行参数](id:命令行参数)

向 shell 脚本传递数据的最基本方式是使用 **命令行参数** (command line parameters)。使用命令行参数可以在执行脚本时向命令行中添加数据值。

    $ ./addem 10 30
    
##### 11.1.1 [读取参数](id:读取参数)

bash shell 将在命令行中输入的所有参数赋值给一些特殊变量, 这些变量称为 **位置参数 (positional parameter)**。其中还包括 shell 执行的程序名称。位置参数通过标准数字表示, 其中 `$0` 表示程序名称, `$1` 为第一个参数, `$2` 为第二个参数, 依此类推, 直到 `$9` 为第九个参数。

    #!/bin/bash
    
    factorial=1
    for (( number = 1; number <= $1 ; number++ ))
    do
        factorial=$[ $factorial * $number ]
    done
    echo The factorial of $1 is $factorial
    
    $ ./testfile 5
    The factorial of 5 is 120

每个参数都是通过空格分隔的, 所以 shell 会将空格解析为两个值的分隔标志。要想在参数值中包含空格, 就必须使用引号 (单引号和双引号均可) :   

    #!/bin/bash
    
    echo Hello $1, glad to meet you 
    
    $ ./testfile 'Rich Blum'
    
**`Tips : `** 引号不作为数据的一部分, 它们只是界定数据的起始和结束。

如果脚本需要的命令参数多于 9 个, 那么可以继续在命令行中添加 命令行参数, 但变量名称会稍有变化。在第 9 个变量之后, 必须使用大rtkguqf变量括起来, 如 `${10}`。 示例如下 :   

    #!/bin/bash
    
    total=$[ ${10} * ${11} ]
    echo The tenth parameter is ${10}
    echo The eleventh parameter is ${11}
    
    $ ./testfile 1 2 3 4 5 6 7 8 9 10 11 
    The tenth parameter is 10
    The eleventh parameter is 11
    
[返回目录](#toc)


#####  11.1.2 [读取程序名称](id:读取程序名称)

传递给变量 `$0` 的字符串实际上是程序的完整路径, 而不仅仅是程序的名称。

`basename` 命令只返回程序名称, 不带路径。如下 : 

    #!/bin/bash
    
    name=`basename $0`
    echo The command entered is : $name
    
    $ /home/rich/testfile
    The command entered is testfile
    
下面我们可以用 `$0` 来处理不同逻辑了。
    
[返回目录](#toc)


##### 11.1.3 [测试参数](id:测试参数)

当脚本认为参数变量中包含数据, 但实际上其中并没有数据时, 极有可能会生成脚本错误消息。好的方法是对参数进行检查来保证使用参数前确实存在数据 :    

    #!/bin/bash
    
    if [ -n "$1" ]
    then 
        echo Hello $1, glad to meet you.
    else
        echo "Sorry, you didn't identify yourself."
    fi

[返回目录](#toc)

#### 11.2 [特殊的参数变量](id:特殊的参数变量)

##### 11.2.1 [参数计数](id:参数计数)

特殊变量 `$#` 中存储执行脚本时包含的命令行参数的个数。在脚本中的任意位置都可以使用这个特殊变量, 就像普通变量一样。

    #!/bin/bash
    echo There were $# parameters supplied.
    
    $ ./testfile 1 2 3 4 5 
    There were 5 parameters supplied 
**`Tips : `** 可能会认为因为 `$#` 变量包含参数个数值, 所以使用变量 `${$#}` 就可以得到最后一个命令行参数值。试一下, 看会得到什么结果。

    #!/bin/bash
    
    ehco The last parameter was ${$#}
    
    $ ./testfile
    The last parameter was 14354
显然产生了错误。结果表明不能在大括号中使用美元符号, 必须将美元符号替换为感叹号。
    
    #!/bin/bash
    
    params=$#
    echo The last parameter is $params
    echo The last parameter is ${!#}
    
    $ ./testfile 1 2 3 4 5 
    The last parameter is 5
    The last parameter is 5
    
[返回目录](#toc)


##### 11.2.2 [获取所有数据](id:获取所有数据)

有些情况下, 会需要获取命令行中提供的所有参数, 并对它们进行迭代。此时, 不必使用 `$#` 变量确定命令行中参数的个数, 然后循环所有参数, 而是使用其他一些特殊的变量。

变量 `$*` 和 `$@` 都是在一个变量中包含所有命令行参数。

变量 `$*` 将命令行中提供的所有参数作为一个单词处理。这个单词中包含出现在命令行中的每一个参数值。本质上, 变量 `$*` 不是将参数视为多个对象, 而是将它们看作一个参数。

而变量 `$@` 将命令行中提供的所有参数作为同一个字符串中的多个单词处理。允许对其中的值进行迭代, 分隔开所有提供的不同参数。通常使用 `for` 命令来进行这样的迭代。

下面的例子来展示它们之间的区别 :   

    #!/bin/bash
    
    count=1
    for param in "$*"
    do
        echo "\$* Parameter #$count = $param"
        count=$[ $count + 1 ]
    done
    
    count=1
    for param in "$@"
    do
        echo "\$@ Parameter #$count = $param"
        count=$[ $count + 1 ]
    done

    $ ./testfile rich barbara katie
    $* Parameter #1 = rich barbara katie
    $@ Parameter #1 = rich
    $@ Parameter #2 = barbara
    $@ Parameter #3 = katie

[返回目录](#toc)

#### 11.3 [移位](id:移位)

`shift` 命令能够改变命令行参数的相对位置。使用 `shift` 命令时, 默认将每个参数变量左移一个位置。于是变量 `$3` 的值移给变量 `$2`, 变量 `$2` 的值移给变量 `$1`, 而变量 `$1` 的值被丢弃 ( 注意变量 `$0` 的值和程序名称都保持不变 )。

操作示例如下 :   

    #!/bin/bash
    
    count=1
    while [ -n "$1" ]
    do
        echo "Parameter #$count = $1"
        shift
    done
    
    $ ./testfile rich barbara katie
    Parameter #1 = rich
    Parameter #2 = barbara
    Parameter #3 = katie
**`Tips : `** 使用 `shift` 命令时要谨慎。将某一参数移位后, 该参数值就丢失了, 不能恢复。

[返回目录](#toc)

#### 11.4 [处理选项](id:处理选项)

**选项** 是由破折号引导的单个字母, 它更改命令的行为。

##### 11.4.1 [找出选项](id:找出选项)

1. **处理简单选项**   
在抽取每个参数时, 使用 `case` 语句判断参数是否符合选项格式 :   

        #!/bin/bash
        
        while [ -n "$1" ]
        do
            case "$1" in
            -a) echo "Found the -a option";;
            -b) echo "Found the -b option";;
            -c) echo "Found the -c option";;
            *) echo "$1 is not a option";;
            esac
            shift
        done
        
        $ ./testfile -a -b -c -d
        Found the -a option
        Found the -b option
        Found the -c option
        -d is not a option

2. **从参数中分离选项**   
在 Linux 中的标准方式是通过特殊字符码将二者分开, 这个特殊字符码告诉脚本选项结束和普通参数开始的位置。    
对于 Linux, 这个特殊字符是双破折号 (`--`)。shel 使用双破折号指示选项列表的结束。发现双破折号后, 脚本就能够安全的将剩余的**命令行参数**作为**参数**而不是**选项处理**。   

        #!/bin/bash
        
        while [ -n "$1" ]
        do 
            case "$1" in
            -a) echo "Found the -a option";;
            -b) echo "Found the -b option";;
            -c) echo "Found the -c option";;
            --) shift
                break;;
            *) echo "$1 is not an option";;
            esac
            shift
        done
        
        count=1
        for param in $@
        do
            echo "Parameter #$count: $param"
            count=$[ $count + 1 ]
        done
        
        $ ./testfile -a -b c d 
        Found the -a option
        Found the -b option
        c is not an option
        d is not an option
        
        
        $ ./testfile -a -b -- c d 
        Found the -a option
        Found the -b option
        Parameter #1: c
        Parameter #2: d
3. **处理带值的选项**   
有些选项需要附加的参数值。这种情况下, 命令行形式与下面的格式类似 :   

        $ ./testfile -a test1 -b -c -d test2
要求脚本必须能够监测命令行选项何时需要附加参数, 并能够正确进行处理。下面是一个例子 :   

        #!/bin/bash
        
        while [ -n "$1" ]
        do
            case "$1" in 
            -a) echo "Found the -a option";;
            -b) param="$2"
                echo "Found the -b option, with parameter value $param"
                shift 2
                continue;;
            -c) echo "Found the -c option";;
            --) shift
                break;;
            *) echo "$1 is not an option";;
            esac
            shift
        done
        
        count=1
        for param in "$@"
        do
            echo "Parameter #$count: $param"
            count=$[ $count + 1 ]
        done
        
        $ ./testfile -a -b test1 -d 
        Found the -a option
        Found the -b option, with parameter value test1
        -d is not an option

**`Tips :`** 但是 仍有局限性。如下, 以前的方法就无法工作 :  

        $ ./testfile -ac

[返回目录](#toc)


##### 11.4.2 [使用 getopt 命令](id:使用 getopt 命令)

`getopt` 命令是个不错的工具, 它对命令行参数进行重新组织, 使其更便于在脚本中解析。

1. **命令格式**   
`getopt` 命令可以接受任意形式的命令行选项和参数列表, 并自动将这些选项和参数转换为适当的格式。命令格式如下 :  

        getopt options optstring parameters
**选项字符串** (optstring) 是处理的关键。它定义命令行中的有效选项字母。它还定义哪些选项字母需要参数值。  
首先, 在选项字符串中列出将在脚本中用到的每个命令行选项字母。然后在每个需要参数值的选项字母后面放置一个冒号。    

        $ getopt ab:cd -a -b test1 -cd test2 test3
        -a -b test1 -c -d -- test2 test3
其中的选项字符串定义了四个有效选项字母, a、b、c 和 d。还定义选项字母 b 需要一个参数值。当执行 `getopt` 命令时, 会监测提供的参数列表, 然后基于提供的选项字符串对列表进行解析。  
**`Tips : `** 解析时自动将 `-cd` 选项分割成两个不同的选项, 并插入双破折号来分隔行中的额外参数。  

2. **在脚本中使用 getopt**   
可以在脚本中使用 `getopt` 命令格式化为脚本输入的任意命令行选项或参数。这里有个技巧 :    
`set` 命令的一个选项是双破折号, 表示将命令行参数变量替换为 `set` 命令的命令行中的值。  
于是, 这个技巧便是 : 将原始脚本 命令行参数送给 `getopt` 命令, 然后将 `getopt` 命令的输出送给 `set` 命令, 以便将原始命令行参数替换为通过 `getopt` 格式化过的更精细的形式。如下所示 :    
    
        set -- `getopts ab:cd "$@"`   
现在原始的命令行参数变量就被替换成了 `getopt` 命令的输出, `getopt` 命令将命令行参数进行了格式化。

        #!/bin/bash
        
        set -- `getopt ab:c "$@"`
        echo "$*"
        
        while echo "$1"
        [ -n "$1" ]
        do
            case "$1" in 
            -a) echo "Found the -a option";;
            -b) param="$2"
                echo "Found the -b option, with parameter value $param"
                shift;;
            -c) echo "Found the -c option";;
            --) shift
                break;;
            *) echo "$1 is not an option";;
            esac
            shift
        done
        
        count=1
        for param in "$@"
        do
            echo "Parameter #$count: $param"
            count=$[ $count + 1 ]
        done
        
        $ ./testfile -ab test1 -c test2
        -a -b test1 -c -- test2
        -a
        Found the -a option
        -b
        Found the -b option, with parameter value test1
        -c
        Found the -c option
        --
        Parameter #1: test2
    
[返回目录](#toc)


##### 11.4.3 [更高级的 getopts 命令](id:更高级的 getopts 命令)

与 `getopt` 不同, `getopts` 命令顺序的对现有的 shell 参数变量进行处理。

每次调用一次 `getopts`, 它只处理在命令中监测到的参数中的一个。处理完所有参数后, 以大于零的退出状态退出。因此, `getopts` 非常适宜用在循环中解析所有命令行参数。

`getopts` 命令的格式为 : 
    
    getopts optstring variable

`getopts` 命令使用两个环境变量。环境变量 OPTARG 中包含需要参加数值的选项要使用的值。环境变量 OPTIND 包含的值表示 `getopts` 停止处理时在参数列表中的位置。这样, 处理完选项后可以继续处理其他命令行参数。

    #!/bin/bash
    
    while getopts :ab:c opt
    do
        case "$opt" in 
        a) echo "Found the -a option";;
        b) echo "Found the -b option, with value $OPTARG";;
        c) ehco "Found the -c option";;
        *) ehco "Unknown option: $opt";;
        esac
    done
    
    $ ./testfile -ab test1 -c
    Found the -a option
    Found the -b option, with value test1
    Found the -c option

`getopts` 命令具有很多很好的特性。首先, 现在可以在参数值中包含空格 :  

    $ ./testfile -b "test1 test2" -a
    Found the -b option, with value test1 test2
    Found the -a option
另一个好特性就是选项字母和参数值中间可以没有空格 :   

    $ ./testfile -abtest1
    Found the -a option
    Found the -b option, with value test1
`getopts` 命令还有一个好特性是将命令行中找到的未定义的选项都绑定为单一的输出---问号 :   

    $ ./testfile -d
    Unknown option: ?

`getopts` 命令知道何时停止处理选项, 将参数留给您处理。 `getopts` 每个处理选项, 环境变量 OPTIND 的值都会增加 1。当到达 getopts 处理的末尾时, 可以使用 shift 命令和 OPTIND 值进行操作来移动到参数 :   

    #!/bin/bash
    while getopts :ab:cd opt
    do
        case "$opt" in
        a) echo "Found the -a option";;
        b) echo "Found the -b option, with value $OPTARG";;
        c) echo "Found the -c option";;
        d) echo "Found the -d opiton";;
        *) echo "Unknown option: $opt";;
        esac
    done
    shift $[ $OPTIND - 1 ]
    
    count=1
    for param in "$@"
    do
        echo "Parameter $count: $param"
        count=$[ $count + 1 ]
    done
    
    $ ./testfile -a -b test1 -d test2 test3
    Found the -a option
    Found the -b option, with value test1
    Found the -d opiton
    Parameter 1: test2
    Parameter 2: test3

[返回目录](#toc)

#### 11.5 [标准化选项](id:标准化选项)

下表列出了一些命令行选项的常用含义。

**常用的 Linux 命令行选项**

选项 | 描述 
----|---
`-a` | 显示所有对象
`-c` | 生成计数
`-d` | 指定目录
`-e` | 展开对象
`-f` | 指定读取数据的文件
`-h` | 显示玲的帮助信息
`-i` | 忽略大小写
`-l` | 生成长格式的输出
`-n` | 使用非交互式 (批量)模式
`-o` | 指定一个输出文件来重向输出
`-q` | 以 quiet 模式执行
`-r` | 递归处理目录和文件
`-s` | 以 silent 模式执行
`-v` | 生成 verbose 输出
`-x` | 排除和拒绝
`-y` | 设置所有提问的回答为 yes

[返回目录](#toc)

#### 11.6 [获取用户输入](id:获取用户输入)

##### 11.6.1 [基本读取](id:基本读取)

`read` 命令的选项有 

选项 | 解释 | 实用
----|---|---
`-n` | 从输入读取n个字符存入变量 | `read -n 3 name`
`-s` | 用不回显（non-echoed）的方式读取密码 | `read -s var`
`-p` | 显示提示信息 | `read -p "Enter input:" var `
`-t` | 在指定的秒数内读取输入 | `read -t 2 var`
`-d` | 用定界符结束输入行 | `read -d ":" var` `hello:` var 置为 hello


`read` 命令接受标准输入 (键盘) 的输入, 或其他文件描述符的输入。得到输入后, `read` 命令将数据放入一个标准变量中。

    #!/bin/bash
    
    echo -n "Enter your name: "
    read name
    echo "Hello $name, welcome to my program."
    
    $ ./testfile
    Enter your name: Kitty Ben
    Hello Kitty Ben, welcome to my program.
**`Tips : `** 生成提示的 `echo ` 命令使用了 `-n` 选项。该选项抵制字符串结尾的新行符, 允许脚本用户在字符串后面立即输入数据, 而不是在下一行中输入。

`read` 命令的 `-p` 选项, 允许在 `read` 命令行中直接指定一个提示符 :  

    #!/bin/bash
    
    read -p "Please enter your age:" age
    days=$[ $age * 365 ]
    echo "That makes you over $days days old!"
    
    $ ./testfile
    Please enter your age: 10
    That makes you over 3650 days old
`read` 命令也可以指定多个变量 :  
    
    #!/bin/bash
    
    read -p "Enter your name: " first last
    echo "Checking data for $last, $first.."
    
    $ ./testfile
    Enter your name: Brown john
    Checking data for john, Brown…
    
在 `read` 命令行中也可以不指定变量。如果不指定变量, 那么 `read` 命令会将接收到的数据放置在环境变量 REPLY 中 :  

    #!/bin/bash
    
    read -p "Enter a number: "
    factorial=1
    for (( count=1; count <= $REPLY; count++ ))
    do
        factorial=$[ $factorial * $count ]
    done
    echo "The factorial of $REPLY is $factorial"
    
    $ ./testfile
    Enter a number : 5
    The factorial of 5 is 120
    
    
    


[返回目录](#toc)

##### 11.6.2 [计时](id:计时)

使用 `read` 命令存在着潜在危险。脚本很可能会停下来一直等待脚本用户输入数据。如果无论是否输入数据脚本都必须执行, 那么可以使用 `-t` 选项指定一个计时器。 `-t` 选项指定 `read` 命令等待输入的秒数。当计时器计时数满时, `read` 命令返回一个非零退出状态 : 

    #!/bin/bash
    
    if read -t 5 -p "Please enter your name: " name
    then 
        echo "Hello $name, wilcom to my script"
    else
        echo "Sorry, too sloe!"
    fi
    
除了输入时间计时, 还可以设置 `read` 命令计数输入的字符。当输入的字符数目达到预定数目时, 自动退出, 并将输入的数据赋值给变量。

    #!/bin/bash
    
    read -n1 -p "Do you want to continue [Y/N]? " answer
    case $answer in 
    Y | y) echo 
            echo "fine, continue on …";;
    N | n) echo 
            echo OK, goodbye
            exit;;
    esac
    echo "This is the end of the script"
这个例子使用了 `-n` 选项, 后接数值 1, 指示 `read` 命令只要接收到一个字符就退出。只要按下一个字符进行回答, `read` 命令立即接收并将其传给变量。无需按回车键。

[返回目录](#toc)
##### 11.6.3 [默读](id:默读)

有时候需要脚本用户进行输入, 但不希望输入的数据显示在监视器上。典型的例子就是输入密码, 当然还有很多其他的数据需要隐藏。

`-s` 选项能够使 `read` 命令中输入的数据不显示在监视器上(实际上, 数据是显示的, 只是read登记将文本颜色设置成 隔壁背景相同的颜色)。

    #!/bin/bash
    
    read -s -p "Enter your password  " pass
    echo 
    echo "iIsyour password really $pass?" 
    
    $ ./testfile
    Enter your password
    iIsyour password really boy?

[返回目录](#toc)
    
##### 11.6.4 [读取文件](id:读取文件)

可以使用 `read` 登记读取 Linux 系统上存储在文件中的数据。每调用一次 `read`  命令, 都会读取文件中的一行文本。当文件中没有可读的行时, `read` 命令将以非零退出状态退出。

    #!/bin/bash
    
    count=1
    cat test | while read line
    do
        echo "Line $count: $line"
        count $[ $count + 1 ]
    done
    echo "Finished processing the file"
    
[返回目录](#toc)


    





