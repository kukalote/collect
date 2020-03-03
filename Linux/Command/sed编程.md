### Linux_高级sed编程

### 18 [高级 sed 编程](id:toc)

- 18.1 [多行命令](#多行命令)
    - 18.1.1 [next 命令](#next 命令)
    - 18.1.2 [多行删除命令](#多行删除命令)
    - 18.1.3 [多行打印命令](#多行打印命令)
- 18.2 [保留空间](#保留空间)
- 18.3 [否定命令](#否定命令)
    - 18.4 [更改命令流](#更改命令流)
    - 18.4.1 [分支](#分支)
    - 18.4.2 [测试](#测试)
- 18.5 [模式替换](#模式替换)
    - 18.5.1 [与号](#与号)
    - 18.5.2 [替换个别单词](#替换个别单词)
- 18.6 [在脚本中使用 sed](#在脚本中使用 sed)
    - 18.6.1 [使用包装器](#使用包装器)
    - 18.6.2 [重定向 sed 输出](#重定向 sed 输出)
- 18.7 [创建 sed 工具](#创建 sed 工具)
    - 18.7.1 [双倍行距](#双倍行距)
    - 18.7.2 [对可能有空行的文件使用双倍行距](#对可能有空行的文件使用双倍行距)
    - 18.7.3 [对文件中的行记数](#对文件中的行记数)
    - 18.7.4 [打印最后几行](#打印最后几行)
    - 18.7.5 [删除行](#删除行)
    - 18.7.6 [删除 HTML 标记](#删除 HTML 标记)



#### 18.1 [多行命令](id:多行命令)

sed 编辑器包括 3 个特殊的命令, 用于处理多行文本 :  

- `N` : 在数据流中添加下一行以创建用于处理的多行组。
- `D` : 删除多行组中的单个行。
- `P` : 打印多行组中的单个行。

##### 18.1.1 [next 命令](id:next 命令)

下面先介绍 `next` 命令单行版本的工作方式 :  

1. **单行 next 命令**  

    小写 `n` 命令告诉 `sed` 编辑器移动到数据流中文本的下一行, 而不是回到命令的开始。请记住, 通常 sed 编辑器在单个行上处理完所有定义的命令后才移动到数据流文本的下一行。单行 `next` 命令改变了这个流程。
    
        $ cat data
        This is the header line.
        
        This is the data line.
        
        This is the last line.
        $ sed '/header/{
        > n
        > d
        > }' data
        
    本例有一个五行的数据文件, 其中有两个空行, 我们要删除第一个空格行。匹配找到第一行后, 用 `n` 移动到下一行, 再用 `d` 命令删除。
    
2. **组合多行文本**  

    单行 `next` 命令移动数据流文本的下一行进入 sed 编辑器的处理空间 ( 称为 **模拟空间** )。 `next` 命令的多行版本 ( 使用大写 N ) 将文本的下一行添加到已经存在于模式空间的文本中。
    
    其效果相当于将数据流中的两个文本行合并, 添加到同一模式空间中。虽然文本行仍然由换行符分隔, 但 sed 编辑器已经将它们作为一个文本行处理了。
    
        $ cat data
        This is the header line.
        This is first data line. 
        This is second data line.
        This is the last line.
        
        $ sed '/first/{
        > N
        > s/\n/ /
        > }' data
        This is the header line.
        This is first data line. This is second data line.
        This is the last line.
        
    sed 编辑器先找到 first 的文本行。当找到该行时, 使用 `N` 命令将该行与下一行合并, 然后用替换命令 ( s ) 将换行符替换为一个空格。然后 sed 将文本文件的两行显示为一行。
    
    还可以用正则的模式来处理字符 : 
    
        $ sed '
        > N
        > s/\../. /
        > ' data
        This is the header line. This is first data line.
        This is second data linec. This is the last line.
        
    `N` 只是对当前及下一行起作用, 而下次起作用是跳过二行，从三行开始匹配的。并没有在完成的结果里面再递归进行正则匹配。这里的 `\..` 匹配的是 句点号及后面的换行符, 替换为 句点号和空格符。
    
[返回目录](#toc)


##### 18.1.2 [多行删除命令](id:多行删除命令)

sed 编辑器提供了多行删除命令 ( D ), 它只删除模式空间中的第一行。它将删除直至换行符的所有字符 ( 包括换行符 )。

    $ cat data
    The first meeting of the Linux System
    Administrator's group will be held on Tuesday.
    All System Administrators should attend this meeting.
    Thank you for your attendance.
    
    $ sed '
    > N
    > /System\nAdministrator/D
    > ' data
    Administrator's group will be held on Tuesday.
    All System Administrators should attend this meeting.
    
由 `N` 命令添加到模式空间的文本的第二行原封不动。这非常便于删除在找到数据字符串的行之前出现的文本行。
sed 编辑器不能对数据流中文本的最后 一行执行操作, 因为最后一行没有下一行。[解决方法看 18.3 否定命令](#resolve1)

    $ cat data
    This is the header line.
    
    This is a data line.
    
    This is the last line. 
    
    $ sed '/^$/{
    > N
    > /data/D
    > }' data
    This is the header line.
    This is a data line.
    
    This is the last line. 

sed 编辑器查找空行, 然后使用 `N` 命令将下一行文本添加到模式空间。如果模式空间的新加入内容包含单词 data, 则 `D` 命令删除模式空间中的前一行, 即空行。

[返回目录](#toc)


##### 18.1.3 [多行打印命令](id:多行打印命令)

多行打印命令 ( P ) 的原理相同 。它只打印多行模式空间中的第一行。这包括直至模式空间中换行字符的所有字符。
    
    $ cat data3
    The first meeting of the Linux System
    Administrator's group will be held on Tuesday.
    All System Administrators should attend this meeting.
    Thank you for your attendance.
    
    $ sed -n '
    > N
    > /System\nAdministrator/P
    > ' data
    The first meeting of the Linux System
    
当发生多行匹配时, `P` 命令只打印模式空间中的第一行。

`D` 命令有一个特有的功能, 它能强制 sed 编辑器返回到脚本开始处, 在相同的模式空间 ( 不从数据流中读取新文本行 ) 上重复执行命令。通过在命令脚本中包含 `N` 命令, 可以有效地一步跨过模式空间, 一起匹配多个行。

[返回目录](#toc)


#### 18.2 [保留空间](id:保留空间)

**模式空间** ( pattern space ) 是一个活动的缓冲区, 它在 sed 编辑器处理命令时保留被检查的文本。然而, 它不是存储文本的唯一可用空间。

sed 编辑器利用另一个称为 **保留空间** ( hold space ) 的缓冲区。在处理模式空间中的其他行时, 可以使用保留空间暂时保留文本行。

**sed 编辑器保留空间命令**

命令 | 描述 
----|---
`h` | 将模式空间复制到保留空间
`H` | 将模式空间追加到保留空间
`g` | 将保留空间复制到模式空间
`G` | 将保留空间追加到模式空间
`x` | 将模式空间和保留空间的内容交换

这些命令可以用于将文本从模式空间复制到保留空间, 其目的是腾空模式空间以载入其他字符串进行处理。

    $ cat data
    This is the header line.
    This is the first data line.
    This is the laste data line.
    This is the last line.
    
    $ sed -n '/first/{
    > h            //将匹配行放入保留区
    > p            //打印匹配行
    > n            //指向匹配行的下一行
    > p            //打印当前指向的行
    > g            //将保留区中行拿出来
    > p            //打印当前模式空间中的行
    > n            //指向下一行(此前已经指向第三行)
    > p            //输出当前指向的行 (第四行)
    > }' data
    This is the first data line.
    This is the laste data line.
    This is the first data line.
    This is the last line.

[返回目录](#toc)


#### 18.3 [否定命令](id:否定命令)

感叹号命令 ( ! ) 用于否定命令。它的意思是在通常命令被激活的地方不激活命令。

    $ sed -n '/header/!p' data
    This is the first data line.
    This is the laste data line.
    This is the last line.

不打印匹配项。

这是另一种用法 : sed 编辑器不能对数据流中文本的最后 一行执行操作, 因为最后一行没有下一行。可以[使用感叹号修改这个问题](id:resolve1) : 

    $ sed '
    > $!N                        // $ 表示末行, ! 否定, N 多行模式, 合起来就是 末行不进行多行模式的匹配
    > /System\nAdministrator/D
    > ' data3
    Administrator's group will be held on Tuesday.
    All System Administrators should attend this meeting.
    Thank you for your attendance.
    
> 此技术可用于反转数据流中文本行的顺序。要反转文本行出现在文本流中的顺序 ( 首先显示后一行, 最后显示第一行 ), 需要使用保留空间做一些工作。

> 需要使用的工作模式类似于下面的流程 :   

> 1. 将一行放到保留空间中; 
2. 将文本的下一行放到模式空间中;
3. 将保留空间追加到模式空间;
4. 将模式空间放到保留空间;
5. 重复第 2 步到第 4 步, 直至将所有行以相反的顺序放到保留空间;
6. 检索行并打印它们。

> 首先, 我们不希望在运行时打印行, 所以要使用 `-n`选项; 

> 其次, 将保留空间追加到处理文本的第一行, 可以通过感叹号命令轻易解决这个问题 :  `1!G`

> 再次, 是将新的模式空间 ( 追加了相反顺序行的文本行 ) 放到保留空间。可以使用 `h` 命令

> 最后, 在模式空间中以相反的顺序获取整个数据流时, 需要做的只是打印出来。当到达数据流的最后一行时, 即在模式空间中获取了整个数据流。要打印结果只需要使用下面的命令 :  `$p`

    $ cat data
    This is the header line.
    This is the first data line.
    This is the laste data line.
    This is the last line.
    
    $ sed -n '{
    > 1!G
    > h
    > $p
    > }' data
    This is the last line.
    This is the laste data line.
    This is the first data line.
    This is the header line.

> 大概过程如下 :  
 
模式区默认 | `1!G` | `h` | `$p`
----|---|---|---
模式区 : 1 | 模式区 : 1    保留区 : 无  |   模式区 : 1    保留区 : 1  | 无
模式区 : 2 | 模式区 : 2,1    保留区 : 1   |   模式区 : 2,1    保留区 : 2,1  | 无
模式区 : 3 | 模式区 : 3,2,1    保留区 : 2,1   |   模式区 : 3,2,1    保留区 : 3,2,1  | 无
模式区 : 4 | 模式区 : 4,3,2,1    保留区 : 3,2,1   |   模式区 : 4,3,2,1    保留区 : 4,3,2,1  | 打印(最后一行打印)

[返回目录](#toc)


#### 18.4 [更改命令流](id:更改命令流)

通常, sed 编辑器从脚本的开头开始执行命令, 并一直执行到脚本的结尾 ( `D` 命令例外, 它强制 sed 编辑器返回到脚本的开头而不读取新的文本行 )。 sed 编辑器提供了改变脚本命令流的方法, 产生一种类似于结构化编程环境的效果。

##### 18.4.1 [分支](id:分支)

sed 编辑器提供了一种否定整个命令小节 ( 基于地址、地址模式或地址范围 ) 的执行的方法。这允许我们只在数据流的特定子集上执行一组命令。分支命令的格式为 :  
    
    [address]b [label]

`address` 参数决定哪个行或哪些行激发分支命令。`label` 参数定义于何处分支。如果 `label` 参数不存在, 则分支命令将继续执行到脚本的结尾。

    $ sed '{
    > 2,3b
    > s/This is/Is this/
    > s/line./test?/
    > }' data
    Is this the header test?
    This is the first data line.
    This is the laste data line.
    Is this the last test?
    
分支命令跳过了数据流中的第二行和第三行的替换命令。

除了到达脚本的最后, 还可以定义一个想要分支命令转到的标签。标签以冒号开头, 最长为 7 个字符 :  

    :label2
    
要指定标签, 只需要将它添加到 `b` 命令后面。使用标签可以跳过与分支地址匹配的命令, 而仍然执行脚本中的其他命令 :  

    $ sed '{
    > /first/b jump1
    > s/ is/ might be/
    > s/line/test/
    > :jump1
    > s/data/text/
    > }' data
    This might be the header test.
    This is the first text line.
    This might be the laste text test.
    This might be the last test.
    
当第二行时, 因为 jump1 标记跳过了前两条替换指令。不过第三行仍然要执行标签后的替换命令 ( 第三条替换命令 )。

如果行匹配分支模式, 则 sed 编辑器分支到分支标签行。因此, 只执行最后一个替换命令。

    echo "This, is, a, test, to, remove commas." | sed -n '{
    > :start
    > s/,//1p
    > /,/b start        // 为标签跳转添加条件, 否则要进入死循环 ( 如 "> b start")
    > }'
    This is, a, test, to, remove commas.
    This is a, test, to, remove commas.
    This is a test, to, remove commas.
    This is a test to, remove commas.
    This is a test to remove commas.
    
[返回目录](#toc)


##### 18.4.2 [测试](id:测试)

测试命令 ( t ) 也用于更改 sed 编辑器脚本的流程。测试命令不是基于地址跳转的标签, 而是基于替换命令的结果跳转的标签。

如果替换命令成功匹配并替换了一个模式, 则测试命令分支到指定的标签。如果替换命令不匹配指定的模式, 则测试命令不分支。

测试命令使用与分支命令相同的格式 :  

    [address]t [label]

与分支命令类似, 如果不指定标签, 则测试成功时 sed 将分支到脚本的最后。

测试命令在数据流的文本上执行基本的 `if-then` 语句提供了一种简单的方式。例如, 如果在另一个替换成功时不需要再执行替换, 则测试登记可以帮忙 :  

    $ sed '{
    > s/first/starting/
    > t
    > s/line/test/
    > }' data
    This is the header test.
    This is the starting data line.
    This is the laste data test.
    This is the last test.

第一个替换命令查找模式文本 first。如果行上能够匹配模式, 则替换该文本, 而测试命令跳过后续的替换命令。如果第一个替换命令未能匹配, 则执行第二个替换命令。

使用测试命令可以改进分支命令循环 : 

    $ echo "This, is, a, test, to, remove commas." | sed -n '{
    :start
    s/,//1p
    t start
    }'
    This is, a, test, to, remove commas.
    This is a, test, to, remove commas.
    This is a test, to, remove commas.
    This is a test to, remove commas.
    This is a test to remove commas.
    
当没有可替换的内容时, 测试命令将不再分支并继续执行剩下的部分。


[返回目录](#toc)


#### 18.5 [模式替换](id:模式替换)

替换字符串不能以通配符的值替换匹配的单词。

##### 18.5.1 [与号](id:与号)

与号 ( & ) 用于表示替换命令中的匹配模式。无论什么文本匹配上定义的模式, 都可以使用与号在替换模式中调用它。这允许处理与定义的模式匹配的任何单词 :  

    $ echo "The cat sleeps in his hat." | sed 's/.at/"&"/g'
    The "cat" sleeps in his "hat".

当模式匹配 cat, cat 出现在替换的单词中。当它匹配 hat 时, hat 出现在替换的单词中。

##### 18.5.2 [替换个别单词](id:替换个别单词)  

sed 编辑器使用圆括号定义替换模式中的字符串元素。然后在替换模式中使用特定的符号来引用子字符串元素。替换字符由反斜杠和数字组成。数字表示子字符串元素的位置。 sed 编辑器将第一个元素分配为字符 `\1` , 第二个元素分配成字符 `\2`, 依次类推。

**`Tips : `** 在替换命令中使用圆括号时, 必须使用转义字符来表示它们是分组字符, 而不是普通的圆括号。这不于其他特殊字符的转义。

    $ echo "The System Administrator manual" | sed '
    > s/\(System\) Administrator/\1 User/'
    The System User manual

上例只替换了以 System 为条件的 Administrator, System 并未改变。

如果需要以单个单词 ( 短语的子集 ) 替换一个短语, 而子字符串又正好使用了通配符, 则最好使用子字符串元素 :  

    $ echo "That furry cat is pretty" | sed 's/furry \(.at\)/\1/'
    That cat is pretty
    
要将文本插入到两个或者多个字符串元素 :  

    $ echo "134567" | sed '{
    > :start
    > s/\(.*[0-9]\)\([0-9]\{3\}\)/\1,\2/
    > t start
    > }'
    134,567

这个正则表达式的解释 :    
`\(.*[0-9]\)` 任何以数字结尾的字符串
`\([0-9]\{3\}\)` 后三位为数字的字符串

[返回目录](#toc)


#### 18.6 [在脚本中使用 sed](id:在脚本中使用 sed)

##### 18.6.1 [使用包装器](id:使用包装器)

与其在每次需要时重新输入整个脚本, 不如将 sed 编辑器命令放到 shell 脚本 **包装器** ( wrapper ) 中。包装器就像是 sed 编辑器脚本和命令行之间的媒介。

在 shell 脚本时, 可以在 sed 编辑器脚本中使用常规的 shell 变量和参数。

    #!/bin/bash
    
    sed -n '{
    1!G
    h
    $p
    }' "$1"
    
    $ ./testfile data
    
[返回目录](#toc)


##### 18.6.2 [重定向 sed 输出](id:重定向 sed 输出)

默认情况下, sed 编辑器将脚本的结果输出到 STDOUT。可以在 shell 脚本中使用所有标准方法来重定向 sed 编辑器的输出。

    #!/bin/bash
    
    factorial=1
    counter=1
    number=$1
    
    while [ $counter -le $number ]
    do
        factorial=$[ $factorial * $counter ]
        counter=$[ $counter + 1 ]
    done
    
    result=`echo $factorial | sed '{
    :start
    s/\(.*[0-9]\)\([0-9]\{3\}\)/\1,\2/
    t start
    }'`
    
    echo "The result is $result"
    
    $ ./testfile 20
    The result is 2,432,902,008,176,640,000
    
[返回目录](#toc)


#### 18.7 [创建 sed 工具](id:创建 sed 工具)

##### 18.7.1 [双倍行距](id:双倍行距)

首先来看一个在文本文件的各行之间添加空行的例子 :  

    $ cat data
    This is the header line.
    This is the first data line.
    This is the laste data line.
    This is the last line.
    
    $ sed 'G' data
    This is the header line.
    
    This is the first data line.
    
    This is the laste data line.
    
    This is the last line.

`G` 命令只是单纯地将保留空间的内容追加到模式空间的内容。 在启动 sed 编辑器时, 保留空间包含一个空行。通过将其追加到现有的行, 可以在现有行后创建一个空行。如果想去掉行尾产生那一个空行, 可用 : 

    sed '$!G' data
    
[返回目录](#toc)


##### 18.7.2 [对可能有空行的文件使用双倍行距](id:对可能有空行的文件使用双倍行距)

如文件已有一个空行, 而想让所有的行都使用双倍行距。

    $ cat data
    This is the header line.
    This is the first data line.
    
    This is the laste data line.
    This is the last line.
    
    $ sed '/^$/d;$!G' data
    This is the header line.
    
    This is the first data line.
    
    This is the laste data line.
    
    This is the last line.
    
[返回目录](#toc)


##### 18.7.3 [对文件中的行记数](id:对文件中的行记数)

等号命令用于显示行号, 得到等于号命令的输出结果后, 可以通过管道将其输出到另一个使用 `N` 命令结合两个行的 sed 编辑器脚本。

    $  sed '=' data | sed 'N; s/\n/  /'
    1  This is the header line.
    2  This is the first data line.
    3  This is the laste data line.
    4  This is the last line.
    
[返回目录](#toc)


##### 18.7.4 [打印最后几行](id:打印最后几行)

显示最后一行很容易 : 

    $ sed -n '$p' data
    
如何显示最后几行? 答案是创建 **滚动窗口** ( rolling window )。 

滚动窗口是一种用于检查模式空间的文本行块的常用方法, 通常与 `N` 命令组合使用。`N` 命令将文本的下一行追加到已经存在于模式空间中的文本后面。假如在模式空间中有一个 10 行的文本块, 可以通过美元符号判断是否处于数据流的最末。如果不是最末, 则可以继续将更多的行添加到模式空间, 但删除原始的行 ( 使用 `D` 命令, 删除模式空间的第一行 )。

通过 `N` 命令和 `D` 命令的循环, 可以向模式空间中的行块添加新行, 同时删除旧行。分支命令非常适合用于这个循环。要结束循环, 只需要找到最后一行并使用 `q` 命令退出。

    $ sed '{
    :start
    $q
    N
    3,$D
    b start
    }' data
    
脚本第一个命令是标记的跳转标签, 第二个命令是到行尾则退出, 第三个是将下一行追加到模式空间当前的行。第四个命令是匹配 3 行 到 最后一行 时删除前面追加过的 第一行。

> 因为是循环, 所以 `N` 命令可以将所有行都连成一行 ( 换行符只是作为字符出现在模式空间 )。  
> 这里有一点要注意如果 `1,$D` 输出是全部内容, 因为删除第 1 行之前的行是没有意义的, 因为第 1 行前面没有行。循环中每次都删除排在第一行数据的前一行, 所以每次都失效。  
> 而这里第 4 条命令 `3,$D`, 比如说文本有 4 行数据, 3 到 4 行才执行 `D` 命令, 相当于只删除 1 次, 因为第 4 行时, 已经 `$q` 提前退出了。

循环次数 | 退出操作 | N 命令后模式空间 | D 命令操作
----|---|---|---
1 | 无效匹配 | 1,2 | 无效匹配
2 | 无效匹配 | 1,2,3 | 无效匹配
3 | 无效匹配 | 1,2,3,4 | 2,3,4
4 | 退出 | -- | --

[返回目录](#toc)


##### 18.7.5 [删除行](id:删除行)

从数据流中删除所有空行非常容易, 但有选择地删除空行就需要技巧了。

1. **删除连续空行**  

删除连续空行的最简单方法是使用地址范围查看数据流, 该范围包括一个非空行和一个空行。如果 sed 编辑器遇到这个范围, 则不会删除行。然而, 对于不匹配该范围的行 ( 行中的两个或多个空行 ), 则删除这些行。

如下面的脚本所求 :  

    /./,/^$/!d
范围是 `/./` 到 `/^$/`。 范围中的起始地址匹配任何包含至少一个字符的行。范围中的结束地址匹配一个空行。位于此范围的行不会被删除。

    $ cat data
    This is the header line.
    
    This is the first data line.
    
    
    This is the laste data line.
    
    
    This is the last line.
    $ sed '/./,/^$/!d' data
    This is the header line.
    
    This is the first data line.
    
    This is the laste data line.
    
    This is the last line.
    
2. **删除开头的空行**  

    从数据流的开头删除空行并不是一个困难的任务 :  
    
        /./,$!d
    
    此脚本使用地址范围确定要删除的行。地址范围以包含一个字符的行开始, 一直到数据流的结尾结束。

        $ cat data
        
        This is the header line.
        
        This is the first data line.
        $ sed '/./,/^$/!d' data
        This is the header line.
        
        This is the first data line.
        
3. **删除结尾的空行**  

    删除结尾的行并不那么容易了, 看下面的例子 :  
    
        sed '{
        :start
        /^\n*$/{$d; N; b  start}
        }'

    注意, 常规脚本的大括号之间还有大括号。这允许在全部的命令脚本之中对命令进行组合。组合的命令应用于特定的地址模式。地址模式匹配任何只包含换行符的行。当找到一行时, 如果是最后一行, 则删除命令将其删除。如果不是最后一行, 则 `N` 命令将追加其下一行, 然后分支命令循环回到开始处。
        
        $ cat data
        This is the header line.
        This is the first data line.
        
        
        $ sed '{
        > :start
        > /^\n*$/{$d; N; b start;}
        > }' data
        This is the header line.
        This is the first data line.
    
    此语句 Mac 系统并不支持。
    
[返回目录](#toc)


##### 18.7.6 [删除 HTML 标记](id:删除 HTML 标记)

遇到 html 页面, 我们想要将 html 标签过滤, 并且将多余的行去除 :  

    $ sed 's/<[^>]*>//g;/^$/d' index.html
    
[返回目录](#toc)







    
    
        


 





