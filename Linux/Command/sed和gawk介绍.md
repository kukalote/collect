### Linux_sed和gawk介绍

### 16 [sed 和 gawk 介绍](id:toc)

- 16.1 [文本处理](#文本处理)
    - 16.1.1 [sed 编辑器](#sed 编辑器)
    - 16.1.2 [gawk 程序](#gawk 程序)
- 16.2 [sed 编辑器基础知识](#sed 编辑器基础知识)
    - 16.2.1 [更多替换选项](#更多替换选项)
    - 16.2.2 [使用地址](#使用地址)
    - 16.2.3 [删除行](#删除行)
    - 16.2.4 [插入和附加文本](#插入和附加文本)
    - 16.2.5 [更改行](#更改行)
    - 16.2.6 [变换命令](#变换命令)
    - 16.2.7 [打印命令温习](#打印命令温习)



#### 16.1 [文本处理](id:文本处理)

##### 16.1.1 [sed 编辑器](id:sed 编辑器)

sed 编辑器称为 **流编辑器** ( stream editor ), 与普通的交互式文本编辑器相应。流编辑器将根据在编辑大处理数据之前事先提供的规则集编辑数据流。

sed 编辑器可以根据输入命令行的命令或者存储在命令文本文件中的命令处理数据。它每次从输入读取一行数据, 将该数据与所提供的编辑器命令进行匹配, 根据命令修改数据流中的数据, 然后将新数据输出到 STDOUT。在流编辑器将全部命令和一行数据匹配完之后, 它读取下一行数据, 并重复上述过程。处理完数据流中的全部数据行之后, 流编辑器停止。

由于是按顺序逐行应用命令, 所以 sed 编辑器进行编辑仅需浏览数据流一次。这使得 sed 编辑器比交互式编辑器要快很多, 因而可以动态地快速修改文件中的数据。

使用 sed 命令的格式是 :  

	sed options script file
	
`options` 参数允许自定义 sed 命令的行为, 它包含的选项如下表 : 

**sed 命令选项**

选项 | 描述
----|---
`-e script` | 将脚本中指定的命令添加到处理输入时执行的命令中
`-f file` | 将文件中指定的命令添加到处理输入时执行的命令中
`-n` | 不需要为每个命令产生输出, 但要等待打印命令

`script` 为单个命令时可以不使用 `-e`, 但如果需要更多命令, 就必须使用 `-e` 选项。

1. **在命令行中定义编辑器命令**  

	默认情况下, sed 编辑器将指定的命令应用于 STDIN 输入流 : 
	
		$ echo "This is a test" | sed 's/test/big test/'	//用 big test 替换 test
		This is a big test
	在编辑文件时, 也能逐行修改, 在 sed 编辑器结束处理文件之前, 就可以看到结果 : 
	
		$ cat data
		The quick brown fox jumps over the lazy dog.
		The quick brown fox jumps over the lazy dog.
		The quick brown fox jumps over the lazy dog.
		
		$ sed 's/dog/cat/' data
		The quick brown fox jumps over the lazy cat.
		The quick brown fox jumps over the lazy cat.
		The quick brown fox jumps over the lazy cat.
		
	**`Tips : `** sed 编辑器并不修改文件中的数据。它只是将修改后的文本发送到 STDOUT。
	
2. **在命令行中使用多个编辑器命令**  

		$ sed -e 's/brown/green/; s/dog/cat/' data
		The quick green fox jumps over the lazy cat.
		The quick green fox jumps over the lazy cat.
		The quick green fox jumps over the lazy cat.
	
	**`Tips : `** 两个命令将同时应用于文件中的每一行数据。命令必须用分号隔开, 且在命令结尾和分号之间不能有任何空格。
	
	在 bash shell 中, 可以使用次提示符, 而不是分号分隔命令。只需输入前单引号打开脚本, bash 将继续提示输入更多的命令, 直到输入后单引号 :  
	
		$ sed -e '
		> s/brown/green/
		> s/green/red/    //此处命令是按顺序执行, 可以将前面内容覆盖
		> s/dog/cat/' data
		The quick red fox jumps over the lazy cat.
        The quick red fox jumps over the lazy cat.
        The quick red fox jumps over the lazy cat.
        
3. **从文件读取编辑器命令**  

    如果有太多需要 sed 处理的命令, 我们可以将其保存到一个文件中, 然后通过 `-f` 选项调取它 :  
    
        $ cat script
        s/brown/green/
        s/green/red/
        s/dog/cat/
        
        $ sed -f script data
    
    这样不需要在每个命令后放一个分号。
    
[返回目录](#toc)

##### 16.1.2 [gawk 程序](id:gawk 程序)

如果您需要更高级的工具来处理文件中的数据, 一个能够提供类似于编程环境的工具, 它允许修改和重新组织文件中的数据。

gawk 程序在流编辑方面比 sed 编辑器更先进的是 :  它提供了一种编程语言而不仅仅是编辑器命令行。在编程语言内部, 可以 :  

- 定义要保存数据的变量;
- 使用算术和字符串操作符对数据进行运算;
- 使用结构化编程概念, 例如 `if-then` 语句和循环, 将逻辑添加到数据处理过程;
- 通过从数据文件内抽取数据元素以及按照其他顺序或格式对它们重定位, 生成带格式的报告。

gawk 程序的报告生成能力常用于从大文本文件中抽取数据元素, 并将日志文件中筛选要查看的元素, 其格式化为易读的报告。

1. **gawk 命令格式**  

    gawk 程序的基本格式是 : 
    
        gawk options program file
        
    **gawk 选项** 
    
    选项 | 描述
    ----|---
    `-F fs` | 指定描绘一行中数据字段的文件分隔符
    `-f file` | 指定读取程序的文件名
    `-v var=value` | 定义 gawk 程序中使用的变量和默认值
    `-mf N` | 指定数据文件中要处理的字段的最大数目
    `-mr N` | 指定数据文件中的最大记录大小
    `-W keyword` | 指定 gawk 的兼容模式或警告级别

2. **自命令行读取程序脚本**  

    gawk 程序脚本由左大括号和右大括号定义。脚本命令必须放置在两个大括号之间。由于 gawk 命令行假定脚本是单文本字符串, 所以必须将脚本包括在单引号内。下面是在行命令行上指定的一个简单的 gawk 程序脚本 :  
    
        $ gawk '{print "Hello John!"}'
        test
        Hello John!
        
    同 sed 编辑器完全一样, gawk 程序对数据流中可用的第一行文本执行程序脚本。由于 程序脚本设为显示固定的文本字符串, 因此无论在数据流中输入什么, 都将得到相同的输出。
    要结束 gawk 程序, 必须信号说明数据流已经结束。bash shell 提供了生成 End-of-File(EOF) 字符的组合键。在 bash 中, Ctrl + D 组合键生成 EOF 字符。使用这一组合键结束 gawk 程序, 返回命令行界面提示符。
    
3. **使用数据字段变量**  

    gawk 主要功能之一是其处理文本文件中数据的能力。它通过自动将变量分配到每行中的每个数据元素实现这一功能。默认情况下, gawk 将下面的变量分配给在文本行中检测到的每个数据字段 :  
    
    - `$0` 表示整行文本;
    - `$1` 表示文本行中的第一个数据字段;
    - `$2` 表示文本行中的第二个数据字段;
    - `$n` 表示文本行中的第 n 个数据字段。

    各数据字段依据文本行中的 **字段分隔符** (field separation character) 确定。 gawk 读取一行文本时, 使用定义的字段分隔符描述各数据字段。gawk 的默认字段分隔符是任意空白字符 ( 如制表符或空格符 )。
    
        $ cat data
        one line of test.
        two line of test
        three line of test
        
        $ gawk '{print $1}' data
        one
        two
        three
    
    还可以指定分隔符, 使用 `-F` 选项 : 
    
        $ gawk -F: '{print $1}' /etc/passwd

4. **在程序脚本中使用多个命令**  

    gawk 编程语言允许将命令组合成普通程序。要在命令行指定的程序脚本中使用多个命令, 只需在各命令之间加一个分号 :  
    
        $ echo "My name is Rich" | gawk '{$4="Dave"; print $0}'
        My name is Dave
        
    gawk 通过上面方式替换了第四个数据字段。  

5. **从文件读取程序**  

    gawk 编辑器允许您将程序保存在文件中并在命令行引用它们 :  
    
        $ cat script
        { print $5 "'s userid is " $1 }
        
        $ gawk -F: -f script /etc/passwd
        Unprivileged Users userid is nobody
        System Administrators userid is root
        System Servicess userid is daemon
        
    如果要使用多个命令, 只需在每行放置一个命令, 不需要使用分号 : 
    
        $ cat script
        {
        text="'s userid is "
        print $5 text $1
        }

6. **在处理数据时运行脚本**  

    gawk 程序还允许指定运行脚本的时间。默认情况下, gawk 从输入读取一行文本, 然后执行程序脚本处理文本行中的数据。但有时, 可能需要在处理数据之前运行脚本, 例如创建报告的标题部分。为达到这一目的, 要使用 BEGIN 关键字。
    当报告结束时也可以用 END 关键字, 来输出结束标语 : 
    
        gawk 'BEGIN {print "Hello World"} {print $0} END {print "Byebye"}'
    
    执行命令时, 自行输出开始部分, 按下 `Ctrl-D` 自动发送结束标语。
    
    我们可以创建一个简单的脚本 : 
    
        $ cat script
        BEGIN {
        print "The latest list of users and shells"
        print "Userid    Shell"
        print "-------    -------"
        FS=":"
        }
        
        {
        print $1 "        " $7
        }
        
        END {
        print "This concludes the listing"
        }
        
        $ gawk -f script /etc/passwd
        The latest list of users and shells
        Userid    Shell
        -------    -------
        nobody     /usr/bin/false
        root       /bin/sh
        This concludes the listing

[返回目录](#toc)

#### 16.2 [sed 编辑器基础知识](id:sed 编辑器基础知识)

成功使用 sed 编辑器的关键字是要了解其大量的命令和格式, 这些命令和格式有助于自定义文本编辑。您可以将它们集成在自己的脚本中, 由此开始使用 sed 编辑器。

##### 16.2.1 [更多替换选项](id:更多替换选项)

1. **替换标记**  

    对于替换命令替换文本字符串匹配模式的方法, 有一个警告 :  

        $ cat data
        This is a test of the test script.
        This is the second test of the test script.
        
        $ sed 's/test/trial/' data
        This is a trial of the test script.
        This is the second trial of the test script.

    替换命令在替换多个文本行中的文本时效果不错, 但默认情况下仅替换各行中首次出现的文本。要使替换命令继续替换之后出现的文本, 则必须使用 **替换标记** (substitution flag) :  
    
        s/pattern/replacement/flags
        
    可用的替换标记有 4 种 :  
    
    - `数字` :  表示新文本替换的模式;
    - `g` : 表示用新文本替换现有文本的全部实例;
    - `p` : 表示打印原始行的内容;
    - `w file` : 将替换的结果写入文件。

    第一种类型替换中, 可指定 sed 编辑器用新文本替换匹配模式的哪一个实例 :  
    
        $ sed 's/test/trial/2' data
        This is a test of the trial script. 

    指定 2 作为替换标记的结果是,  sed 编辑器仅替换每一行中第二次出现的模式。
    
    `g` 替换标记能够替换文本中模式的每一个实例 :  
    
        $ sed 's/test/trial/g' data
        This is a trial of the trial script.
        
    `p` 替换标记会打印包含替换命令中匹配模式的哪一行。经常和 `-n` 选项一起使用 :  
    
        $ cat data
        This is a test line.
        This is a different line.
        
        $ sed -n 's/test/trial/p' data
        This is a trial line
        
    `-n` 选项禁止 sed 编辑器的输出。然而, `p` 替换标记会输出所有已修改的行。二者结合使用仅生成替换命令已更改的那些行的输出。
    
    `w` 替换标记生成相同的输出, 但会将输出保存到指定的文件中 :  
    
        $ sed 's/test/trial/w testfile' data    //将替换后的内容写入到 testfile 文件
        This is a trial lien.
        This is a different line.
        
        $ cat testfile
        This is a trial line.
        
    sed 编辑器的正常输出出现在 STDOUT 中, 只有包含匹配模式的那些行保存在指定输出文件中。
    
2. **替换字符**  

    因此正则用正斜杠作定界符, 替换某个文件路径可能比较困难, 要斛决这一问题, 可以用分号 ( ; ) 作定界符 :  
    
        $ sed 's!/bin/bash!/bin/csh!' /etc/passwd

[返回目录](#toc)

##### 16.2.2 [使用地址](id:使用地址)

默认情况下, 在 sed 编辑器中你爱我吗的命令应用于所有文本数据行。如果仅想将某个命令应用于某一特定的文本数据行或一组文本数据行, 则必须使用 **行寻址** (line addresing)。

在 sed 编辑器中, 有两种行寻址形式 : 

- 行的数值范围;
- 筛选行的文本模式。

两种形式使用相同的格式指定的地址 :  

    [address] command

也可以将多个命令结合在一起, 用于一个特定的地址 :  

    address {
        command1
        command2
        command3
    }
    
sed 编辑器将指定的每一个命令仅应用于与指定地址匹配的行 :  

1. **数字式行寻址**  

    使用数字式行寻址时, 可以使用行在文本流中的位置引用行。 sed 编辑器指定文本流中的首行行号为 1, 每一个新行号顺延。
    
    在命令中指定的地址可以是单个行号, 也可以是由起始行号、逗号和结束行号指定的行范围 :  
    
        $ sed '2s/dog/cat/' data
        The quick brown fox jumps over the lazy dog
        The quick brown fox jumps over the lazy cat
        The quick brown fox jumps over the lazy dog
    
    sed 编辑器仅个性指定地址第二行中的文本, 而且它也可以使用行地址范围 :  
    
        $ sed '2,3s/dog/cat/' data
        The quick brown fox jumps over the lazy dog
        The quick brown fox jumps over the lazy cat
        The quick brown fox jumps over the lazy cat
        
    如果将命令应用于文本内从某一点开始直到文本结束的一组文本行, 可以使用特殊地址--`$` :  
    
        $ sed '2,$s/dog/cat/' data
        
2. **使用文本模式筛选器**  

    sed 编辑器允许指定为该命令筛选文本行所使用的文本模式, 格式是 :  
    
        /pattern/command
    必须用正斜杠标记指定的 pattern。 sed 编辑器仅将该命令应用于包含指定文本模式的行。  
    如果只想为用户 rich 更改默认的 shell, 可以使用 sed 命令 :  
    
        $ sed '/root/s/sh/csh/' /etc/passwd
        root:*:0:0:System Administrator:/var/root:/bin/csh    //这行可以找到 root, 只修改这行
        daemon:*:1:1:System Services:/var/empty:/usr/bin/bash

3. **组合命令**     

    如果需要在单独一行上执行多个命令, 请使用大括号将登记组合起来。 sed 编辑器将处理地址行上列出的所有命令 :  
    
        $ sed '2,${
        > s/fox/elephant/
        > s/dog/cat/
        > }' data 

[返回目录](#toc)

##### 16.2.3 [删除行](id:删除行)

删除命令 `d` 的功能就是删除。它将删除与所提供的寻址模式一致的所有文本行。

    $ cat data
    The quick brown fox jumps over the lazy dog
    The quick brown fox jumps over the lazy dog
    The quick brown fox jumps over the lazy dog
    The quick brown fox jumps over the lazy dog
    
    $ sed 'd' data
    $                //无输出, 全部删除

用数字指定删除行号 :   

    $ sed '3d' data
    This is line number 1.
    This is line number 2.
    This is line number 4.

范围删除行 :  

    $ sed '2,3d' data
    This is line number 1.
    This is line number 4.
    
    $ sed '3,$d' data        //从第三行删除到最后
    This is line number 1.
    This is line number 2.

**`Tips : `**  sed 编辑器不会处理原始文件, 所删除的所有文本行仅从 sed 编辑器输出中删除。原始文件仍然包含这些 “已删除的” 行。

使用两个文本模式删除行 : 指定一个模式 "打开" 行删除, 而第二个模式将 "关闭" 行删除。 sed 编辑器将删除指定的这之间的所有文本行 : 

    $ sed '/1/,/3/d' data    //当包含 '1' 时开启, 包含 '3' 时关闭
    This is line number 4.
    
**`Tips : `** 这样做必须小心, 只要 sed 编辑器在数据流中检测到开始模式, 删除功能就会随时开启。这可能会产生意想不到的结果 :  

    $ cat data
    This is line number 1.        //检测开始, 从此行开始删除行
    This is line number 2.
    This is line number 3.        //检测结束, 从此行结束删除
    This is line number 4.        
    This is line number 1 again.    //重新检测到 '1', 重新开启删除功能
    This is another text.           //此行在删除模式开启时被删除
    
    $ sed '/1/,/3/d' data
    This is line number 4.

[返回目录](#toc)

##### 16.2.4 [插入和附加文本](id:插入和附加文本)

sed 编辑器允许向数据流插入和附加文本。两个操作之间的差别很容易混淆 :  

- 插入命令 ( i ) 在指定行之前添加新的一行;
- 附加命令 ( a ) 在指定行之后添加新的一行。

不能在单命令行上使用这两个命令。必须单独指定要插入或附加的行。实现这一功能的格式为 :  

    sed '[address] command\
    new line'

new line 中的文本出现在 sed 编辑器输出中的指定位置。记住, 使用插入命令时, 文本出现在该数据流文本之前 :  
    
    $ echo "testing" | sed 'i\
    > This is a test'
    This is a test
    testing

然而, 在使用附加命令时, 在数据流文本之后出现文本 :  
    
    $ echo "testing" | sed 'a\
    > This is a test'
    testing
    This is a test
    
为在数据流行内部插入或附加数据, 必须使寻址告诉 sed 编辑器想要操作哪一行。可以与某一数字行号或文本模式匹配, 便不能使用地址范围 ( 这是有道理的, 因为只能在某一行而不是某一范围前或后插入或附加 )。

    $ sed '3i\                            //将内容插入到指定行前部
    > This is an inserted line.' data
    This is line number 1.
    This is line number 2.
    This is an inserted line.
    This is line number 3.
    
    $ sed '2a\                           //将两行数据追加到指定行后面
    > This is an inserted line.\
    > This is another inserted line.' data    
    This is line number 1.
    This is line number 2.
    This is an inserted line.
    This is another inserted line.
    This is line number 3.

[返回目录](#toc)

##### 16.2.5 [更改行](id:更改行)

更改行命令允许更改数据流中整行文本的内容, 要独立于 sed 命令的其他部分指定新行 :  

    $ sed '3c\
    > This is a changed line of text. ' data
    This is line number 1.
    This is line number 2.
    This is a changed line of text.
    This is line number 4.
    
也可以使用文本模式, 不过, 文本模式更改命令将更改与文本模式匹配的所有文本行 :  
    
    $ sed '/number 3/c\
    > This is a changed line of text.' data     //输出同上

在更改命令中, 可以使用地址范围, 但其结果可能不是预期结果 : 

    $ sed '2,3c\
    > This is a new line of text.' data
    This is line number 1.
    This is a new line of text.
    This is line number 4.

[返回目录](#toc)

##### 16.2.6 [变换命令](id:变换命令)

变换命令 ( y ) 是唯一对单个字符进行操作的 sed 编辑器命令。 变换命令使用格式 :  

    [address]y/inchars/outchars/
    
变换命令将 `inchars` 和 `outchars` 的值进行一对一映射。将 `inchars` 中的第一个字符转换为 `outchars` 中的第一个字符。第二个换为第二个。。。这样的映射一直持续直到完成指定的字符长度, 而且两字符长度应该相同。

    $ sed 'y/123/789/' data
    This is line number 7.
    This is line number 8.
    This is line number 9.
    This is line number 4.

这种转换没有次数限制 :  

    $ echo "This 1 is 1 test of 2 try" | sed 'y/123/456/'
    This 4 is 4 test of 5 try

[返回目录](#toc)

##### 16.2.7 [打印命令温习](id:打印命令温习)

还有三个命令也可以用于打印来自数据流的信息 : 

- 打印文本行的小写 p 命令;
- 打印行号的等号 ( = ) 命令;
- 列出行的 l ( 小写 L ) 命令。

1. **打印行**  

    它所做的只是打印您已知的文本, 打印命令最常用的是打印包含与文本模式匹配的文本行 :  
    
        $ sed -n '/number 3/p' data
        This is line number 3.
        
    在命令行上使用 `-n` 选项可以禁止所有其他的行, 而仅打印包含匹配文本模式的行。
    
        $ sed -n '2,3p' data
        This is line number 2.
        This is line number 3.
    
    打印命令的另一个用途是在改变某行之前查看该行, 如果利用替换或者更改命令。可以创建脚本在一行更改之前显示该行 :  
    
        $ sed -n '/number 3/{            //文本匹配模式
        > p
        > s/line/test/p
        > }' data
        This is line number 3.
        This is test number 3.
        
2. **打印行号**  

    等号命令打印数据流内当前行的行号。行号使用数据流中的换行符出现在数据流中时, sed 编辑器认为它结束一行文本。
    
        $ sed -n '/number 4/{
        > =                        //输出行号
        > p                        //打印原行内容
        > s/line/row/p             //打印替换后内容
        > }' data
        4
        This is line number 4.
        This is row number 4.

3. **列出行**  

    列出命令 ( l ) 允许打印数据流中的文本和不可打印的 ASCII 字符。所有不可打印的字符或者使用反斜杠加上八进制值表示, 或者使用常用的不可打印字符的标准 C 样式术语表示, 比如用 `\t` 表示制表符 :  
    
        $ cat data
        This    line    contains        tabs.
        $ sed -n 'l' data
        This\tline\tcontains\t\ttabs.$
    
    制表符的位置用 `\t` 表示。行尾的换行符用 `$` 表示。

[返回目录](#toc)

##### 16.2.8 [将文件用于 sed](id:将文件用于 sed) 

1. **写文件**  

    `w` 命令用于将文本行写入文件。 `w` 命令的格式为 :  
    
        [address]w filename
    
    `filename` 可以指定为相对或绝对路径, 但用户操作时一定要有写权限。地址可以是 sed 中使用的任意类型的寻址方法, 例如单一行号、文本模式、行号或者文本模式范围。
    
        $ sed -n '1,2w testfile' data
        $ cat testfile
        This is line number 1.
        This is line number 2.
        
        $ sed -n '/number 1/w testfile' data
        $ cat testfile        
        This is line number 1.
        
2. **从文件记取数据**
    
    读命令 ( r ) 允许您插入包含在独立文件中的数据。读命令格式为 :  
    
        [address]r filename
        
    `filename` 参数可以为包含数据的文件指定相对或绝对路径。对于读命令, 不能使用地址范围。只能指定单一的行号或文本模式地址。sed 编辑器在该地址之后插入文件中的文本。
    
        $ cat data_extra
        This is an added line.
        
        $ sed '3r data_extra' data
        This is line number 1.
        This is line number 2.
        This is line number 3.
        This is an added line.
        This is line number 4.
        
    sed 编辑器将数据文件中的所有文本行插入数据流。使用文本模式地址时, 工作方式相同 : 
    
        $ sed '/number 2/r data_extra' data
    
    如果将文本添加到数据流末尾, 只需使用美元地址符号 : 
    
        $ sed '$r data_extra' data
        
    读命令的一个很好的用途是和删除命令一起使用, 用另一个文件中的数据替代一个文件中的占位符。
    
        $ cat letter
        Would the following people: 
        List
        Please report to the office.
    
    套用信函使用类占位符 LIST 代替人员列表。要在点位符后插入人员列表, 只需使用读命令。不过, 这样仍会将点位符位符文本留在输出中。要删除点位符, 只需使用删除命令 :  
    
        $ sed '/LIST/{
        > r data
        > d
        > }' letter 
        
        Would the following people: 
        This is line number 1.
        This is line number 2.
        This is line number 3.
        This is line number 4.
        Please report to the office.
        

[返回目录](#toc)

    
    
        


    






    














