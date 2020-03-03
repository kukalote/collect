# Linux_cmd_grep

<!-- create time: 2016-03-25 10:24:02  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

### Grep 命令 用法大全

`grep` 全称是**Global Regular Expression Print**，表示全局正则表达式版本，它的使用权限是所有用户。

`grep` 的工作方式是这样的，它在一个或多个文件中搜索字符串模板。如果模板包括空格，则必须被引用，模板后的所有字符串被看作文件名。搜索的结果被送到标准输出，不影响原文件内容。

`grep` 可用于 shell 脚本，因为 `grep` 通过返回一个状态值来说明搜索的状态，如果模板搜索成功，则返回 **0**，如果搜索不成功，则返回 **1**，如果搜索的文件不存在，则返回 **2**。我们利用这些返回值就可进行一些自动化的文本处理工作。

1、 参数：

    -a : --text   #不要忽略二进制的数据
    -A <显示行数> :  #除了显示符合范本样式的那一列之外，并显示该行之后的内容。
        --after-context=<显示行数>
    -b : #在显示符合样式的那一行之前，标示出该行第一个字符的编号。
        --byte-offset
    -B <显示行数> : #除了显示符合样式的那一行之外，并显示该行之前的内容。
        --before-context=<显示行数>
    -c : #计算符合样式的列数。
        --count
    -C <显示行数> : #除了显示符合样式的那一行之外，并显示该行之前后的内容。   
        --context=<显示行数>或-<显示行数>
    -d <动作> : #当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。   
        --directories=<动作>   
    -e <范本样式> : #指定字符串做为查找文件内容的样式。   
        --regexp=<范本样式>   
    -E : #将样式为延伸的普通表示法来使用。   
        --extended-regexp   
    -f <规则文件> : #指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。   
        --file=<规则文件>   
    -F : #将样式视为固定字符串的列表。   
        --fixed-regexp   
    -G : #将样式视为普通的表示法来使用。   
        --basic-regexp       
    -h : #在显示符合样式的那一行之前，不标示该行所属的文件名称。   
        --no-filename       
    -H : #在显示符合样式的那一行之前，表示该行所属的文件名称。   
        --with-filename       
    -i : #忽略字符大小写的差别。   
        --ignore-case       
    -l : #列出文件内容符合指定的样式的文件名称。   
        --file-with-matches       
    -L : #列出文件内容不符合指定的样式的文件名称。   
        --files-without-match       
    -n : #在显示符合样式的那一行之前，标示出该行的列数编号。   
        --line-number       
    -q : #不显示任何信息。   
        --quiet或--silent       
    -r : #此参数的效果和指定“-d recurse”参数相同。   
        --recursive       
    -s : #不显示错误信息。   
        --no-messages       
    -v : #显示不包含匹配文本的所有行。   
        --revert-match       
    -V : #显示版本信息。   
        --version       
    -w : #只显示全字符合的列。   
        --word-regexp       
    -x : #只显示全列符合的列。   
        --line-regexp       
    -y   #此参数的效果和指定“-i”参数相同。
    

2、RE（正则表达式）

    \         #忽略正则表达式中特殊字符的原有含义
    ^         #匹配正则表达式的开始行
    $         #匹配正则表达式的结束行
    \<        #从匹配正则表达式的行开始
    \>        #到匹配正则表达式的行结束
    [ ]       #单个字符；如[A] 即A符合要求
    [ - ]     #范围 ；如[A-Z]即A，B，C一直到Z都符合要求
    .         #所有的单个字符
    *         #所有字符，长度可以为0 
    \(..\)    #标记匹配字符，如'\(love\)'，love被标记为1。       
    x\{m,n\}  #重复字符x，至少m次，不多于n次，如：'o\{5,10\}'匹配5--10个o的行。
    \w        #匹配文字和数字字符，也就是[A-Za-z0-9]，如：'G\w*p'匹配以G后跟零个或多个文字或数字字符，然后是p。   
    \W        #\w的反置形式，匹配一个或多个非单词字符，如点号句号等。   
    \b        #单词锁定符，如: '\bgrep\b'只匹配grep。 
    
### grep命令使用简单实例
    
    //显示所有以d开头的文件中包含 test的行。
    $ grep 'test' d*
    
    //显示在aa，bb，cc文件中匹配test的行。
    $ grep 'test' aa bb cc
    
    //匹配’manic’和’man’，但不是’Batman’，
    $ grep '\<man' * 
    
    $ grep -n 't[ae]st' regular_express.txt
    8:I can't finish the test.
    9:Oh! The soup taste good.
    
    //grep不显示本身进程
    $ ps aux|grep \[s]sh    //这个还不太清楚原理

    $ ps aux | grep ssh | grep -v "grep"
    
    //输出非u开头的行内容
    $ cat test.txt |grep ^[^u]
    
    //显示当前目录下面以.txt 结尾的文件中的所有包含每个字符串至少有7个连续小写字符的字符串的行
    $ grep '[a-z]\{7\}' *.txt
    