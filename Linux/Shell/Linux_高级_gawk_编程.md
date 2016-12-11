### Linux_高级 gawk 编程

### 19 [高级 gawk 编程](id:toc)



- 19.1 [使用变量](#使用变量)
    - 19.1.1 [内置变量](#内置变量)
    - 19.1.2 [用户定义的变量](#用户定义的变量)
- 19.2 [使用数组](#使用数组)
    - 19.2.1 [定义数组变量](#定义数组变量)
    - 19.2.2 [在数组变量中递归](#在数组变量中递归)
    - 19.2.3 [删除数组变量](#删除数组变量)
- 19.3 [使用模式](#使用模式)
    - 19.3.1 [正则表达式](#正则表达式)
    - 19.3.2 [匹配操作符](#匹配操作符)
    - 19.3.3 [数学表达式](#数学表达式)
- 19.4 [结构化命令](#结构化命令)
    - 19.4.1 [if 语句](#if 语句)
    - 19.4.2 [while 语句](#while 语句)
    - 19.4.3 [do-while 语句](#do-while 语句)
    - 19.4.4 [for 语句](#for 语句)
- 19.5 [格式化打印](#格式化打印)
- 19.6 [内置函数](#内置函数)
    - 19.6.1 [数学函数](#数学函数)
    - 19.6.2 [字符串函数](#字符串函数)
    - 19.6.3 [时间函数](#时间函数)
- 19.7 [用户定义的函数](#用户定义的函数)
    - 19.7.1 [定义函数](#定义函数)
    - 19.7.2 [使用自己的函数](#使用自己的函数)
    - 19.7.3 [创建函数库](#创建函数库)


#### 19.1 [使用变量](id:使用变量)

gawk 编程语言支持两种不同的变量 : 

- 内置变量;
- 用户定义的变量。

##### 19.1.1 [内置变量](id:内置变量)

gawk 程序使用内置变量引用程序数据内的特定功能。

1. **字段和记录分隔符变量**  

	gawk 中的一种内置变量, 数据字段变量。数据字段变量允许使用美元符号和数据字段在记录中的数字位置引用数据记录中的单个数据字段。因此, 要引用 记录中的第一个数据字段, 则可以使用 `$1`。要引用第二个数据字段, 则使用 `$2` 变量, 依次类推。
	
	数据字段由字段分隔符号描述。默认情况下, 字段分隔符号是一个空白符号, 如空格或制表符。可以在命令行上使用 `-F` 参数 或者 在 gawk 程序中使用特定的 `FS` 内置变量更改字段分隔符号。
	
	**gawk 数据字段和记录变量**
	
	变量 | 描述
	----|---
	`FIELDWIDTHS` | 以空格分隔的数字列表, 用空格定义每个数据字段的精确宽度
	`FS` | 输入字段分隔符号(一段文本间分隔字段标记)
	`RS` | 输入记录分隔符号(为文本分段标记)
	`OFS` | 输出字段分隔符号
	`ORS` | 输出记录分隔符号
	
	`OFS` 默认为空格, 我们可以调整其应用请看下例 : 
	
	    $ cat data2
	    data11,data12,data13
        data21,data22,data23
        data31,data32,data33
        
		$ gawk 'BEGIN {FS=",";OFS="-"} {print $1,$2,$3}' data2
        data11-data12-data13
        data21-data22-data23
        data31-data32-data33
        
    `FIELDWIDTHS` 变量允许不使用字段分隔符读取记录。在某些应用程序中, 数据不使用字段分隔符号, 而是被放置在记录的特定列中。在这种情况下, 必须设置 `FIELDWIDTHS` 变量以匹配记录中的数据布局。
    
    设置 `FIELDWIDTHS` 变量后, gawk 忽略 `FS` 并基于提供的字段宽度计算数据字段 :  
    
        $ cat data4
        1005.3223423.37
        115-2.243245.00
        02423.1204560.1
        
        $  gawk 'BEGIN{FIELDWIDTHS="3 5 2 5"}{print $1,$2,$3,$4}' data4
        100 5.322 34 23.37
        115 -2.24 32 45.00
        024 23.12 04 560.1
        
    **`Tips : `** 设置 `FIELDWIDTHS` 变量后, 这些值必须是常量。此方法不支持长度为变量的数据字段。
    
    `RS` 和 `ORS` 变量定义 gawk 程序处理数据流中记录的方式。默认情况下, gawk 将 `RS` 和 `ORS` 变量设置为换行符。默认 `RS` 变量值表示输入数据流中文本的每个新行为一条新的记录。请将 `RS` 变量设置为空白字符串, 然后在数据流中的数据记录之间保留一个空行。gawk 程序会将每个空行解析为记录的分隔符。
    
        $ cat data5
        Riley Mullen
        123 Main Street
        Chicago, IL 60601
        (312)555-1234
        
        Frank Williams
        456 Oak Street
        Indianapolis, IN 46201
        (317)555-9876
        
        $ gawk 'BEGIN{FS="\n"; RS=""}{print $1,$4}' data5
        Riley Mullen (312)555-1234
        Frank Williams (317)555-9876
        
2. **数据变量**   

    gawk 中的其他变量 :   
    
    **更多 gawk 内置变量**
    
    变量 | 描述
    ----|---
    `ARGC` | 出现的命令行参数的个数
    `ARGIND` | 当前正在处理的文件在 ARGV 中的索引
    `ARGV` | 命令行参数数组
    `CONVFMT` | 数字的转换格式(参见 printf 语句) 默认值为 %.6g
    `ENVIRON` | 当前 shell 环境变量及其值的关联数组
    `ERRNO` | 当读取或关闭输入文件时发生错误时的系统错误
    `FILENAME` | 用于输入到 gawk 程序的数据文件的文件名
    `FNR` | 数据文件的当前记录号
    `IGNORECASE` | 如果设置非0, 则忽略 gawk 命令中使用的字符串的大小写
    `NF` | 已处理的输入记录的个数
    `OFMT` | 显示数字的输出格式, 默认为 %.6g
    `RLENGTH` | 匹配函数中匹配上的子字符串的长度
    `RSTART` | 匹配函数中匹配上的字符串的开始索引
    
    `ARGC` 变量表示命令行参数个数。`ARGV` 是一个数组, 索引从 `0` 开始, 索引为 `0`表示(gawk)命令, 索引为 `1` 时表示 gawk 后个第一个命令行参数
    
    **`Tips : `** 与 shell 变量不同, 在脚本中引用 gawk 变量时, 不需要在变量名称前加美元符号。
    
        $ gawk 'BEGIN{print ARGC,ARGV[0],ARGV[1]}' data2
        2 gawk data2

    `ENVIRON` 变量使用 **关联数组** (associative array) 检索 shell 环境变量。关联数组使用文本而不是使用数组作为数组的索引值。数组引用的就是 shell 环境变量的值 :  
    
        $ gawk 'BEGIN{print ENVIRON["HOME"];print ENVIRON["PATH"]}'
        /Users/xun
        /usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin

    `FNR`、`NF` 和 `NR` 变量在 gawk 程序中用于跟踪数据字段和记录。有时, 您可能不知道记录中具体有多少个数据字段。`NF` 变量允许指出记录中的最后一个数据字段而不需要知道它的位置 :  
    
        $ gawk 'BEGIN{FS=":";OFS=":"} {print $1,$NF}' data_test
        nobody:/usr/bin/false
        root:/bin/sh
        daemon:/usr/bin/false
        _uucp:/usr/sbin/uucico
        _taskgated:/usr/bin/false
    
    `NF` 变量包含数据文件中最后一个数据字段的数值。通过在前面加美元符号, 可以将其作为数据字段变量使用。
    
    `FNR` 变量包含当前数据文件中处理的记录数。`NR` 变量包含已经处理的记录的总数。也就是如果处理文件只有一个时, 两个值是一样的。
    
        $ gawk 'BEGIN{FS=","}{print $1,"FNR="FNR,"NR="NR}' data2 data2
        data11 FNR=1 NR=1
        data21 FNR=2 NR=2
        data31 FNR=3 NR=3
        data11 FNR=1 NR=4
        data21 FNR=2 NR=5
        data31 FNR=3 NR=6
    
[返回目录](#toc)


##### 19.1.2 [用户定义的变量](id:用户定义的变量)

gawk 用户自定义变量名称可以是多个字母、数字和下划线, 但不能以数字开头。还有就是, gawk 变量名称是区分大小写的。

1. **在脚本中赋值变量**  

    在 gawk 程序中赋值变量的方法类似于 shell 脚本, 即使用 **赋值语句** (assignment statement) : 
    
        $ gawk '
        > BEGIN {
        > testing="This is a test"
        > print testing
        > }'
        This is a test
        
    赋值语句还可以包含数学算法以处理数值 :  
    
        $ gawk 'BEGIN{x=4; x=x*2+3; print x}'
        11
    
    gawk 编程语言包括了处理数值的标准数学运算符。还可以包含求余的符号 (%) 和求幂的符号 (使用 ^ 或 % )。
    
2. **命令行中赋值变量**  

    还可以使用 gawk 命令行为 gawk 程序变量赋值。这允许您在常规代码以外设置值、实时更改值。
    
        $ gawk -f script1 n=2 data2
        data12
        data22
        data32
        
    此功能可以更改脚本的行为而不必改实际的脚本代码。例子展示了文件中的第二个数据字段。
    
    使用命令行参数定义变量的值存在一个问题。在设置变量时, 值不能在代码的 `BEGIN` 部分使用 :  
    
        $ cat script1
        BEGIN {print "The starting value is",n; FS=","}
        {print $n}
        
        $ gawk -f script1 n=2 data2
        The starting value is
        data12
        data22
        data32
        
    上面这个问题, 可以通过命令参数中使用 `-v` 来解决。它也允许指定在代码的 `BEGIN` 部分之前设置的变量。 `-v` 命令行参数必须放置在命令行上的脚本代码之前 :  
    
        $ gawk -v n=3 -f script1 data2
        The starting value is 3
        data13
        data23
        data33
    
[返回目录](#toc)


#### 19.2 [使用数组](id:使用数组)

gawk 编程语言使用 **关联数组** ( associative arrays ) 。关联数组由引用各个值的混杂字符串构成。每个索引字符串必须唯一, 且唯一标识分配给它的数据元素。

##### 19.2.1 [定义数组变量](id:定义数组变量)

可以使用标准的赋值语句定义数组变量。数组变量的赋值格式为 :  

    var[index] = element
其中, `var` 是变量名称, `index` 是关联数组的索引值, 而 `element` 是数据元素值。

在引用数组变量时, 必须包括索引值才能检索相应的数据元素值 :  

    $ gawk 'BEGIN{
    > capital["Illinois"] = "Springfield"
    > print capital["Illinois"]
    > }'
    Springfield
    
在引用数组变量时, 数据元素值出现了。这也适用于数字数据元素值 :  

    $ gawk 'BEGIN{
    > var[1] = 34
    > var[2] = 3
    > total = var[1] + var[2]
    > print total
    > }'
    37
    
[返回目录](#toc)


##### 19.2.2 [在数组变量中递归](id:在数组变量中递归)

如果需要在 gawk 的关联数组中递归, 可以使用特殊格式的 `for` 语句 :  

    for (var in array)
    {
        statements
    }

`for` 语句在各语句中循环, 每次都向变量 `var` 分配 `array` 关联数组中的下一个索引值。要点是要记住变量是索引的值, 而不是数据元素的值。通过将变量用作数组索引, 可以轻松地提取数据元素值 :  

    $ gawk 'BEGIN{
    > var["a"] = 1
    > var["g"] = 2
    > var["m"] = 3
    > for (test in var)
    > {
    >     print "Index:",test," - Value:", var[test]
    > }
    > }'
    Index: m  - Value: 3
    Index: a  - Value: 1
    Index: g  - Value: 2
    
**`Tips : `** 索引值不是以一定的顺序返回的, 但它们引用了正确的数据元素值。重要的是要知道, 不能指望以相同的顺序返回值, 但索引和数据值是匹配的。
    
[返回目录](#toc)


##### 19.2.3 [删除数组变量](id:删除数组变量)

从关联数组删除数组索引需要一个特殊的命令 :  

    delete array[index]
    
`delete` 命令从数组删除关联索引值和关联的数组元素值 :  

    $ gawk 'BEGIN {
    > var["a"] = 1
    > var["g"] = 2
    > for (test in var)
    > {
    >     print "Index:",test,"-Value:",var[test]
    > }
    > delete var["g"]
    > print "---"
    > for (test in var)
    >     print "Index:",test,"- Value:",var[test]
    > }'
    Index: a -Value: 1
    Index: g -Value: 2
    ---
    Index: a - Value: 1
    
我们可以看到在后面没有了已删除变量。
    
[返回目录](#toc)


#### 19.3 [使用模式](id:使用模式)

gawk 程序支持多种类型的模式匹配以过滤数据记录。`BEGIN` 和 `END` 关键字是特殊的模式, 它们分别在读取数据流数据之前和之后执行语句。类似地, 可以创建其他模式以便在匹配数据出现在数据流中时执行语句。

##### 19.3.1 [正则表达式](id:正则表达式)

可以使用 **基本正则表达式** ( Basic Regular Expression, BRE ), 或者 **扩展的正则表达式** ( Extended Regular Expression, ERE ) 来过滤程序脚本要应用到的数据流中的行。

在使用正则表达式时, 正则表达式必须出现在程序脚本的左括号之前 :  

    $ gawk 'BEGIN{FS=","} /11/{print $1}' data2
    data11
    
正则表达式 `/11/` 匹配任何数据字符包含字符串 11 的记录。
    
[返回目录](#toc)


##### 19.3.2 [匹配操作符](id:匹配操作符)

**匹配操作符** ( matching operator ) 允许限制正则表达式匹配记录中的特定数据字段。匹配操作符即 波浪号 (~)。需要指定匹配操作符, 数据字段变量, 还有要匹配的正则表达式 :  

    $1 ~ /^data/

`$1` 变量表示记录中的第一个数据字段。此表达式可过滤出第一个数据字段以文本 data 开头的记录。下面是在 gawk 程序脚本中使用它的示例 :  

    $ gawk 'BEGIN {FS=","} $2 ~ /^data2/{print $0}' data2
    data21,data22,data23
    
匹配操作符将第二个数据字段与正则表达式 `/^data2/` 进行比较, `/^data2/` 表示以文本 data2 开头的字符串。

这是一种在 gawk 程序脚本常用的, 也是非常好用的方法, 用于在数据文件中搜索特定的数据元素 :  

    $ gawk -F: '$1 ~ /root/{print $1, $NF}' /etc/passwd
    root /bin/sh
    _cvmsroot /usr/bin/false

还可以通过 `!` 符号否定正则表达式的匹配 : 

    $1 !~ /expression/
    
如果没有在记录中找到正则表达式, 则程序脚本将应用到记录数据上 :  

    $ gawk 'BEGIN{FS=","} $2 !~ /^data2/{print $1}' data2
    data11
    data31
    
gawk 程序脚本打印记录的第二个数据字段不以 `data2` 开头的第一个数据字段。
    
[返回目录](#toc)


##### 19.3.3 [数学表达式](id:数学表达式)

除了正则表达式, 还可以在匹配模式中使用数学表达式。当数据字段中包含数值时, 使用这一功能将非常得心应手。例如, 如果想显示所有属于要用户组 ( 组号为 0 ) 的系统用户, 则可以使用下面的脚本 :  

    $ gawk -F: '$4 == 0{print $1}' /etc/passwd
    root
    
脚本查看第 4 个数字字段包含值 0 的记录。

在 Linux 系统上, 有 5 个属于根用户组的用户账户。可以使用常规的数学比较表达式 :  

- `x == y` : 值 x 等于 y 
- `x <= y` : 值 x 小于等于 y
- `x < y` : 值 x 小于 y
- `x >= y` : 值 x 大于等于 y
- `x > y` : 值 x 大于 y

还可以使用带文本数据的表达式, 但使用时必须小心。不同于正则表达式, 表达式是精确匹配的。数据必须与模式精确匹配 : 

    $ gawk -F, '$1 == "data" {print $1}' data2
    $
    $ gawk -F, '$1 == "data11" {print $1}' data2
    data11
    
字段的值为 `data11` 时才有输出, 不能匹配 `data`

    
[返回目录](#toc)


#### 19.4 [结构化命令](id:结构化命令)

##### 19.4.1 [if 语句](id:if 语句)

gawk 编程语言支持 `if` 语句的标准 `if-then -else` 格式。必须为 `if` 语句定义一条以进行评估, 条件使圆括号括起来。如果条件值为 TRUE, 则执行紧跟在 `if` 语句后面的语句。如果条件为 FALSE, 则跳过语句。格式为 : 

    if (condition)
        statement1
    或者
    if (condition) statement1
    
下面是例展示 :  

    $ cat data6
    10
    5
    13
    50
    34
    $ gawk '{if ($1 > 20) print $1}' data6
    50
    34

gawk 的 `if` 语句还支持 `else` 分句, 允许在 `if` 语句失败时执行一个或多个语句。下面是使用 `else` 分句的示例 :  

    $ gawk '{
    > if ($1 > 20)
    > {
    >     x = $1 * 2
    >     print x
    > }else{
    >     x = $1 / 2
    >     print x
    > }}' data6
    5
    2.5
    6.5
    100
    68
    
    也可以写成 : 
    
    $ gawk '{if ($1 > 20) print $1 * 2; else print $1 / 2}' data6
    
[返回目录](#toc)


##### 19.4.2 [while 语句](id:while 语句)

`while` 语句为 gawk 程序提供了基本的循环功能。 `while` 语句的格式为 :  

    while (condition)
    {
        statements
    }
    
`while` 循环允许在数据集中递归, 检查停止递归的条件。当每个记录中有多个数据值, 且必须在计算中使用这些值时, `while` 语句非常有用 :  

    $ gawk '{
    > total = 0
    > i = 1
    > while (i < 4)
    > {
    >     total += $i
    >     i++
    > }
    > avg = total / 3
    > print "Average:",avg
    > }' data7
    Average: 128.333
    Average: 137.667
    Average: 176.667
    
[返回目录](#toc)


##### 19.4.3 [do-while 语句](id:do-while 语句)

`do-while` 语句类似于 `while` 语句, 但是在检查条件之前执行语句。`do-while` 语句的格式为 :  

    do
    {
        statements
    } while (condition)
    
此格式保证在评估条件之前语句至少被执行一次。这在需要评估条件之前执行语句时非常有用 :  

    $ gawk '{
    > total = 0
    > i = 1
    > do
    > {
    >     total += $i
    >     i++
    > } while (total < 150)
    > print total
    > }' data7
    250
    160
    315
    
[返回目录](#toc)


##### 19.4.4 [for 语句](id:for 语句)

`for` 语句是许多编程语言常用的循环方法。 gawk 编程语言支持 C 语言形式的 `for` 循环 :  

    for ( variable assignment; condition; iteration process )
    
这可以通过将若干个功能组合在一个语句中来简化循环 :  

    $ gawk '{
    > total = 0
    > for ( i = 1; i < 4; i++ )
    > {
    >     total += $i
    > }
    > avg = total / 3
    > print "Average:",avg
    > }' data7
    Average: 128.333
    Average: 137.667
    Average: 176.667
    
[返回目录](#toc)


#### 19.5 [格式化打印](id:格式化打印)

为解决格式打印, 可以用 `printf` 命令。如果熟悉 C 语言编程, 则 gawk 执行 `printf` 命令的方式与其相同, 允许对显示数据的方式进行详细说明。

`printf` 命令的格式为 :  

    printf "format string", var1, var2…
    
`format string` 是格式化输出的关键。它使用文本元素和 **格式化说明符** ( format specifier ) 说明了格式化后输出的显示方式。格式化说明透露是一种特殊的代码, 它指示显示什么样的变量类型及其显示方式。 gawk 程序将每个格式化说明符用作命令中列出的每个变量的点位符。第一个格式化说明符匹配列出的第一个变量, 第二个为第二个变量, 依次类推。

格式化说明符使用下面的格式 :  

    %[modifier]control-letter
    
其中, `control-letter` 是表示要显示的数据值类型的字符代码, 而 `modifier` 定义可选的格式化功能。

**格式化说明符控制字**

控制字 | 描述
----|---
`c` | 将数据显示为 ASCII 字符
`d` | 显示整数值
`i` | 显示整数值 (与 d 相同)
`e` | 用科学计数法显示数学
`f` | 显示浮点数值
`g` | 以科学计数法和浮点数的较短者显示数字
`o` | 显示八进制数值
`s` | 显示文本字符串
`x` | 显示十六进制数值
`X` | 显示十六进制数值, 但对 A 到 F 使用大写字母

除了控制字以外, 还可以使用 3 种修饰符更多地控制输出 : 

- `width` : 指定输出字段最小宽度的数值。如果输出比较短, `printf` 用空格填补空白, 使文本右对齐。如果输出的宽度hfv过指定的宽度, 则使用该值覆盖宽度。
- `prec` : 指定浮点数中十进制数小数点右侧位数的数值, 或者文本字符串中显示的最大字符数。
- `-`(减号) : 减号表示在将数据放到格式化空间时不使用右对齐, 而是使用左对齐。

可以使用 `printf` 命令帮助格式化输出以使其更加美观。

    $ gawk 'BEGIN{FS="\n"; RS=""} {printf "%16s %s\n", $1, $4}' data5
      Riley Mullen (312)555-1234
    Frank Williams (317)555-9876
    
通过添加 16 修饰符值, 我们可以强制第一个字符串占 16 个字符。
    
我们还可以用  `-` 让输出左对齐 : 

    $ gawk 'BEGIN{FS="\n"; RS=""} {printf "%-16s %s\n", $1, $4}' data5
    Riley Mullen     (312)555-1234
    Frank Williams   (317)555-9876
    
下面是对小数的输出 : 

    $ gawk '{
    total = 0
    for ( i = 1; i < 4; i++ )
    {
        total += $i
    }
    avg = total / 3
    printf "Average: %5.1f \n",avg
    }' data7
    Average: 128.3
    Average: 137.7
    Average: 176.7


**`Tips : `** 必须手动在 `printf` 命令的结尾添加换行符以强制换行。如果不强制换行, 则 `printf` 命令将继续使用同一行打印后面的内容。

这在将内容打印到一行时非常有用, 但是要使用单独的 `printf` 命令 :  

    $ gawk 'BEGIN{FS=","} {printf "%s ", $1} END{printf ""}' data2
    data11 data21 data31
    
[返回目录](#toc)


#### 19.6 [内置函数](id:内置函数)

gawk 编程语言提供了许多执行常规数学计算、字符串计算, 甚至是时间计算的内置函数。

##### 19.6.1 [数学函数](id:数学函数)

gawk 的内置函数 : 

**gawk 数学函数**

函数 | 描述
----|---
`atan2(x, y)` | x/y 的反正切, x 和 y 以弧度表示
`cos(x)` | x 的余弦, x 以弧度表示
`exp(x)` | x 的指数
`int(x)` | x 的整数部分, 截止到 0
`log(x)` | x 的自然对数
`rand()` | 大于 0 小于 1 的随机浮点值
`sin(x)` | x 的正弦, x 以弧度表示
`sqrt(x)` | x 的平方根
`srand(x)` | 用指定值计算的随机数

这里介绍下 `int()` 函数, 它没有四舍五入的功能, 它产生 0 和值之间最接近整数的值。

这表示, 5.6 的 `int()` 函数值将返回 5, 而 -5.6 的 `int()` 函数值将返回 -5。

`rand()` 函数用于创建随机数, 但需要使用一些技巧来获取有意义的值。 `rand()` 函数返回一个随机数, 但只返回 0 到 1 之间 ( 不包括 0 和 1 ) 的随机数。要取得更大的数值, 要放大返回的值。

常用的做法是使用 `rand()` 函数和 `int()` 函数生成一个算法来产生更大的整数 : 

    x = int(10 * rand())

这将返回 0-9 (包括 0 和 9 ) 之间的随机整数。只需要为应用程序将 10 替换为更高的限制值, 就可以得到更大的整数。

**`Tips : `** gawk 编程语言只能处理有限范围的数字值。如果超出范围, 则会得到一条错误信息 : 

    $ gawk 'BEGIN{x=exp(1000); print x}'
    inf
    
除了标准的数学函数, gawk 还提供了一些函数用于数据的位操作处理 : 

- `and(v1, v2)` : 执行 v1 和 v2 的与操作
- `compl(val)` : 执行对 val 的补位
- `lshift(val, count)` : 将值 val 向左移动 count 位
- `or(v1, v2)` : 执行 v1 和 v2 的或操作
- `rshift(val, count)` : 将值 val 向右移动 count 位
- `xor(v1, v2)` : 执行 v1 和 v2 的异或操作
    
[返回目录](#toc)


##### 19.6.2 [字符串函数](id:字符串函数)

**gawk 字符串函数**

函数 | 描述
----|---
`asort(s [, d])` | 基于数据元素值对数组 s 进行排序。索引值被替换为表示新排序顺序的一系列数字。另外, 可以指定将新排序后的数组存放在 d 数组中。
`asorti(s [, d])` | 基于索引值对数组 s 进行排序。产生的数组将索引值包含为数据元素值, 并带有表示排序的一系列数字。另外, 可以指定将新排序后的数组存放在 d 数组中。
`gensub(r, s, h [, t])` | 搜索变量 $0 , 或目标字符串 t (如果指定)以匹配正则表达式 r。如果 h 是以 g 或 G 开头的字符串, 则以 s 替换匹配的文本。如果 h 是一个数字, 则表示用 r 进行替换的次数。
`gsub(r, s [, t])` | 搜索变量 $0, 或目标字符串 t ( 如果指定 ) 以匹配正则表达式 r 。如果找到, 则全局替换字符串 s。
`index(s, t)` | 返回字符串 s 中字符串 t 的索引, 如果找不到, 则返回 0. 
`length([s])` | 返回字符串 s 的长度, 如果未指定, 则返回 $0 的长度。
`match(s, r, [, a])` | 在具有正则表达式 r 的地方返回字符串 s 的索引。如果指定数组 a, 则包含 s 与正则表达式匹配的部分。
`split(s, a [, r])` | 使用 FS 字符或正则表达式 (如果已指定) 将 s 分为数组 a。返回字段的个数。 
`sprintf(format, variables)` | 返回一个字符串, 该字符串与使用指定的 format 和 variables 的 printf 的输出类似
`sub(r, s [, t])` | 搜索变量 $0 或目标字符串 t, 以匹配正则表达式 r 。如果找到, 则将第一个找到的目标替换为字符串 s。
`substr(s, i [, n])` | 返回子字符串 s 的从索引 i 开始的第 n 个字符。如果未指定 n, 则使用其余的 s
`tolower(s)` | 将 s 中的所有字符转换为小写
`toupper(s)` | 将 s 中的所有字符转换为大写

有一些字符串函数从名称上就可以知道它的用途 :  

    $ gawk 'BEGIN{x = "testing";print toupper(x); print length(x)}'
    TESTING
    7
    
然而有一些字符串函数却非常复杂。

    $ gawk 'BEGIN{
    > var["a"] = 1
    > var["g"] = 2
    > var["m"] = 6
    > var["u"] = 4
    > asort(var, test)
    > for ( i in test )
    >     print "Index:", i, " - Value:", test[i]
    > }'
    Index: 1  - Value: 1
    Index: 2  - Value: 2
    Index: 3  - Value: 4
    Index: 4  - Value: 6

新的数组 test 包含原始数组新排序的数组元素, 但现在的索引值更改为表示适当顺序的数字值。`split()` 函数特别适合将数据字段放到数组中以供后续处理 :  

    $ gawk 'BEGIN {FS=","}{
    > split($0, var)
    > print var[1], var[3]
    > }' data2
    data11 data13
    data21 data23
    data31 data33
    
新的数组使用一系列数字作为数组的索引, 索引值从第一个数据字段的 1 开始。
    
[返回目录](#toc)


##### 19.6.3 [时间函数](id:时间函数)

**gawk 时间函数**

函数 | 描述
----|---
`mktime(datespec)` | 将以 YYYY MM DD HH MM SS 格式显示的日期[DST], 转换为时间戳值
`strftime(format [, timestamp])` | 将时间戳的当前时间, 或时间戳(如果提供), 使用 `date()` shell 函数格式转换为格式化的日和时间
`system()` | 返回当前时间的时间戳

时间函数经常用于处理需要进行比较的包含日期的日志文件。通过将日期的文本表示转换为纪元时间, 可以轻松实现对日期的比较。

    $ gawk 'BEGIN{
    > date = systime()
    > day = strftime("%A, %B %d, %Y", date)
    > print day
    > }'
    星期二, 八月 25, 2015    //根据系统不同, 有输出英文的
    
[返回目录](#toc)


#### 19.7 [用户定义的函数](id:用户定义的函数)

用户不仅可以使用 gawk 提供的内置函数, 还可以创建自已的函数, 以便在 gawk 程序中使用。

##### 19.7.1 [定义函数](id:定义函数)

要定义自己的函数, 必须使用 `function` 关键字 :  

    function name([variables])
    {
        statements
    }
    
函数名必须唯一标识函数。可在调用 gawk 程序时将一个或多个变量传递给函数 : 

    function printthird(limit)
    {
        print $3
        return int(limit * rand())
    }
    
函数还可以使用 `return` 语句返回值, `value` 可以是变量, 或计算值的公式。

在 gawk 程序中可以将函数返回的值分配给变量 : 

    x = myrand(100)
    
[返回目录](#toc)


##### 19.7.2 [使用自己的函数](id:使用自己的函数)

在定义函数时, 它必须单独出现在定义的任何代码部分 (包括 BEGIN 部分) 之前。

    $ gawk '
    > function myprint()
    > {
    > printf "%-16s - %s\n", $1, $4
    > }
    > BEGIN{FS="\n"; RS=""}
    > {
    >     myprint()
    > }' data5
    Riley Mullen     - (312)555-1234
    Frank Williams   - (317)555-9876
    
函数定义了 myprint() 函数, 它格式化记录中的第一个和第四个数据字段以进行打印。然后,  gawk 程序使用该函数显示数据文件中的数据。
    
[返回目录](#toc)


##### 19.7.3 [创建函数库](id:创建函数库)

gawk 提供了一种将函数合并到单个库文件中的方法, 可以将此文件用于所有的 gawk 编程工作。

首先, 需要创建包含所有 gawk 函数的文件 : 

    $ cat funclib
    function myprint()
    {
        print "%-16s - %s\n", $1, $4
    }
    
    function myrand(limit)
    {
        return int(limit * rand())
    }
    
    function printthird()
    {
        print $3
    }
    
funclib 文件包含 3 个函数定义。要使用它们, 需要使用 `-f` 命令行参数。不幸的是, 不能将 `-f` 命令行参数与内嵌的 gawk 脚本结合使用, 但可以在同一命令行上使用多个 `-f` 参数。

因此, 要使用库, 只需要创建包含 gawk 程序的文件, 然后在命令行上指定库文件和程序文件 : 

    $ cat script4
    BEGIN { FS="\n"; RS=""}
    {
        myprint()
    }
    
    $ gawk -f funclib -f script4 data2
    Riley Mullen     - (312)555-1234
    Frank Williams   - (317)555-9876
    
[返回目录](#toc)

