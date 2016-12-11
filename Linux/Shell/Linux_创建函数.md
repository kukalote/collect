
## Linux_创建函数

### 14 [创建函数](id:toc)


- 14.1 [基本脚本函数](#基本脚本函数)
    - 14.1.1 [创建函数](#创建函数)
    - 14.1.2 [使用函数](#使用函数)

- 14.2 [返回值](#返回值)
    - 14.2.1 [默认退出状态](#默认退出状态)
    - 14.2.2 [使用 return 命令](#使用 return 命令)
    - 14.2.3 [使用函数输出](#使用函数输出)

- 14.3 [在函数中使用变量](#在函数中使用变量)
    - 14.3.1 [向函数传递参数](#向函数传递参数)
    - 14.3.2 [在函数中处理变量](#在函数中处理变量)

- 14.4 [数组变量与函数](#数组变量与函数)
    - 14.4.1 [向函数传递数组](#向函数传递数组)
    - 14.4.2 [从函数返回数组](#从函数返回数组)

- 14.5 [函数递归](#函数递归)

- 14.6 [创建库](#创建库)

- 14.7 [在命令行中使用函数](#在命令行中使用函数)
    - 14.7.1 [在命令行创建函数](#在命令行创建函数)
    - 14.7.2 [在 .bashrc 文件中定义函数](#在 .bashrc 文件中定义函数)




#### 14.1 [基本脚本函数](id:基本脚本函数)  

**函数** 是被赋予名称的脚本代码块, 可以在代码的任意位置重用。每当需要在脚本中使用这样的代码时, 只需引用该代码块被赋予的函数名称 ( 称为 **调用** 该函数 )。

##### 14.1.1 [创建函数](id:创建函数)

在bash shell 脚本中创建函数可以使用两种格式 : 

一种格式是使用关键字 `function`, 后跟代码块的函数名 :  

    function name {
        commands
    }
>`name` 属性定义了该函数的唯一名称。脚本中自定义的每个函数都必须赋予唯一的名称。      
`commands` 是组成函数的一条或多条 bash shell 命令。当调用函数时, 就像在普通脚本中一样, bash shell 按照各条命令在函数中出现的顺序依次执行。  

在 bash shell 脚本中定义函数还有另一格式, 它更接近一般编程语言定义函数的方式 :  

    name() {
        commands
    }

[返回目录](#toc)


##### 14.1.2 [使用函数](id:使用函数)

要在脚本中使用函数, 相应函数名需要在脚本行中指定, 这与其他 shell 命令的用法一样 : 

    #!/bin/bash
    
    function func1 {
        echo "This is an example of a function "
    }
    
    count=1
    while [ $count -el 5 ]
    do
        func1
        count=$[ $count + 1 ]
    done
    echo "This is the end of the loop"
    func1
    echo "Now this is the end of the script"
    
**`Tips : `** 函数定义一定位于 shell 脚本的起始部分, 但是应当小心使用。如果在函数定义之前使用函数, 会得到错误消息。

**`Tips : `** 函数的命令也需要注意。请记住, 每个函数名必须唯一, 否则会出问题。如果重新定义函数, 那么新定义将取代函数原先的定义, 这不会引发错误消息。

[返回目录](#toc)


#### 14.2 [返回值](id:返回值)

bash shell 将函数看作小型脚本, 并以退出状态结束。函数退出状态有 3 种生成方式。

##### 14.2.1 [默认退出状态](id:默认退出状态)

默认情况下, 函数的退出状态是函数的最后一条命令返回的退出状态。函数执行完毕之后, 可以使用标准变量 `$?` 来确定函数的退出状态。

    #!/bin/bash
    
    func1 () {
        echo "trying to display a non-existent file"
        ls -l badfile
    }
    echo "testing the function: "
    func1
    echo "The exit status is :$?"
    
    $ ./testfile
    testing the function:
    trying to display a non-existent file
    ls: badfile: No such file or directory
    The exit status is :1
    
不过, 可能会由于方法前面一条命令执行失败, 但后面还有执行成功的命令而使退出状态为 0。所以这种方法并不安全。

[返回目录](#toc)


##### 14.2.2 [使用 return 命令](id:使用 return 命令)

bash shell 使用 `return` 命令以特定退出状态退出函数。 `return` 命令可以使用单个整数值来定义函数退出状态, 提供了一种通过编程设置函数退出状态的简单方法 :  

    #!/bin/bash
    
    function db1 {
        read -p "Enter a value: " value
        echo "doubling the value"
        return $[ $value * 2 ] 
    }
    
    db1
    echo "The new value is $?"
    
    $ ./testfile
    Enter a value: 23
    doubling the value
    The new value is 46
    
**`Tips : `** 但是使用这种方法从函数返回数据时, 必须注意避免两个容易发生的错误 :  

- 请记住在函数完成后尽快提取返回值;
- 请记住退出状态的取值范围是 0~255。

如果在通过使用变量 `$?` 提取函数值之前执行了其他命令, 那么该函数的返回值将丢失(请记住, 变量 `$?` 返回最近已执行登记的退出状态)。

如果想要返回大于 255 的整数 或者 字符串, 那么就不要使用这种返回值方法。

[返回目录](#toc)


##### 14.2.3 [使用函数输出](id:使用函数输出)

正如命令输出可以捕获并存放在 shell 变量中一样, 函数的输出也可以捕获并存放在 shell 变量中。这种方法以可以从函数获取任意类型的输出并给变量赋值 : 

    result=`db1`

这条命令将函数 db1 的输出赋予变量 $result。在脚本中使用这种方法示例如下 : 

    #!/bin/bash
    
    function db1 {
        read -p "Enter a value: " value
        echo $[ $value * 2 ]
    }
    result=`db1`
    echo "The new value is $result"
    
    $ ./testfile
    Enter a value: 200
    The new value is 400
这个新函数使用 `ehco` 语句显示计算结果。该脚本仅捕获函数 db1 的输出, 而不查看结果的退出状态。bash shell 脚本比较聪明, 不会将些消息作为 STDOUT 输出的组成部分, 而将其忽略。

**`Tips : `** 这个方法也可以返回浮点数和字符串值, 所以这种方法能够非常灵活地从函数返回数据。

[返回目录](#toc)


#### 14.3 [在函数中使用变量](id:在函数中使用变量)


##### 14.3.1 [向函数传递参数](id:向函数传递参数)

函数可以使用标准参数环境变量来表示命令行传递给函数的参数。在脚本中指定函数时, 必须在函数所在命令行提供参数 :   

    func1 $value1 10
这样, 该函数才可以使用参数变量获取参数值。使用这种方法向函数传递值 :  

    #!/bin/bash
    
    function addem {
        if [ $# -eq 0 ] || [ $# -gt 2 ]
        then
            echo -1
        elif [  $# -eq 1 ]
        then 
            echo $[ $1 + $1 ]
        else
            echo $[ $1 + $2 ]
        fi
    }
    
    echo -n "Adding 10 and 15 : "
    value=`addem 10 15`
    echo $value
    
    $ ./testfile 
    Adding 10 and 15 :
    25
    
**`Tips : `**  由于函数为自己的参数值使用专用的参数环境变量, 所以函数无法从脚本命令行直接访问脚本参数值。请看下例 :   

    #!/bin/bash
    
    function badfunc1 {
        echo $[ $1 * $2 ]
    }
    
    result=`badfunc1`            #方法里的 $1 、$2 变量应该在这里进行支持
    echo "The result is $result"
    
    $ ./testfile
    ./testfile: line 3: +  : syntax error: operand expected (error token is " ")

请参看下面例子 : 

    #!/bin/bash
    
    function func {
        echo $[ $1 * $2 ]
    }
    
    if [ $# -e1 2 ]
    then 
        value=`func $1 $2`
        echo "The result is $value"
    else
        echo "Usage:badtest1 a b"
    fi
    
    $ ./testfile 3 4
    The result is 12
    
    $ ./testfile
    Usage:badtest1 a b
    
[返回目录](#toc)


##### 14.3.2 [在函数中处理变量](id:在函数中处理变量)

变量 **作用域** 是 shell 脚本程序员遇到常见问题。作用域是变量的可见区域。函数内定义的变量与普通变量有不同的作用域, 前者能被脚本外部定义的变量所覆盖。

函数使用两种类型的变量 :  
- 全局变量; 
- 局部变量。

1. **全局变量**  

    **全局变量** 是在 shell 脚本内处处有效的变量。如果脚本的主代码定义了全局变量, 那么在函数内部可以获取这个变量的值。同样, 如果在函数内部定义了全局变量, 那么脚本的主代码也可以获取该变量的值。
    
    默认情况下, 脚本中定义的变量都是全局变量。在函数外部定义的变量, 在函数内部仍然正常访问 :   
    
        #!/bin/bash
        function db {
            value=$[ $value * 2 ]
        }
        
        read -p "Enter a value: " value
        db
        echo "The new value is : $value "
        
        $ ./testfile
        Enter a value: 3
        The new value is : 6

2. **局部变量**  

    与全局变量相对, 函数内部使用的变量可以称为局部变量。这只需在变量声明前冠以 `local` 关键字 :  

        local temp
    也可以给局部变量赋值语句冠以 `local` 关键字:  
    
        local temp=$[ $value + 5 ]    
    关键字 `local` 确保变量公在函数内部使用。如果脚本在函数自问有同名变量, 那么 shell 将能区分开这两个变量。这样就可以很容易地将函数变量与脚本变量分开, 而只共享需要的变量 :  
    
        #!/bin/bash
        
        function func {
            local temp=$[ $value + 5 ]    #函数内部使用 temp 不会对主代码变量 $temp 的值产生影响
            result=$[ $temp * 2 ]
        }
        
        temp=4
        value=6
        
        func
        echo "The result is $result"
        if [ $temp -gt $value ]
        then
            echo "temp is larger"
        else
            echo "temp is smaller"
        fi
        
        $ ./testfile
        The result is 22
        temp is smaller

[返回目录](#toc)


#### 14.4 [数组变量与函数](id:数组变量与函数)

##### 14.4.1 [向函数传递数组](id:向函数传递数组)

向脚本函数传递数组变量的技术不太容易理解。如果试图将数组变量作为单个参数传递, 是无法正常工作的 :   

    #!/bin/bash
    
    function test {
        echo "The parameters are : $@ "
        thisarray=$1
        echo "The received array is ${thisarray[*]}"
    }
    
    myarray=(1 2 3 4 5)
    echo "The original array is : ${myarray[*]}"
    test $myarray
    
    $ ./testfile
    The original array is : 1 2 3 4 5
    The parameters are : 1
    The received array is 1
    
如果试图将数组变量作为函数参数使用, 那么函数只提取数组变量的第一个取值。  

要解决这个问题, 必须将数组变量拆分为单个元素, 然后使用这些元素的值作为函数参数。函数内部可以再将这些参数重组为新数组变量 : 

    #!/bin/bash

    function test {
        local sum=0
        local newarray
        newarray=(`echo "$@"`)
        echo "The new array value is: ${newarray[*]}"

        for value in ${newarray[*]}
        do
            sum=$[ $sum + $value ]
        done
        echo $sum
    }

    myarray=(1 2 3 4 5)

    echo "The original array is ${myarray[*]}"
    result=`test ${myarray[*]}`
    echo "The result is $result"
    
    $ ./testfile
    The original array is 1 2 3 4 5
    The new array value is: 1 2 3 4 5
    The result is 15

[返回目录](#toc)


#### 14.4.2 [从函数返回数组](id:从函数返回数组)

类似方法也可用于从函数向 shell 脚本回传数组变量。函数使用 `echo` 语句以恰当顺序输出数组各元素的值, 然后脚本必须将这些数据重组为新数组变量 :  

    #!/bin/bash
    
    function arraydblr {
        local origarray
        local newarray
        local elements
        local i
        
        origarray=(`echo "$@"`)
        newarray=(`echo "$@"`)
        elements=$[ $# - 1 ]
        for (( i = 0; i <= $elements; i++ ))
        {
            newarray[$i]=$[ ${origarray[$i]} * 2 ]
        }
        echo ${newarray[*]}
    } 
    
    myarray=(1 2 3 4 5)
    echo "The original array is : ${myarray[*]}"
    arg1=`echo ${myarray[*]}`
    result=(`arraydblr $arg1`)
    echo "The new array is : ${result[*]}"

    $ ./testfile
    The original array is : 1 2 3 4 5
    The new array is : 2 4 6 8 10
    
[返回目录](#toc)


#### 14.5 [函数递归](id:函数递归)

**自给** (self-containment) 是局部函数变量的一个特性。自给函数除了脚本通过命令行传递的变量, 不使用函数之处的任何资源。

这种特性使函数能够以 **递归** 方式调用。递归调用函数是指函数调用自身进行求解。通常, 递归函数有基值, 函数最终递推达到该值。许多高级数学算法使用递归将复杂等式的递归层次反复降低, 直到到达基值指定的层次。

递归算法的一个经典示例是计算阶乘 : 

    5! = 1 * 2 * 3 * 4 * 5 = 120
    这个等式你爱我吗递归可以归结为 : 
    x! = x * (x-1)!
    
这可以表述为简单的递归脚本 :   

    #!/bin/bash
    
    function factorial {
        if [ $1 -eq 1 ]
        then 
            echo 1
        else
            local temp=$[ $1 - 1 ]
            local result=`factorial $temp`
            echo $[ $result * $1 ]
        fi
    }
    
    read -p "Enter value: " value
    result=`factorial $value`
    echo "The factorial of $value is : $result"
    
    $ ./testfile
    Enter value: 3
    The factorial of 3 is : 6

[返回目录](#toc)


#### 14.6 [创建库](id:创建库)

bash shell 可以创建函数的 **库文件**, 然后可以在不同脚本中引用该库文件。

这种方法首先要创建公共库文件, 包含多个脚本需要调用的函数。一个简单库文件称为 myfuncs, 定义了 3 个简单的函数 : 

    # my script functions
    
    function addem {
        echo $[ $1 + $2 ]
    }
    
    function multem {
        echo $[ $1 * $2 ]
    }
    
    function divem {
        if [ $2 -ne 0 ]
        then 
            echo $[ $1 / $2 ]
        else
            echo -1
        fi
    }
使用函数库的关键是 `source` 命令。`source` 命令在当前 shell 环境中执行命令, 而非创建新 shell 来执行命令。使用 `source` 命令在 shell 脚本内部运行库文件脚本。这样脚本可以使用这些函数。

`source` 有一个短小的别名, 称为 **点操作符**。为了在 shell 脚本中调用 myfuncs 库文件, 只需添加下列命令行 :  
    
    . ./myfuncs
    
下面示例假定 myfuncs 库文件与 shell 脚本在同一目录 :   

    #!/bin/bash
    . ./myfuncs
    
    val1=10
    val2=5
    result=`addem $val1 $val2`
    echo "The result of adding them is : $result"
    
    $ ./testfile
    The result of adding them is : 15
    
[返回目录](#toc)


#### 14.7 [在命令行中使用函数](id:在命令行中使用函数)

脚本函数不仅可以用作 shell 脚本命令, 也可以用作命令行界面的命令。这个特性很好, 因为一旦在 shell 中定义了函数, 那么就可以从系统的任意目录使用这个函数。不必担心 PATH 环境变量是否包含函数文件所在的目录。

##### 14.7.1 [在命令行创建函数](id:在命令行创建函数)

shell 在键盘输入命令时解释命令, 函数可以直接在命令行定义。这有两种方法 :   

第一种方法将函数定义在一行命令中 :  

    $ function divem { echo $[ $1 / $2 ] }
    $ divem 100 5
    20

在命令行中定义函数时, 每条命令的结尾必须包含分号, 这样 shell 才知道命令在哪里分开 :  

    $ function doubleit { read -p "Enter value: " value; ehco $[
    $ value * 2 ]; }
    $ doubleit
    Enter value: 20
    40
    
第二种方法是使用多行命令定义函数。这样, bash shell 使用次提示符提示输入更多命令。使用这种方法不需要在每条命令的结尾添加分号, 只需按 Enter 键 :  

    $ funtion multem {
    > echo $[ $1 * $2 ]
    > }
    $ multem 2 5
    10
在函数末尾使用大括号时, shell 知道定义函数结束。

**`Tips : `** 这种方式可能会覆盖到其他同名的方法。

[返回目录](#toc)


##### 14.7.2 [在 .bashrc 文件中定义函数](id:在 .bashrc 文件中定义函数)

要将函数定义放在 shell 每次启动都能重新载入的地方。 `.bashrc` 文件就是这样的地方。每次无论 bash shell 是交互式启动, 还是从已有 shell 启动新 shell , 都会在目录下查找这个文件。

1. **直接定义函数**  

    在主目录下的 `.bashrc` 文件中可以直接定义函数, 只需在已有文件内容末尾添加自己定义的函数。
    
2. **提供函数文件**  

    可以用 `source` 命令 ( 或称点操作符 )将现有库文件的函数包含进 `.bashrc` 脚本。

[返回目录](#toc)





    

