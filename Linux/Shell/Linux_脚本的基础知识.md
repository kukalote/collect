### Linux 脚本的基础知识

>为什么要使用脚本 :   
命令行的最大字符数不超过 255 个字符。

创建 shell 脚本文件时, 必须在文件的第一行指明所使用的 shell。格式如下 :   

    #!/bin/bash

在普通的 shell 脚本中, 英镑符号 (#) 用作注释行。shell 不处理 shell 脚本中的注释行。但是, shell 脚本文件的第一行是个特例。用来指明shell。

[功能介绍](id:toc)

章节 | 介绍
----|---
[echo](#echo) |  显示消息
--------------------| [使用变量](#变量)
[环境变量](#环境变量) | 
[用户变量](#用户变量) | 
[反引号](#反引号) | 
--------------------| [重定向输入输出](#输入输出)
[输出重定向](#输出重定向) |  
[输入重定向](#输入重定向) |  
--------------------| [管道](#管道)
--------------------| [数学计算](#数学计算)
[expr](#expr) | 
[使用括号](#使用括号) | 
[浮点解决方案](#浮点解决方案) |
--------------------| [退出脚本](#退出脚本)
[核对退出状态](#核对退出状态)  |
[退出命令](#退出命令) |


#### [echo](id:echo) :  显示消息

`echo` 命令能显示一个简单的文本字符串 :   

    $ echo This is a test
    This is a test
**`Tips : `** 默认情况下不需要使用引号来标记想要显示的字符串。但如果在字符串中使用了引号, 有时会出现问题 : 

    $ echo Let's see if this'll work
    Lets see if thisll work    
`echo` 命令既可以用双引号也可以用单引号来标记文本字符串。如果要在字符串中使用它们, 需要在文本使用一种引号类型, 然后用另一种类型标记字符串 : 

    $ echo "This is a test to see if you're paying attention"
    This is a test to see if you're paying attention
    $ echo 'Rich says "scripting is easy".'
    Rich says "scripting is easy".

如果想要 `echo` 命令结果和 `echo` 文本字符串在同一行, 只需对 `echo` 语句使用 `-n` 参数即可。

[返回目录](#toc)

### [使用变量](id:变量) : 使用变量
####  **[环境变量](id:环境变量)**
可以直接输出变量 :   

        $ echo user info userid: $USER
        user info for userid: rich
可以用双引号输出变量 :   
        
        $ echo "user info userid: $USER"
        user info for userid: rich
可以用 `${variable}` 格式来引用变量。
        
        
        $ echo user info userid: ${USER}
        user info for userid: rich
如果想显示美元符号 `$`, 可以加反斜杠 : 
    
        $ echo "The cost of the item is \$15"
        The cost of the item is $15

####  **[用户变量](id:用户变量)**
用户变量可以是由不超过 `20` 个字符的 *字母*、*数字* 或 *下划线* 组成的文本字符串。用户变量 *区分大小写*。  
**`Tips : `**在变量、等号和变量值之间不允许有空格。

        var1=10
shell 脚本自动为变量值确定数据类型。脚本中定义的变量在 shell 脚本的生命周期内保留它们的值,但当 shell  脚本完成时就被删除了。

####  **[反引号](id:反引号)**
反引号允许将 shell 命令的输出赋值给变量。
    
        #!/bin/bash
        testing=`date`
        echo $testing
还有写日志的例子 : 
        
        #!/bin/bash
        today=`date +%y%m%d`
        ls /usr/bin -al >log.$today

[返回目录](#toc)

### [重定向输入输出](id:输入输出)

#### [输出重定向](id:输出重定向)

重定向的最基本类型是通过一条命令将输出发送到文件中或将文件内容重新改写。bash shell 重定向为此使用大于号 :  

    Command > outputfile
有时可能需要将命令的输出附加到现有文件, 而不是重写此文件内容。可以用两个大于号 (`>>`) 附加数据 : 
    
    $ date >> testfile

[返回目录](#toc)

#### [输入重定向](id:输入重定向)

输入重定向是将一个文件的内容重定向到一条命令中。
输入重定向符号是小于号 (`<`)。

    command < inputfile
输入重定向还有另一个方法, 称为 **`内置输入重定向`**。这种方法允许在命令行中而非文件中为输入重定向指定数据。

内置输入重定向符号是两个小于号(`<<`)。除了这个符号, 还必须指定一个文本标记来说明输入数据的开始和结尾。文本标记可以使用任何字符串值, 但在数据的开始和结尾处必须相同 :   

    command << marker
    data
    marker
例子如下 : 

    $ wc << EOF
    > test string 1
    > test string 2
    > EOF
    
[返回目录](#toc)

### [管道](id:管道)

可以将输出重定向到另一条命令, 而不是将命令的输出重定向到一个文件。这个过程称为 **`管道传送`**。管道的符号是竖条操作符(`|`) :  

    command1 | command2
Linux 系统实际上同时运行两条命令, 并在系统内部把它们连接在一起。第一条命令生成输出时, 输出就立即发送给第二条命令。没有使用中间文件或者缓冲区来传递数据。

    cat testfile | sort > sortfile

[返回目录](#toc)

### [数学计算](id:数学计算)
#### [expr命令](id:expr)

`expr` 命令允许处理命令行中的等式, 但是很笨拙。
    
    $ expr 1+5
    6
`expr` 命令能够区分一此坏同的数学操作符和字符串操作符, 如下表 :   
**expr 命令操作符**

操作符 | 描述
----|---
`ARG1 \| ARG2` | 如果两个参数都不为空或都不为0, 返回 ARG1; 否则, 返回 ARG2
`ARG1&ARG2` | 如果两个参数都不为空或都不为0, 返回 ARG1;否则, 返回 0
`ARG1<ARG2` | 如果 ARG1 小于 ARG2, 返回 1; 否则, 返回 0
`ARG1<=ARG2` | 如果 ARG1 小于等于 ARG2; 返回1, 否则, 返回0
`ARG1=ARG2` | 如果 ARG1 等于ARG2; 返回1;否则, 返回0
`ARG1!=ARG2` | 如果 ARG1 不等于 ARG2, 返回 1; 否则, 返回0
`ARG1>=ARG2` | 如果 ARG1 大于等于 ARG2, 返回1; 否则, 返回0
`ARG1>ARG2` | 如果 ARG1 大于 ARG2, 返回1; 否则, 返回0
`ARG1+ARG2` | 返回 ARG1 与 ARG2 的和
`ARG1-ARG2` | 返回 ARG1 与 ARG2 的差
`ARG1*ARG2` | 返回 ARG1 与 ARG2 的积
`ARG1/ARG2` | 返回 ARG1 与 ARG2 的商
`ARG1%ARG2` | 返回 ARG1 与 ARG2 的余
`STRING:REGEXP` | 如果 REGEXP 匹配 STRING 中的一个模式, 返回该模式
`match STRING REGEXP` | 如果 REGEXP 匹配 STRING 中的一个模式, 返回该模式
`substr STRING POS LENGTH` | 从 POS 位置起始(始于1), 返回长度为 LENGTH 字符
`index STRING CHARS` | 返回在 STRING 中找到 CHARS 的位置, 否则返回0
`length STRING` | 返回字符串 STRING 的长度
`+ TOKEN` | 将 TOKEN 解释为一个字符串, 即使它是一个关键字
`(EXPRESSION)` | 返回 EXPRESSION 值

**`Tips : `** shell 中许多 `expr` 命令操作符 (如星号) 有其他含义。在 `expr` 命令中使用它们会生成一些奇怪的结果 :   
    
    $ expr 5 * 2
    expr : syntax error
    
    要解决解析错误, 可以用shell转义字符 (反斜杠)
    $ expr 5 \* 2
    10

[返回目录](#toc)

#### [使用括号](id:使用括号)

在 bash 中, 为一个变量指定一个数学值时, 可以用美元符号和方括号(`$[operation]`) 把数学等式括起来 : 

    $ var1=$[1 + 5]
    $ echo $var1
    6
    
    用于脚本中 : 
    #!/bin/bash
    var1=100
    var2=50
    var3 = $[$var1 - $var2]
    echo $var3        //50
**`Tips : `** 使用方括号方法计算等式时, 不必担心 shell 错误理解乘法符号或者其他符号。shell 知道它不是一个通配符, 因此它在方括号中。

    #!/bin/bash
    var1=100
    var2=45
    var3=$[$var1/$var2]
    echo $var3    //2
**`Tips : `** bash shell 的数学操作只支持整数算法。如果任何类型实际的计算, 这是一个很大的限制。

[返回目录](#toc)

#### [浮点解决方案](id:浮点解决方案)

克服整数限制的解决方案, 最普遍的解决方法是内置的 bash 计算器 (称为 `bc`)。

1. **浮点解决方案**
bash 计算器实际上是一种编程语言, 该语言允许在命令 行中输入浮点表达式, 然后解释表达式并计算它们, 最后返回结果。bash 计算器可以识别 : 

    >   - 数字 (整数和浮点)
    - 变量 (简单变量和数组)
    - 注释 (以英镑符号开始的行或 C 语言的/**/对)
    - 表达式
    - 编程语句 (如 if-then 语句)
    - 函数  

        $ bc
        $ 12 * 5.4
        64.8
        $ quit
浮点算术被称为 `scale` 的内置变量控制, 默认值为 0。必须把这个值设置为想要的十进制小数位数, 否则得不到想要的结果 :   
        
        $ bc -q
        $ 3.44 / 5
        0
        $ scale=4
        $ var1 = 3.44 / 5
        $ print var1
        .6880
        $ quit
2. **脚本中使用 bc**
**可以用反引号运行 `bc` 命令, 并且把输出赋值给一个变量**。 使用格式为 : 

        variable=`echo "options; expression" | bc`
第一部分 `options` 允许设置变量。如果需要设置多个变量, 使用分号将它们隔开。 `expression` 参数定义了使用 `bc` 计算的数学表达式。这看起来有点怪, 但它运行起来很快。

        #!/bin/bash
        var1=`echo " scale=4; 3.44 / 5 " | bc`
        echo $var1    //.6880
**在 shell 脚本中定义的变量** : 
        
        #!/bin/bash
        var1=100
        var2=45
        var3=`echo "scale=4; $var1 / $var2" | bc`
        echo $var3    //2.2222
**还可以使用内置输入重定向方法, 而不使用文件重定向。**内置重定向允许从命令行直接重定向数据。在 shell 脚本中, 可以把输出赋值给变量。如下 :   

        variable=`bc << EOF
        options
        statements
        expressions
        EOF
        ` 
示例如下 :   
        
        #!/bin/bash
        var1=10.46
        var2=43.67
        var3=33.2
        var4=71
        
        var5=`bc << EOF
        scale = 4
        a1 = ( $var1 * $var2 )
        b1 = ( $var3 * $var4 )
        a1 + b1
        EOF
        `
        echo $var5    //2813.9882
**`Tips : `** 在 bash 计算器中可以给变量赋值。但在 bash 计算器内创建的变量只在 bash 计算器内有效, 而不能用在 shell 脚本中。
        
[返回目录](#toc)

### [退出脚本](id:退出脚本)
为向 shell 表明, 命令已经处理完毕, 每条运行在 shell 中的命令都使用一个 **退出状态**。 这个退出状态是一个介于 0 和 255 之间的整数值, 当命令运行完成时, 命令就会把退出状态传递给 shell 。可以捕捉这个值并在您的脚本中使用它。

#### [核对退出状态](id:核对退出状态)
Linux 提供 `$?` 特殊变量来保存最后一条命令执行结果的退出状态。如果核对一条命令的退出状态, 必须在这条命令运行完成之后立即查看或使用变量 `$?`。 它会改变为 shell 执行的最后一条命令的退出状态值 :  

    $ date
    xxxx
    $ echo $?
    0
**`Tips : `** 按照惯例, 一条命令成功完成的退出状态是 0。如果命令执行错误, 那么退出状态就会是一个正整数 :   

    $ asdfg
    -bash : asdfg: command not found
    $ echo $?
    127
**Linux 退出状态代码**

代码 | 描述
----|---
0 | 命令成功完成
1 | 通常的未知错误
2 | 误用 shell 命令
126 | 命令无法执行
127 | 没找到命令
128 | 无效的退出参数
128+x | 使用 Linux 信号 x 的致命错误
130 | 使用 Ctrl-C 终止命令
255 | 规范外的退出状态

[返回目录](#toc)

#### [退出命令](id:退出命令)
`exit` 命令允许在脚本结束时, 指定一个退出状态 : 

    $ cat testfile
    #!/bin/bash
    var1=10
    var2=30
    var3=$[ $var1 + $var2]
    echo $var3
    exit 5

    $ ./testfile
    40
    $ echo $?
    5

退出状态码也可以与变量使用 : 

    exit $var
**`Tips : `** 退出状态码只能在 0～255 范围之内。shell 通过使用模计算做到了这一点。一个值的模是除操作之后的余数。运算结果就是那个特定的数被 256 除了之后的余数。
    
    
    $ exit 300
    $ echo $?
    44
[返回目录](#toc)



