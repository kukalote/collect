### Linux_使用结构化命令
有一类命令, 允许脚本根据变量值的条件或者其他命令的结果跳过一些命令或者循环执行这些命令。这类命令通常被称为 [**结构化命令**](id:toc)。

- [`if-then`](#ifthen)
- [`if-then-else `](#ifthenelse)
- [`test 命令`](#test)
    - [数值比较](#数值比较)
    - [字符串比较](#字符串比较)
    - [文件比较](#文件比较)
- [`复合条件检查`](#复合条件检查)
- [`if-then 的高级特征`](#ifthen的高级特征)
    - [双圆括号](#双圆括号)
    - [双方括号](#双方括号)
- [`case 命令`](#case)
- [`for 命令`](#for)
- [`C 式的 for 命令`](#cfor)

#### [使用 if-then 语句](id:ifthen)
最基本的结构化命令类型就是 `if-then` 语句。 `if-then` 语句格式如下 :  

	if command
	then
		commands
	fi
	
	if command; then
		commands
	fi
bash shell 中的 `if` 语句运行在 `if` 行定义的命令。如果命令的退出状态是 **`0`** (成功执行命令)[^1], 将执行 `then` 后面的所有命令。如果命令的退出状态是 **`0`** 以外的其他值, 那么 `then` 后面的命令将不会执行, bash shell 会移动到脚本的下一个命令。
[^1]: 请看Linux_脚本基础知识。

例子 :   

	#!/bin/bash
	if date
	then
		echo "it worked"
	fi
	
	这是另一个一定不成功的脚本
	#!/bin/bash
	if asdfg				//此处报错
	then 
		echo "it worked"
	fi
	echo "it did't work"	//只输出这一条
[返回目录](#toc)

#### [使用 if-then-else 语句](id:ifthenelse)
`if-then-else` 语句在语句中提供了另一组命令 : 

	if command
	then
		commands
	else
		commands
	fi
如果 `if` 语句行的命令返回的退出状态码为 `0`, 就会执行在 `then` 部分列出的命令, 就像在普通的 `if-then` 语句中一样。如果 `if` 语句行的命令返回非零退出状态码, bash shell 将会执行 `else` 部分的命令。
	
	#!/bin/bash
	testuser=badtest
	if grep $testuser /etc/passwd
	then
		echo The files for user $testuser are:
		ls -a /home/$testuser/.b*
	else
		echo "The user name $testuser doesn't exist on this system"
	fi
**嵌套 if 语句**  

`elif` 以另一个 `if-then` 语句继续 `else` 部分 : 

	if command1
	then
		commands
	elif command2
	then
		more commands
	fi
命令块的执行依赖于哪条命令的退出状态码为 `0`。 记住, bash shell 会按顺序执行 `if` 语句, 只有第一个返回 `0` 退出状态的命令会导致 `then` 部分命令被执行。

[返回目录](#toc)
#### [test 命令](id:test)
`test` 命令提供一种检测 `if-then` 语句中不同条件的方法。如果 `test` 命令中列出的条件评估值为 true, `test` 命令以 `0` 退出状态代码退出, 这使 `if-then` 语句使用与其他编程语言中的 `if-then` 语句一样的方法运行。如果条件为 false, 则 `test` 命令退出, 使得 `if-then` 语句失败。

	test condition
`condition` 是一系列 `test` 命令评估的参数和值。在 `if-then` 语句中使用时, `test` 命令如下 : 

	if test condition
	then
		commands
	fi
	
bash shell 提供一种在 `if-then` 语句中声明 `test` 命令的另一种方法 : 
	
	if [ condition ]
	then 
		commands
	fi
**`Tips : `** 方括号定义在 `test` 命令中使用的条件。注意, 在前半个方括号的后面和后半个方括号的前面必须都有一个空格, 否则会得到错误信息。  

`test` 命令能够评估以下 3 类条件 :  

- 数值比较;
- 字符串比较;
- 文件比较。
[返回目录](#toc)

##### [数值比较](id:数值比较)

**test 数值比较**

比较 | 描述
----|---
`n1 -eq n2` | 检查 n1 是否等于 n2
`n1 -ge n2` | 检查 n1 是否大于或等于 n2
`n1 -gt n2` | 检查 n1 是否大于 n2
`n1 -le n2` | 检查 n1 是否小于或等于 n2
`n1 -lt n2` | 检查 n1 是否小于 n2
`n1 -ne n2` | 检查 n1 是否不等于 n2

数值测试条件能用于估计数字和变量 : 

	#!/bin/bash
	val1=10
	val2=11
	
	if [ $val1 -gt 5 ]
	then 
		echo "The test value $val1 is greater than 5 "
	fi
	
	if[ $val1 -eq $val2 ]
	then 
		ehco "The values are equal"
	else
		echo "The values are different"
	fi

但 `test` 数值条件有一个限制 : 

	#!/bin/bash
	val1=' echo "scale=4; 10 / 3 " | bc'
	echo "The test value is $val1"
	if [ $val1 -gt 3 ]
	then
		echo "The result is larger than 3"
	fi
	
	$ ./testfile
	The test value is 3.3333
	./testfile: line 4: [: 3.3333: integer expression expected
`test` 命令无法处理存储在变量 val1 中的浮点值。

**`Tips : `** bash shell 只能处理整数数字。使用 bash 计算器时, 只是欺骗 shell 把浮点值作为字符串值存储于一个变量。但在面向数值函数 (如果数值测试条件) 中不起作用。底线是不能在 `test` 命令中使用整数。

[返回目录](#toc)
##### [字符串比较](id:字符串比较)
`test` 命令允许对字符串进行比较。执行字符串的比较可能会很复杂。

**test 命令字符串比较**

比较 | 描述 
----|---
`str1 = str2` | 检查 str1 与 str2 是否相同
`str1 != str2` | 检查 str1 与 str2 是否不同
`str1 < str2` | 检查 str1 是否小于 str2
`str1 > str2` | 检查 str1 是否大于 str2
`-n str1` | 检查 str1 的长度是否大于 0
`-z str1` | 检查 str1 的长度是否为 0 

1. **字符串相等**

		#!/bin/bash
		testuser=rich
		
		if [ $USER = $testuser ]	
		then
			echo "Welcom $testuser"
		fi
类似, 字符串不相等比较 :  
        
        #!/bin/bash
        testuser=baduser
        
        if [ $USER != $testuser ]
        then
            echo "This isn't $testuser"    //输出
        else
            echo "Welcom $testuser"
        fi
比较字符串是否相等时, 测试比较将所有标点符号和大写都考虑在内。

2. **字符串顺序**
尝试比较一个字符串比另一个字符串是大还是小时, 事情就开始变得复杂了。使用 `test` 命令的大于或者小于功能时, shell 程序员通常会遇到两个问题。
    
    - 大于和小于符号一定要转义, 否则 shell 会将它们当作重定向符号, 将字符串值看作文件名。
    - 大于和小于顺序与在 sort 命令中的顺序不同。   
    
  第一条能导致很大的错误, 而且编写脚本时通常不会发现该问题。如下 : 
  
      #!/bin/bash
      
      val1=baseball
      val2=hockey      
      
      if [ $val1 > $val2 ]
      then
          echo "$val1 is greater than $val2"
      else
          echo "$val1 is less than $val2"
      fi
      
      $ ./badtest
      baseball is greater than hockey
      $ ls -l hockey
      -rw-r--r-- 1 rich rich 0 Sep 30 19:08 hockey
  *在脚本中只单独使用大于号, 没有生成错误, 但结果不对。脚本将大于号理解为输出重定向, 所以合建了一个 hockey 文件。由于重定向成功完成, 所以测试命令返回退出状态码 0, `if` 语句认为事情完成。*
  
  要解决这个问题, 需要适当地转义大于号 :  
      
      #!/bin/bash
      
      val1=baseball
      val2=hockey      
      
      if [ $val1 > $val2 ]
      then
          echo "$val1 is greater than $val2"
      else
          echo "$val1 is less than $val2"
      fi
      $ ./testfile
      baseball is less than hockey
  第二条更加微妙一点, 除非使用大写和小写字母, 否则不会遇到这种问题。`sort` 命令处理大写字母的方法与 `test` 命令处理大写字母的方法相反。
  
      #!/bin/bash
      val1=Testing
      val2=testing
      
      if [ $val1 \> $val2 ]
      then
          echo "$val1 is greater than $val2"
      else
          echo "$val1 is less than $val2"
      fi
      $ ./testfile
      Testing is less than testing
      $ sort testfile
      testing
      Testing
  
  test | sort
  ----|---
  使用字母的 ASCII 码值大小排序 | 以系统当前语言设置定义的排列顺序(英语中小写字母排列顺序在大写字母之前)
  `小写字母 > 大写字母` | `大写字母 > 小写字母`
  
3. 字符串大小
评估一个变量是否包含数据时, 使用 `-n` 和 `-z` 比较很方便 : 
    
        #/bin/bash
        val1=testing
        val2=''
        
        if [ -n $val1 ]        //确定变量 val1 的长度是否为非零值, 如果是非零值, 那么将处理 then 部分
        then 
            echo "The string '$val1' is not empty"    //输出
        else
            echo "The string '$val1' is empty"
        fi
        
        if [ -z $val2 ]        //确定变量 val2 的长度是否为0, 如果是 0, 那么将处理 then 部分
        then 
            echo "The string '$val2' is empty"        //输出
        else
            echo "The string '$val2' is not empty"
        fi
        
        if [ -z $val3 ]        //确定变量 val3 的长度是否为0, 这个变量没有在 shell 脚本中定义, 所以字符串长度仍然为零, 即使它没有定义。
        then
            echo "The string '$val3' is empty"        //输出
        else
            echo "The string '$val3' is not empty"
        fi
**`Tips : `** 空变量和没有初始化的亦是可能会对 shell 脚本 测试产生灾难性的影响。如果不确定变量的内容, 在数值比较或者字符串比较中使用它之前, 最好使用 `-n` 或者 `-z` 测试一下它是否有值。

[返回目录](#toc)
##### [文件比较](id:文件比较)

`test` 命令能够测试 Linux 文件系统上的文件状态和路径。

**test 命令文件比较**

比较 | 描述
----|---
`-d file` | 检查 file 是否存在并且是一个目录
`-e file` | 检查 file 是否存在(不区分目录还是文件)
`-f file` | 检查 file 是否存在并且是一个文件
`-r file` | 检查 file 是否存在并且可读
`-s file` | 检查 file 是否存在并且不为空
`-w file` | 检查 file 是否存在并且可写
`-x file` | 检查 file 是否存在并且可执行
`-O file` | 检查 file 是否存在并且被当前用户拥有
`-G file` | 检查 file 是否存在并且默认组是否为当前用户组
`file1 -nt file2` | 检查 file1 是否比 file2 新
`file1 -ot file2` | 检查 file1 是否比 file2 旧

1. **检查目录**
`-d` 测试检查指定的文件是否作为目录存在于系统中。如果想将文件写到一个目录下, 或者在试图改变到目录位置之前, 这通常是一个很好的操作 :   

        #!/bin/bash
        if [ -d $HOME ]
        then
            echo "Your HOME directory exists"    //执行
            cd $HOME
            ls -a
        else
            echo "There's a problem with your HOME directory"
        fi
- **检查文件日期**
`-nt` 比较确定一个文件是否比另一个文件 新。如果一个文件更新, 它就有一个更近的文件创建时间。`-ot` 比较确定一个文件是否比另一个文件更旧。如果文件更旧, 它就会有一个比较久远的创建时间 : 
    
        #!/bin/bash
        
        if [ ./test19 -t ./test18 ]
        then 
            echo "The test19 file is newer than test18"             //输出
        fi
        
        if [ ./test17 -ot ./test19 ]
        then 
            echo "The test17 file is older than the test19 file"    //输出
        fi
运行脚本的目录不同, 在比较中使用的文件路径就不同。如果正在检查的文件可以四处移动, 就会产生问题。另一个问题是, 这个比较都不能用于检查文件是否一开始就存在。

        #!/bin/bash
        
        if [ ./badfile1 -nt .badfile2 ]
        then 
            echo "The badfile1 file is newer than badfile2"
        else
            echo "The badfile2 file is newer than badfile1"    //输出
        fi
这个小示例表明, 如果文件不存在, `-nt` 比较只是返回一个失败条件。要在 `-nt` 或 `-ot` 比较中使用文件, 必须首先确定该文件存在。

[返回目录](#toc)
#### [复合条件检查](id:复合条件检查)
`if-then` 语句可以使用布尔逻辑来合并检查条件。可以使用两个布尔操作符 :   

- `[ condition1 ] && [ condition2 ]`
- `[ condition1 ] || [ condition2 ]`  

第一个布尔操作使用 `AND` 布尔操作符来合并两个条件, 两个条件必须都满足才执行 `then` 部分。   
第二个布尔操作使用 `OR` 布尔操作符来合并两个条件。如果任意一个条件评估为 `true`, 那么就会执行 `then` 部分。

    #!/bin/bash
    
    if [ -d $HOME ] && [ -w $HOME/testing ]
    then
        echo "The file exists and you can write to it"
    else
        echo "I can't write to the file"
    fi
[返回目录](#toc)
#### [if-then 的高级特征](id:ifthen的高级特征)
`if-then` 中提供的高级功能 :  

- 双圆括号表示数学表达数;
- 双方括号表示高级字符串处理函数。

[返回目录](#toc)
##### [使用双圆括号](id:双圆括号)
**`双圆括号`** 命令允许在比较中包含高级数学公式。 `test` 命令只允许在比较中进行简单的算术操作。`双圆括号` 命令提供更多的数学符号, 这些符号是其他语言程序员习惯使用的符号。双圆括号命令的格式是 :   

    (( expression ))
术语 `expression` 可以是任何的数学赋值表达式或数学比较表达式。

**双圆括号命令符号**

符号 | 描述
----|---
`val++` | 后增量
`val--` | 后减量
`++val` | 前增量
`--val` | 前减量
`!` | 逻辑否定
`~` | 逻辑取反
`**` | 取幂(N次方)
`<<` | 逐位左移
`>>` | 逐位右移
`&` | 逐位布尔逻辑与
`\|` | 逐位布尔逻辑或
`&&` | 逻辑与
`\|\|` | 逻辑或

    #!/bin/bash
    
    val1=10
    
    if (( $val1 ** 2 > 90 ))
    then
        (( val2 = $val1 ** 2 ))
        echo "The square of $val1 is $val2"
    fi
    
    $ ./testfile
    The square of 10 is 100
**`Tips : `** 在双圆括号内的表达式中, 不必转义大于号。这是双圆括号提供的另一个高级功能。

[返回目录](#toc)
##### [使用双方括号](id:双方括号)
**`双方括号`** 命令为字符串比较提供高级功能。`双方括号` 命令的格式是 :   

    [[ expression ]]
`双方括号` 包围的 `expression` 使用在 `test` 命令中使用的标准字符串比较。但是它提供了 `test` 命令没有的一个功能, 即 **模式匹配**。

在 `模式匹配` 中, 可以定义与字符串相匹配的正则表达式 :   

    #!/bin/bash

    if [[ $USER == r* ]]
    then 
        echo "Hello $USER"
    else
        echo "Sorry, I don't know you "
    fi
    
    $ ./testfile
    Hello rich 

[返回目录](#toc)
#### [case 命令](id:case)

`case` 命令, 以列表导向格式检查单个变量的多个值 :  

    case variable in 
    pattern1 | pattern2) commands1;;
    pattern3) commands2;;
    *) default commands;;
    esac
`case` 命令将指定的变量与不同的模式进行比较。如果变量与模式匹配, shell 执行为该模式指定的命令。可以在一行中列出多个模式, 使用竖条操作符将每个模式分开。星号是与任何列出的模式都不匹配的所有值。

    #!/bin/bash
    
    case $USER in 
    rich | barbara)
        ehco "Welcom, $USER"
        echo "Please enjoy your visit";;
    testing)
        echo "Special testing account";;
    jessica)
        echo "Don't foget to log off when you're done";;
    *)
        echo "Sorry, you're not allowed here";;
    esac
    
    $ ./testfile
    Welcom, rich
    Please enjoy your visit
`case` 命令提供了一中为每个可能的变量值指定不同选项的更清楚方法。

[返回目录](#toc)


===


#### [for 命令](id:for)
bash shell 提供了 `for` 命令, 用于创建通过一系列值重复的循环。每次重复使用系列中的一个值执行一个定义的命令集。

bash shell `for` 命令的基本格式为 :  

    for var in list
    do 
        commands
    done
在参数 `list` 中提供一系列用于迭代的值。指定列表中的值有几种不同的方法。

在每次迭代中, 变量 `var` 包含列表的当前值。第一次迭代使用列表中的第一项, 第二次迭代使用第二项, 依次类推直到列表中的所有项都被使用为止。

进入 `do` 和 `done` 语句之间的命令可以是一条或多条的标准 bash shell 命令。在命令中, 变量 `$var` 包含当前迭代的列表项值。

[返回目录](#toc)

**读取列表中的值**

`for` 命令的最基本使用方法是通过在 `for` 命令中定义一列值来迭代 :  

    #!/bin/bash
    
    for test in Alabama Alaska Arizona Arkansas
    do
        echo The next state is $test
    done
    ehco $test     //Arkansas
最后一次迭代之后, 变量 `$test` 在 shell 脚本中的其他部分中仍然有效。它仍然是迭代的最后一个值。

**读取列表中的复杂值**

shell 看到列表中的单引号, 并试图用它们来定义一个单独的数据值, 它的确破坏了这个过程。

有两种方法解决这个问题 :   

- 使用转义字符 (反斜杠符号) 来转义单引号;
- 使用双引号来定义使用单引号的值。

        #!/bin/bash
        
        for test in I don\'t know if "this'll" work
        do
            echo "word:$test"
        done
        //I 、 don't、 know、 if、 this'll、 work 

另一个问题是运行多个字值。`for` 循环认为每个值都用空格分隔。如果有包含空格的数据值, 就会遇到另一个问题。如果在个别数据值中有空格, 必须使用双引号将它们包围起来。 :   
    
    #!/bin/bash
    
    for test in Nevada "New Hampshire" "New Mexico" "New York"
    do
        echo "Now goking to $test"
    done
    //Nevada "New Hampshire、 New Mexico、 New York
    
**从变量读取列表**

shell 脚本经常发生的是积累了存储于变量中的一个值列表, 然后需要通过列表迭代。也可以使用 `for` 命令做到这一点 :   

    #!/bin/bash
    
    list="Alabama Alaska Arizona Arkansas"
    list $list" Connecticut"
    
    for state in $list
    do 
        echo "Have you ever visited $state?"
    done
    
**读取命令中的值**
生成列表中使用的值的另一种方法是使用命令的输出。可以使用反引号字符执行生成输出的任何命令, 然后在 `for` 命令中使用命令的输出 :   

    #!/bin/bash
    
    file="states"
    
    for state in 'cat $file'
    do
        echo "visit beautiful $state"
    done
    
    //states内容
    Alabama
    Alaska
    Arizona
**`Tips : `** 文件 states 每行只包含一个状态, 不能用空格分隔。

**改变字段分隔符**

引起这个问题的原因是称为 **内部字段分隔符** (internal field separator, IFS)的特殊环境变量。环境变量 IFS 定义了 bash shell 用作字段分隔符的字符列表。

- 空格
- 制表符
- 换行符

如果 bash shell 在数据中遇到这几种字符的一种, 它就会认为您在正在列表中启动新的数据字段。当处理能包含空格的数据 ( 如文件名 )时, 就会产生干扰。

要解决问题, 可以在 shell 脚本中暂时更改环境变量 IFS 的值, 限制 bash shell 看作是字段分隔符的字符。然而, 这样做有点奇怪。

    IFS=$'\n'
在脚本中添加这条语句, 通知 bash shell 在数据值中忽略空格和制表符。将该语句应用于先前的脚本, 产生的结果如下 :   

    #!/bin/bash
    
    file="states"
    
    IFS=$'\n'
    for state in `cat $file`
    do
        echo "Visit beautiful $state"
    done
**`Tips : `** 现在 shell 可以在列表中使用包含空格的值了。记得将 IFS 设置回原值阿。

环境变量 IFS 有其他的很好的应用。如果想迭代文件中用冒号分隔的值 (如在文件 `/etc/passwd` 中)。需要做的就是将 IFS 的值设置为冒号 :  
    
    IFS=:
如果想指定多个 IFS 字符, 只需将它们在赋值进行中串连起来即可 : 
    
    IFS=$'\n':;"
该赋值使用了 换行符、冒号、分号 和 双引号 字符作为字段分隔符。对于使用的 IFS 字符如何解析数据没有限制。

**使用通配符读取目录**

可以使用 `for` 命令自动迭代文件的目录。为此, 必须在文件或路径名中使用通配符。这就使 shell 使用 **文件通配** (file globbing)。 文件通配是生成与指定通配符匹配的文件或路径名的过程。

当不知道目录中所有的文件名时, 这个功能非常好 :  

    #!/bin/bash
    
    for file in /home/rich/test/*
    do
        if [ -d "$file" ]
        then
            echo "$file is a directory"
        elif [ -f "$file" ]
        then
            echo "$file is a file"
        fi
    done
    
    $ ./testfile
    /home/rich/test/dir1 is a directory
    /home/rich/test/myscript is a file
    /home/rich/test/newdir is a directory

**`Tips : `** 在 Linux 中, 包含空格的路径和文件名是合法的。 要容纳它们, 应该使用双引号将变量 $file 包围起来。如果不这样做, 遇到包含空格的路径或文件名时就会出错。

[返回目录](#toc)

#### [C 式的 for 命令](id:cfor)

**C 语言中的 for 命令**

C 语言中的 `for` 命令有一种特定的方法指定一个变量, 即必需保持 true 值用于继续迭代的条件, 和一种每次迭代改变变量的方法。当特定的条件变为 false, `for` 循环结束。条件式使用标准的数学符号定义。

    for (( variable assignment ; condition ; iteration process ))

**`Tips : `** C 与 bash shell 的 `for` 方法的不同 :  

- 变量的赋值可以包含空格;
- 条件中的变量不以美元符号做前缀;
- 迭代处理式不使用 `expr` 命令格式 

        #!/bin/bash
        
        for (( i=1; i <= 10; i++ ))
        do 
            echo "The next number is $i"
        done
        
        $ ./testfile
        The next number is 1
        The next number is 2
        ...
        The next number is 10

**使用多个变量**

C 式的 `for` 命令也允许使用多个变量迭代。循环分别处理每个变量, 允许为每个变量定义不同的迭代过程。虽然可以使用多个变量, 但只可以在 `for` 循环中定义一个条件 : 

    #!/bin/bash
    
    for (( a=1,b=10; a <=10; a++, b-- ))
    do
        echo "$a - $b"
    done
    
    $ ./testfile
    1-10
    2-9
    ...
    10-1   
    

[返回目录](#toc)


#### [while 命令](id:while)

`while` 命令的格式是 : 

    while test command
    do 
        other commands
    done

常见示例 : 

    #!/bin/bash
    
    var1=10
    while [ $var1 -gt 0 ]
    do
        echo $var1
        var＝$[ $var1 - 1 ]
    done
    
**使用多条测试命令**

`while` 命令允许在 `while` 语句行定义多条 `test` 命令。只有最后一条测试命令的退出状态是用来决定循环是何时停止的。

    #!/bin/bash
    
    var1=10
    
    while echo $var1
        [ $var1 -ge 0 ]    //这里是大于等于的意思
    do
        echo "This is inside the loop"
        var1=$[ $var1 - 1 ]
    done
    
    $ ./testfile
    10
    This is inside the loop
    9
    ...
    This is inside the loop
    -1                        //当变量变为-1时被终止循环

#### until 命令

`until` 命令需要制定一个测试命令, 这条命令通常产生一个非 0 的退出状态。只要测试命令的退出状态非 0, bash shell 就会执行列在循环当中的命令。一旦测试条件返回 0 退出状态, 循环停止。

`until` 命令格式是 :   
    
    until test commands
    do
        other commands
    done
 下面是使用示例 :   
 
    #!/bin/bash
     
    var1=100
     
    until [ $var1 -eq 0 ]
    do
        echo $var1
        var1=$[ $var1 - 25 ]
    done

    $ ./testfile
    100
    75
    50
    25
一旦变量的值等于 0, `until` 命令就停止循环。在 `until` 命令中使用多条测试命令时。  
要注意的事情与 `while` 相同 :   

    #!/bin/bash
    
    var1=100
    
    until echo $var1
        [ $var1 -eq 0 ]
    do
        echo Inside the loop: $var1
        var1=$[ $var1 - 25 ]
    done
    
    $ ./testfile
    100
    Inside the loop: 100
    75
    ...
    Inside the loop: 25
    0

#### [嵌套循环](id:嵌套循环)

**嵌套循环** : 一条循环语句可以在循环中使用任何类型的命令, 包括其他循环命令。

这是一个 `for` 循环中嵌套另一个 `for` 循环的简单示例 : 

    #!/bin/bash
    
    for (( a = 1; a <= 3; a++ ))
    do
        echo "Starting loop $a:"
        for (( b = 1; b <= 3; b++ ))
        do
            echo " Inside loop: $b"
        done
    done
#### [文件数据的循环](id:文件数据的循环)

通常需要迭代存储在文件内部的项。这就需要结合两种介绍过的技术 :   

- 使用嵌套循环;
- 更改环境变量 IFS。

通过更改环境变量 IFS, 可以迫使 `for` 命令将文件中的每行作为单独的一项来处理, 即使数据包含空格。提取了文件中的个别行之后, 您可能还必须再循环以提取其包含的数据。

    #!/bin/bash
    
    IFS.OLD=$IFS
    IFS=$'\n'
    for entry in `cat /etc/passwd`
    do
        echo "Values in $entry -"
        IFS=:
        for value in $entry
        do
            echo " $value"
        done
    done

    $ ./testfile
    ...
    Values in daemon:*:1:1:System Services:/var/root:/usr/bin/false -
    daemon
    bc.sh
    for.sh
    passwd
    1
    1
    System Services
    /var/root
    /usr/bin/false
    ...
#### [控制循环](id:控制循环)

**break**

`break` 命令是处理过程中跳出循环的一种简单方法。可以使用 `break` 命令退出任何类型循环, 包括 `while` 循环和 `until` 循环。  

有几种情况可以使用 `break`  : 

1. **跳出循环**  
shell 执行 `break` 命令时, 它试图跳出当前正在处理的循环 : 

        #!/bin/bash
        
        for var1 in 1 2 3 4 5 6 7 8 9 10
        do
            if [ $var1 -eq 5 ]
            then
                break
            fi
            echo "Iteration number: $var1"
        done
        echo "The for loop is completed"
        
        $ ./testfile
        Iteration number: 1
        Iteration number: 2
        Iteration number: 3
        Iteration number: 4
        The for loop is completed
    
2. **跳出内循环**  
使用多循环时 `break` 命令自动终止您您所在的最里面的内部循环 :  

        #!/bin/bash
        
        for (( a = 1; a < 4; a++ ))
        do
            echo "Outer loop: $a"
            for (( b = 1; b < 100; b++ ))
            do 
                if [ $b -eq 5 ]
                then
                    break
                fi
                echo " Inner loop: $b"
            done
        done 
语句在执行内循环时, 跳出可以继续执行外循环。

3. **跳出外循环**  
可能有时处于内循环但需要停止外循环。 `break` 命令包括单独的命令行参数值 :   

        break n 
`n` 表明要跳出的循环级别。默认情况下, `n` 是 1, 代表跳出当前循环。如果将 n 设置为 2,`break` 命令将停止处循环的下一级循环 :   
        
        #!/bin/bash
        
        for (( a = 1; a < 4; a++ ))
        do
            echo "Outer loop: $a"
            for (( b = 1; b < 100; b++ ))
            do
                if [ $b -gt 4 ]
                then 
                    break 2
                fi
                echo " Inner loop: $b"
            done
        done
        
        $ ./testfile
        Outer loop: 1
         Inner loop: 1
         Inner loop: 2
         Inner loop: 3
         Inner loop: 4
当 shell 执行 `break` 命令时外循环停止。

**continue 命令**

`continue` 命令是一种提前停止循环内命令, 而不完全终止循环的方法。   
示例 :    

    #!/bin/bash
    
    for (( var1 = 1; var1 < 15; var1++ ))
    do
        if [ $var1 -gt 5 ] && [ $var1 -lt 10 ]
        then 
            continue
        fi
        echo "Iteration number: $var1"
    done
    
    $ ./testfile
    Iteration number: 1
    Iteration number: 2
    Iteration number: 3
    Iteration number: 4
    Iteration number: 5
    Iteration number: 10
    Iteration number: 11
    Iteration number: 12
    Iteration number: 13
    Iteration number: 14

`continue` 命令也允许使用命令行参数指定要继续的循环级别 :  
        
        continue n
其中, `n` 定义了要继续的循环级别。

    #!/bin/bash
    
    for (( a = 1; a <= 5; a++ ))
    do
        echo "Iteration $a : "
        for (( b = 1; b < 3; b++ ))
        do
            if [ $a -gt 2 ] && [ $a -lt 4 ]
            then 
                continue 2
            fi
            var3=$[ $a * $b ]
            echo " The result of $a * $b is $var3"
        done
    done
    
    $ ./testfile
    Iteration 1 :
     The result of 1 * 1 is 1
     The result of 1 * 2 is 2
    Iteration 2 :
     The result of 2 * 1 is 2
     The result of 2 * 2 is 4
    Iteration 3 :
    Iteration 4 :
     The result of 4 * 1 is 4
     The result of 4 * 2 is 8
    Iteration 5 :
     The result of 5 * 1 is 5
     The result of 5 * 2 is 10
使用 `continue` 命令停止处理循环内部的命令, 但是继续外部循环。注意在脚本输出中, 迭代的值为 3 时不处理任何循环语句, 因为 `continue` 命令停止了处理, 但是继续外循环处理。

#### [处理循环的输出](id:处理循环的输出)

可以在 shell 脚本中使用管道或者重定向循环输出结果。可以通过在 `done` 命令的末尾添加处理命令来做到这一点 :    

    #!/bin/bash
    
    for file in /home/rich/*
    do
        if [ -d "$file" ]
        then
            ehco "$file is a directory"
        elif
            echo "$file is a file"
        fi
    done > output.txt
    echo "The command is finished."
    
同样的技术也可用于将循环的输出管道传送给其他命令 :   
    
    #!/bin/bash
    
    for state in "North Dakota" Connecticut Illinois Alabama Tennessee
    do 
        echo "$state is the next place to go "
    done | sort
    echo "This completes our travels"
    
state 值没有在 `for` 命令列表中以任何特定的顺序中列出。 `for` 命令的输出结果被管道传送给 `sort` 命令, 它会改变 `for` 命令输出的顺序。




