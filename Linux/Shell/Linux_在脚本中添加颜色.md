### Linux_在脚本中添加颜色

### 15 [在脚本中添加颜色](id:toc)

- 15.1 [创建文本菜单](#创建文本菜单)
    - 15.1.1 [创建菜单布局](#创建菜单布局)
    - 15.1.2 [创建菜单函数](#创建菜单函数)
    - 15.1.3 [添加菜单逻辑](#添加菜单逻辑)
    - 15.1.4 [将其全部组合在一起](#将其全部组合在一起)
    - 15.1.5 [使用 select 命令](#使用 select 命令)
- 15.2 [添加颜色](#添加颜色)
    - 15.2.1 [ANSI 转义码](#ANSI 转义码)
    - 15.2.2 [显示 ANSI 转义码](#显示 ANSI 转义码)
    - 15.2.3 [在脚本中使用颜色](#在脚本中使用颜色)


#### 15.1 [创建文本菜单](id:创建文本菜单)

shell 脚本菜单的核心是 `case` 命令。`case` 命令根据用户在菜单中选择的字母执行特定的命令。

##### 15.1.1 [创建菜单布局](id:创建菜单布局)

在创建菜单之前, 一般要先清理监视器显示。这样可以在空白环境中显示菜单而没有令人分心的文本。

默认情况下, `echo` 命令仅显示打印文本字符。在创建新菜单项时, 通常使用不可打印项, 如制表符和换行符。要在 `echo` 命令中包含这些字符, 必须使用 `-e` 选项。

所以总结上面我们就可以写出下面这样的菜单 :  

    clear
    ehco 
    echo -e "\t\t\tSys Admin Menu\n"
    echo -e "\t1. Display disk space"
    echo -e "\t2. Display logged on users"
    echo -e "\t3. Display memory usage"
    echo -e "\t0. Exit menu\n\n"
    echo -en "\t\tEnter option:"
    
最后一行中的 `-en` 选项将显示该行, 而不在行尾添加换行符。

创建菜单后的最后一部分是获取客户的输入。最好在 `read` 命令中使用 `-n` 选项仅获取一个字符。这允许用户输入一个数字而不必按 `Enter` 键 : 

    read -n 1 option
    
[返回目录](#toc)

##### 15.1.2 [创建菜单函数](id:创建菜单函数)

shell 脚本菜单选项作为一组独立的函数创建更容易。这允许您创建一个容易理解的 `case` 命令。创建菜单 shell 脚本的第一步是确定脚本要执行的函数, 并将其作为代码中独立的函数来设计。

常见的做法是为未实现的函数创建 **桩函数** ( stub funciton )。**桩函数** 是尚未包含任何命令, 或者可能仅有一个 `echo` 语句说明此处最终应该放置的内容的函数 :  

    funciton diskspace {
        clear
        echo "This is where the disspace commands will go "
    }
    
为了方便建议将菜单布局本身作为函数来创建 :  

    function menu {
        clear
        ehco 
        echo -e "\t\t\tSys Admin Menu\n"
        echo -e "\t1. Display disk space"
        echo -e "\t2. Display logged on users"
        echo -e "\t3. Display memory usage"
        echo -e "\t0. Exit menu\n\n"
        echo -en "\t\tEnter option:"
        read -n 1 option
    }

这样只需要调用 `menu` 函数就可以很容易地随时再显示该菜单。


[返回目录](#toc)

##### 15.1.3 [添加菜单逻辑](id:添加菜单逻辑)

`case` 命令根据从菜单选择的字符调用合适的函数。使用默认的 `case` 命令字符 (星号) 捕获任意错误的菜单输入始终是一个好办法。

在典型菜单中使用 `case` 命令如下所示 :  

    menu
    case $option in 
    0)
        break ;;
    1)
        diskspace ;;
    2)
        whoseon ;;
    3) 
        memusage ;;
    *)
        clear
        echo "Sorry, wrong selection" ;;
    esac

[返回目录](#toc)

##### 15.1.4 [将其全部组合在一起](id:将其全部组合在一起)

现在把上面的东西收集起来，做一个完整的示例 :  

    #!/bin/bash
    
    function diskspace {
        clear
        df -k
    }
    function whoseon {
        clear
        who
    }
    function memusage {
        clear
        cat /proc/meminfo
    }
    function menu {
        clear
        ehco 
        echo -e "\t\t\tSys Admin Menu\n"
        echo -e "\t1. Display disk space"
        echo -e "\t2. Display logged on users"
        echo -e "\t3. Display memory usage"
        echo -e "\t0. Exit menu\n\n"
        echo -en "\t\tEnter option:"
        read -n 1 option
    }
    
    while [ 1 ]
    do
        menu
        case $option in 
        0)
            break ;;
        1)
            diskspace ;;
        2)
            whoseon ;;
        3)
            memusage ;;
        *)
            clear
            echo "Sorry, wrong selection" ;;
        esac
        echo -en "\n\n\n\t\t\t Hit any key to continue"
        read -n 1 option
    done
    clear



[返回目录](#toc)

##### 15.1.5 [使用 select 命令](id:使用 select 命令)

`select` 命令允许从单命令行创建菜单, 然后获取输入的答案并自动处理它。`select` 命令的格式是 :  

    select variable in list
    do
        commands
    done
    
列表参数是用空格隔开的构建菜单的文本项列表。 `select` 命令将列表中的每一项显示为一个编号选项, 然后为选择显示一个特殊的提示符 ( 由 PS3 环境变量定义 )。

    #!/bin/bash
    
    function diskspace {
        clear
        df -k
    }
    function whoseon {
        clear
        who
    }
    function memusage {
        clear
        cat /proc/meminfo
    }
    
    PS3="Enter option: "
    select option in "Display disk space" "Display logged on users" "Display memory usage" "Exit program"
    
    do
        case $option in
        "Exit program")
            break ;;
        "Display disk space")
            diskspace ;;
        "Display logged on users")
            whoseon ;;
        "Display memory usage")
            memusage ;;
        *)
            clear
            echo "Sorry, wrong selection" ;;
        esac
    done
    clear    

