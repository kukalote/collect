### shell_xargs命令介绍


有时候, 我们没法用管道来提供那些只有通过命令行参数才能提供的数据。  
该 `xargs` 命令出场了, 它擅长将标准输入数据转换成命令行参数。 `xargs` 能够处理 stdin 并将其转换为特定命令的命令行参数。 `xargs` 也可以将单行或多行文本输入转换成其他格式, 例如单行变多行或多行变单行。  

`xargs` 命令应该紧跟在管道操作符之后, 以标准输入作为主要的源数据流。它使用 stdin 并通过提供命令行参数来执行其他命令, 例如 : 

    command | xargs
    
`xargs` 命令把从 stdin 接收到的数据重新格式化, 再将其作为参数提供给其他命令。  
`xargs` 可以作为一种替代, 其作用类似于 `find` 命令中的 `-exec`。下面是使用 `xargs` 命令的各种技巧 : 

1. 将多行输入转换成单行输出  : 

    只需要将换行符移除, 再用 " " (空格)进行代替, 就可以实现多行输入的转换。'\n' 被解释成一个换行符, 换行符其实就是多行文本之间的定界符。利用 `xargs`, 我们可以利用空格替换掉换行符, 这样就能够将多行文本转换成单行文本 : 
    
        $ cat example.txt #样例文件
        1 2 3
        4 5
        6
        
        $ cat example.txt | xargs
        1 2 3 4 5 6
        
2. 将单行输入转换成多行输出, -n : 

    指定每行最大的参数量 `n`, 我们可以将任何来自 stdin 的文本划分成多行, 每行 `n` 个参数。每一个参数都是由 " "(空格)隔开的字符串。空格是默认的定界符。下面的方法可以将单行划分成多行 : 
    
        $ cat example.txt | xargs -n 3
        1 2 3
        4 5 6
3. 自己指定界符来分隔参数, -d : 

    用 `-d` 选项为输入指定一个定制的定界符 : 
    
        $ echo "splitXsplitXsplitXsplit" | xargs -d X
        split split split split 
        
    在上面代码中, stdin 是一个包含了多个 X 字符的字符串。我们可以用 `-d` 将 X 作为输入定界符。在这里, 我们明确指定 X 作为输入定界符, 而在默认情况下, `xargs` 采用 **内部字段分隔符** (空格) 作为输入定界符。
    
    结合 `-n` 选项, 我们可以将输入划分成多行, 而每行包含两个参数 : 
    
        $ echo "splitXsplitXsplitXsplit" | xargs -d X -n 2
        split split
        split split
4. 替换命令参数, -I : 

    如果我们的命令格式是固定不变的, 而只是某个参数会有所变化 : 
    
        ./cecho.sh -p arg1 -l
    比如, 上面的 arg1 是唯一的可变内容, 其余部分都保持不变。我们可以从文件 (args.txt) 中读取参数, 并按照下面的方式提供给命令 : 
    
        ./cecho.sh -p arg1 -l
        ./cecho.sh -p arg2 -l
        ./cecho.sh -p arg3 -l
        
    `xargs` 有一个选项 `-I`, 可以提供上面这种形式的命令执行序列。我们可以用 `-I` 指定替换字符串, 这个字符串在 `xargs` 扩展时会被替换掉。如 : 
    
        $ cat args.txt | xargs -I {} ./cecho.sh -p {} -l
        -p arg1 -l #
        -p arg2 -l #
        -p arg3 -l #
    `-I {}` 指定了替换字符串。对于每一个命令参数, 字符串 `{}` 都会被从 stdin 读取到参数替换掉。
    
    > 使用 -I 的时候, 命令以循环的方式执行。如果有3个参数, 那么命令就会连同 {} 一起被执行 3 次。在每一次执行中 {} 都会被替换为相应的参数。


#### 实战应用

1. 读取stdin, 将格式化参数传递给命令

    编写一个小型的定制版 echo 来更好地理解用 `xargs` 提供命令行参数的方法 : 
    
        #!/bin/bash
        # 文件名 :  cecho.sh
        
        echo $*'#'
        
    再假设有另一个文件, 其值可以作为参数用于命令 : 
    
        $ cat args.txt
        arg1
        arg2
        arg3
        
    当参数被传递给文件  cecho.sh 文件后, 它会将这些参数打印了来, 并以 # 字符结尾。例如 : 
    
        $ ./cecho.sh arg1 arg2
        arg1 arg2 #
        
    下面是我们要实现的效果 : 
    
    - 每次使用指定参数 : 
    
            $ cat args.txt | xargs -n 1 ./cecho.sh
            arg1 #
            arg2 #
            arg3 #
        每次使用命令都是 : 
            
            INPUT | xargs -n X
            
        一次性提供所有的参数, 可以使用 : 
        
            $ cat args.txt | xargs ./cecho.sh
            arg1 arg2 arg3 #
            
2. 结合 find 使用 xargs

    `xargs` 和 `find` 是一以死党。两者结合使用可以可以让任务变得更轻松。不过人们通常以一种错误的方式组合使用它们 : 
    
        $ find . -type f -name "*.txt" -print | xargs rm -f    #这很危险
        
    原因在于, 我们没有预测 `find` 命令输出结果的定界符究竟是什么 ( '\n' 或者 ' ' )。很多文件名中都可能含有空格符 (' '),因此 `xargs` 很可能会误认为它们是定界符。
    
    只要我们把 `find` 的输出作为 `xargs` 的输入, 就必须将 `-print0` 与 `find` 结合使用, 以字符 null ('\0') 来分隔输出。然后用 `xargs -0` 将 \0 作为输入定界符。  
    
    用 `find` 匹配并列出所有的 .txt 文件, 然后用 `xargs` 将这些文件删除 : 
    
        $ find . -type f -name "*.txt" -print0 | xargs -0 rm -fR
        
3. 统计源代码目录中所有 C 程序文件的行数

    统计所有 C 程序文件的行数 (Lines Of Code, LOC) 是大多数程序员都会遇到的任务。完成这项任务的代码如下 : 
    
        $ find source_code_dir_path -type f -name "*.c" -print0 | xargs -0 wc -l
        
4. 结合 stdin, 巧妙运用 while 语句和子 shell

    `xargs` 只能以有限的几种方式来提供参数, 而且它也不能为多组命令提供参数。要执行包含来自标准输入的多个参数的命令, 有一种非常灵活的方法。包含 `while` 循环的子 shell 可以用来读取参数, 然后通过一种巧妙的方式执行命令 : 
    
        $ cat files.txt | ( while read arg; do cat $arg; done ) 
        # 等同于 cat files.txt | xargs -I {} cat {}
    
    在 `while` 循环中, 可以将 cat $arg 替换成做任意数量的命令, 这样我们就可以对同一个参数执行多条命令。也可以不借助管道, 将输出传递给其他命令。这个技巧能够适用于各种问题场景。子 shell 操作符内部的多个命令可以作为一个整体来运行。

   