[返回目录](#toc)

#### 15.2 [添加颜色](id:添加颜色)

##### 15.2.1 [ANSI 转义码](id:ANSI 转义码)

大多数终端模拟软件能识别设置显示输出格式的 ANSI 转义码。ANSI 转义码以 **控制序列指示器** (control sequence indicator, CSI) 开头, 后面跟表示要在显示器上执行的操作的数据。CSI 告诉终端该数据代码一个转义码。

有些 ANSI 转义码可以用于将光标定位在显示器上的指定位置, 擦除部分显示, 以及控制显示器格式。要控制显示格式, 必须使用 **选择图形再现** ( Select Graphic Rendition, SGR ) 转义码。 SGR 转义码的格式为 :  

    CSIn[;k]m
    
该代码中的 `m` 代表 SGR 转义码。`n` 和 `k` 参数定义所使用的显示控制。可以仅指定一个参数或者同时指定两个, 中间用分号隔开。显示控制参数有 3 类 :  

- 效果控制代码;
- 前景色控制代码;
- 背影色控制代码。

**ANSI SGR 效果控制代码**

代码 | 描述
----|---
0 | 重置为普通模式
1 | 设置为强亮度
2 | 设置为弱亮度
3 | 使用斜体
4 | 使用单下划线
5 | 使用慢闪烁
6 | 使用快闪烁
7 | 背景、前景色反转
8 | 将前景色设置为背影色 ( 文字不可见 )

前景色控制码和背影色控制码都使用两位数代码。前景色使用 3 开头的一个两位数的值, 而背景色使用 4 开头的两位数的值。其中北二位数字表示具体颜色。

要将显示设置为使用斜体和闪烁, 可以发送代码 :  

    CSI3;5m

**ANSI 颜色控制代码**

代码 | 描述
----|---
0 | 黑色
1 | 红色
2 | 绿色
3 | 黄色
4 | 蓝色
5 | 洋红色
6 | 青色
7 | 白色

要指定白色前景色和红色背景, 发送代码 :  

    CSI37m;41m

[返回目录](#toc)

##### 15.2.2 [显示 ANSI 转义码](id:显示 ANSI 转义码)

CSI 字符通常是一个两字符序列。这个序列是 ESC ASCII 值, 后跟左方括号字符。有一个大多数编辑器都能识别的常用键序列允许在脚本中插入 ESC ASCII 值。这就是 `Ctrl-v` 组合键, 后跟 Esc 键。在输入此组合键时, 字符 `^[` 出现。

**`Tips : `** 在使用 ANSI 转义码的脚本中常常会看到 `^[` 字符。看到该组合时, 记住它是使用 `Ctrl-V Esc` 组合键生成的。

**`Tips : `** ANSI 颜色控制码会保持有效, 直到另一个 ANSI 颜色控制码改变输出。

要解决这一问题, 通常较好的方法是使用重置控制码 ( 0 ) 将终端重置为正常显示 :   

    $ echo ^[[41mThis is a test ^[[0m
    
**`Tips : `** 在一个 `echo` 命令中放置两个转义控制码时, 重要的是要用双引号将代码字符串引起来。没有双引号, `echo` 命令就不能正确解释转义码, 进而产生错误消息。

    $ echo "^[[33;44mThis is a test^[[0m"


[返回目录](#toc)

##### 15.2.3 [在脚本中使用颜色](id:在脚本中使用颜色)

使用 ANSI 转义控制码创建脚本时必须谨慎。记住, 无论何时当终端模拟器遇到控制码, 它都会处理。使用 `cat` 命令对含有 ANSI 转义控制码的脚本进行列表时, 这尤其危险。 `cat` 命令将在显示器上回显该代码, 然后终端模拟器解释这些代码, 并改变显示。在改变很多显示特性的长脚本中, 这是件很烦人的事情。

    #!/bin/bash
    
    function diskspace {
        clear
        df -k
    }
    function whoseon {
        clear
        who
    }
    function memusage {
        clear
        cat /proc/meminfo
    }
    function menu {
        clear
        ehco 
        echo -e "\t\t\tSys Admin Menu\n"
        echo -e "\t1. Display disk space"
        echo -e "\t2. Display logged on users"
        echo -e "\t3. Display memory usage"
        echo -e "^[[1m\t0. Exit menu\n\n^[[0m^[[44;33m"
        echo -en "\t\tEnter option:"
        read -n 1 option
    }
    
    echo "^[[44;33m"
    while [ 1 ]
    do
        menu
        case $option in 
        0)
            break ;;
        1)
            diskspace ;;
        2)
            whoseon ;;
        3)
            memusage ;;
        *)
            clear
            echo -e "^[[5m\t\t\tSorry, wrong selection^[[0m^[[44;33m" ;;
        esac
        echo -en "\n\n\n\t\t\t Hit any key to continue"
        read -n 1 option
    done
    echo "^[[0m"
    clear

**`Tips : `** 在使用 `clear` 命令之前, 重要的是要设置前景色和背景色; 否则 `clear` 命令将用默认的颜色给终端模拟屏幕着色, 从而使菜单项与背景的其他部分不协调。

[返回目录](#toc)





