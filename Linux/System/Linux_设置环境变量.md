
### Linux_设置环境变量


### 5 使用 Linux 环境变量

- [查看全局全境变量](#printenv)
- [显示特定进程的环境变量](#set)
- [移除环境变量](#unset)
- 5.2 [设置环境变量](#设置环境变量)
    - 5.2.1 [设置本地环境变量](#设置本地环境变量)
    - 5.2.2 [设置全局环境变量](#设置全局环境变量)
- 5.4 [默认的 shell 环境变量](#默认的 shell 环境变量)
- 5.5 [设置 PATH 环境变量](#设置 PATH 环境变量)
- 5.6 [定位系统环境变量](#定位系统环境变量)
- 5.7 [变量数组](#变量数组)
- 5.8 [使用命令别名](#使用命令别名)







#### [printenv](id:printenv) : 查看全局环境变量 

Linux 系统会在开始 bash 会话时设置一些全局环境变量。系统环境变量始终使用全大写字母来区别于普通用户环境变量。
    
    $ printenv
如果显示单个环境变量的值,请使用 echo 命令。
    
    $ echo $HOME

[返回目录](#toc)




#### [set](id:set) : 显示特定进程的环境变量 

`set` 命令显示特定进程的所有环境变量集。但是,这其中也包括全局环境变量。还有一些额外的变量,它们是 **本地环境变量**。

[返回目录](#toc)



    set
#### [unset](id:unset) : 移除环境变量 

`unset` 能够删除已有的环境变量。

    $ unset test
    $ echo $test
    $ 
**`Tips : `** 在操作全局环境变量时,需要稍微使用一些技巧。如果在子进程中使用 `unset` 移除全局环境变量,则该操作只对子进程有效。全局环境变量在父进程中仍然可用 : 

    $ test=teting
    $ export test
    $ bash
    $ echo $test
    testing
    $ unset test
    $ echo $test
    $ exit
    exit
    $ echo $test
    testing

[返回目录](#toc)



#### 5.2 [设置环境变量](id:设置环境变量)

##### 5.2.1 [设置本地环境变量](id:设置本地环境变量)

你可以为环境变量分配一个数字或字符串,方法是使用等号将变量指定为具体值 : 

    $ test=testing
    $ echo $test
    testing
**`Tips : `**  

1. 如果要指定包含空格的字符串值,则需要使用单引号来指示字符的起始位置。
    
        $ test=testing a long string
        -bash:error
        $ test='testing a long string'
        $ echo $testing
        testing a long string
如果没有单引号,bash shell 将假定下一个字符是另一个要处理的命令。
2. 如果创建新环境变量,则建议使用小写字母。
3. 环境变量名称、等号和值之间没有空格。
4. 设置本地变量之后,可以在 shell 进程的任何地方使用它。但是,如果产生另外一个 shell,则不能在子 shell 中使用它。

[返回目录](#toc)



##### 5.2.2 [设置全局环境变量](id:设置全局环境变量)
全局环境变量在任何由设置全局环境变量的进程创建的子进程中都可见。创建全局环境变量的方法是创建一个本地环境变量,然后再将它导出到全局环境中。  
这将通过使用 `export` 命令来完成 : 

    $ echo $test
    testing a long string
    $ export test
    $ bash
    $ echo $test    //子进程中显示变量内容
    testing a long string


[返回目录](#toc)



#### 5.4 [默认的 shell 环境变量](id:默认的 shell 环境变量)
bash shell 默认将使用一些特定的环境变量来定义系统环境。你可以随时调用这些 Linux 系统中设置的变量。由于 bash shell 派生于原 Unix Bourn shell,因此它还包括最初在该 shell 中定义的环境变量。

**bash shell 提供的与原 Unix Bourn shell 兼容的环境变量**

变量 | 描述
----|---
CDPATH | 冒号分隔的目录列表,用作 cd 命令的搜索路径
HOME | 当前用户的主目录
IFS | 用于分隔字段的字符列表,shell 使用它们分隔文本字符串
MAIL | 当前用户邮箱的多个文件名。对于新邮件,bash shell 将检查该文件
MAILPATH | 当前用户邮箱的多个文件名,由冒号分隔。对于新邮件,bash shell 将检查该列表中的各个文件
OPTARG | getopts 命令处理的最后一个选项参数的值
OPTIND | getopts 命令处理的最后一个选项参数的索引值
PATH | 冒号分隔的目录列表,shell 将在这些目录中查找命令
PS1 | 主 shell 命令行界面提示字符串
PS1 | 次 shell 命令行界面提示字符串

除了默认的 Bourne 环境变量之外,bash shell 还提供了一些自已的变量 :   
**bash shell 环境变量**  

变量 | 描述
----|---
BASH | 执行当前 bash shell 实例的完整路径名称
BASH_ENV | 设置后,各 bash 脚本都将在运行之前尝试执行些变量定义的启动文件
BASH_VERSION | 当前 bash shell 实例的版本号
BASH_VERSINFO | 包含当前 bash shell 实例的主要及次要版本号的变量数组
COLUMNS | 包含当前 bash shell 实例所使用的终端的终端宽度
COMP_CWORD | 变量 COMP_CWORD 中的索引,它包含当前光标的位置
COMP_LINE | 当前命令行
COMP_POINT | 当前光标相对于当前命令开始处的位置的索引
COMP_WORDS | 包含当前命令行上的各单词的变量数组
COMPREPLY | 包含可能由 shell 函数生成的完成代码的变量数组
DIRSTACK | 包含目录栈当前内容的变量数组
EUID | 当前用户数值形式的有效用户 ID
FCEDIT | fc 命令使用的默认编辑器
FIGNORE | 冒号分隔的后缀列表,执行文件名完成(filename completion) 时忽略
FUNCNAME | 当前正在执行的 shell 函数的名称
GLOBIGNORE | 冒号分隔的模式列表,用于定义文件扩展忽略的文件名集合
GROUPS | 包含当前用户所在用户组的变量数组
Histchars | 最多 3 个字符,用于控制历史扩展
HISTCMD | 当前命令的历史记录数量
HISTCONTROL | 控制 shell 历史列表中输入的命令
HISTFILE | 保存 shell 历史记录的文件的名称 (默认为 .bash history)
HISTFILESIZE | 历史文件中可保存的最大行数
HISTIGNORE | 冒号分隔的模式列表,用于定义哪些命令将被历史文件忽略
HISTSIZE | 存储在历史文件中的最大命令数量
HOSTFILE | 包含当 shell 需要完成主机名时应该读取的文件的名称
HOSTNAME | 当前主机的名称
HOSTTYPE | 描述运行 bash shell 的机器的字符串
IGNOREEOF | shell 在退出之前必须接受的连续 EOF 字符的数量。如果该值不存在,则默认为1
INPUTRC | Readline 初始化文件的名称(默认为 .inputrc)
LANG | shell 的地区类别
LC_ALL | 覆盖 LANG 变量,定义地区类别
LC_COLLATE | 设置对字符串值进行排序时所使用的排序顺序
LC_CTYPE | 确定在文件名扩展和模式匹配中使用的字符译码
LC_MESSAGES | 确定在解释双引号字符串前出现美元符号时使用的地区设置
LC_NUMERIC | 确定格式化数字时使用的地区设置
LINENO | 当前执行脚本中的行号
LINES | 定义终端上可用的行数
MACHTYPE | 以 cpu-company-system 格式定义系统类型的字符串
MALCHECK | shell 应该检查新邮件的频率 （以秒为单位,默认为 60 秒）
OLDPWD | shell 中刚使用过的工作目录
OPTERR | 如果设置为1,则 bash shell 将显示 getopts 命令生成的错误
OSTYPE | 定义运行 shell 的操作系统
PIPESTATUS | 包含前台进程中的进程退出状态值列表的变量数组
POSIXLY_CORRECT | 如果设置了些变量,则 bash 将以 POSIX 模式启动
PPID | bash 父进程的进程ID (PID)
PROMPT_COMMAND | 如果设置了该变量,则表示在显示主提示符之前执行的命令
PS3 | select 命令使用的提示符
PS4 | 回显命令行之前显示的提示符(如果使用了 bash -x 参数)
PWD | 当前工作目录
RANDOM | 返回 0 到 32767 之间的随机数。为此变量分配的值将作为随机数生成程序的 read
REPLY | read 命令的默认变量
SECONDS | 自 shell 启动之后的秒数。为该变量分配值会将计时器重设为该值
SHELLOPTS | 已启用的 bash shell 选项列表,由冒号分隔
SHLVL | 指示 shell 级别,每次开始一个新 bash shell 时,该变量都增加 1
TIMEFORMAT | 指定 shell 如何显示时间值的格式
TMOUT | select 和 read 命令应该等待输入的时间 (以秒为单位)。默认值 0 表示无限等待
UID | 当前用户数值形式的真实用户id

**`Tips : `** 当使用 `set` 命令时,并非所有的默认环境变量都会显示出来。其原因是,虽然有很多默认环境变量,但并非所有变量都需要包含值。

#### 5.5 [设置 PATH 环境变量](id:设置 PATH 环境变量)

应用程序经常将它们的可执行程序放置在 PATH 环境变量以外的目录中。技巧是确保你的 PATH 环境变量包含应用程序所在的所有目录。

为 `PAHT` 添加任意新目录 : 

    $ echo $PATH
    /usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin
    $ PATH=$PATH:/home/rich/bin

[返回目录](#toc)



#### 5.6 [定位系统环境变量](id:定位系统环境变量)

Linux 系统使用环境变量在程序和脚本中标识自已,是为了获取程序的系统信息提供一种简便的方法。

通过登陆 Linux 系统启动 bash shell 时, bash 默认将检查一些文件以执行命令。这些文件被称为 **启动文件**。 bash 处理的启动文件依赖于启动 bash shell 的方法。  
可采用三种方法来启动 bash shell : 

- 在登录时作为默认登录 shell;
- 作为非登录 shell 的交互式 shell;
- 作为非交互式 shell 运行脚本。

    1. **登录shell**  
在登录到 Linux 系统中时,bash shell 将作为登录 shell 启动。登录 shell 将查找 4 个不同的启动文件来处理其中的命令。bash shell 处理文件的顺序如下 :    

      - `/etc/profile`;
      - `$HOME/.bash_profile`;
      - `$HOME/.bash_login`;
      - `$HOME/.profile`。  

    `/etc/profile` 文件是 bash shell 在系统上的主默认启动文件。系统上的每一个用户在登录时都将执行此启动文件。另外3个启动文件特定于各个用户，并且可以根据各用户的需求自定义它们。
    1. **/etc/profile 文件**  
    在此启动文件中设置的环境变量。  
    能迭代 `/etc/profile.d` 目录中的任何文件的 for 语句。这使 Linux 系统提供了一个放置特定于应用程序的启动文件 (这些文件将在你登录时由 shell 执行) 的地方。
    2. **$HOME 启动文件**  
    其余 3 个启动文件具备相同的功能-提供特定于用户的启动文件,用于定义特定于用户的环境变量。  
    用户可以编辑这些文件并添加自已的、对于他们启动的每一个 bash shell 会话都为活动状态的环境变量。  
    
   2. **交互式 shell**    
   如果启动了一个 bash shell 而没有登陆系统 (比如说, 只在 CLI 提示符中键入 bash), 则您启动的就是一个 **交互式 shell**。 `交互式 shell` 与 `登录 shell` 的行为不同, 但它仍然提供了一个 CLI 提示符供您输入命令。     
   
      如果 bash 作为 `交互式 shell` 启动, 则它不会处理 `/etc/profile` 文件。相反, 它将检查用户 HOME 目录中的 `.bashrc` 文件。   
   
      `.bashrc` 文件执行两个任务。首先, 它检查 `/etc` 目录中的公共 `bashrc` 文件。其次它为用户输入个人别名和私有脚本函数提供了地方。
   
      公共 `/etc/bashrc` 文件可由系统上启动了交互式 shell 会话的任何人运行。默认文件将设置一些环境变量。但注意,它没有使用 `export` 命令将它们设置为全局性质。请记住, 每次启动新交互式 shell 时都地运行交互式 shell 启动文件; 因此, 任何子 shell 都将自动执行交互式 shell 启动文件。
   
      您还将注意到, `/etc/bashrc` 文件也执行了位于 `/etc/profile.d` 目录中的、特定于应用程序的启动文件。

  3. **非交互式 shell**  
  这是系统开始执行 shell 脚本的 shell。 它的不同之处在于不用担心 CLI 提示符。但是,您仍然希望在每次启动系统中的脚本时运行特定的启动命令。
  
     为适应这种情况, bash shell 提供 BASH_ENV 环境变量。当 shell 开始一个非交互式 shell 进程时, 它将检查该环境变量表示的待执行启动文件的名称。如果该变量有值, 则 shell 将执行该文件中的命令。

[返回目录](#toc)



#### 5.7 [变量数组](id:变量数组)

环境变量的一个非常好的特性就是能够当作 **数组** 使用。 **数组** 是能保存多个值的变量。 **数组** 中的值既可以分别引用, 也可以作为整体引用。  

要为某个环境变量设置多个值, 只需将它们列出在圆括号中, 各值以空格分隔 :   

    $ mytest=(one two three four five)
    $ echo $mytest
    one
**`Tips : `** 如果将数组作为普通环境变量显示, 只有数组的第一个值将被显示出来。要引用某个单独的数组元素, 您必须使用一个数组索引值, 它表示该元素在数组中的位置。 索引值包含在方括号中 :   

    $ echo ${mytest[2]}
    three
*环境变量数组的索引值以0开始。*  

要显示整个数组变量, 可以使用星号通配符作为索引值 :   

    $ echo ${mytest[*]}
    one two three four five
    
还可以更改单个索引位置的值 :  
    
    $ mytest[2]＝seven
    $ echo ${mytest[*]}
    one two seven four five
    
还可以使用 `unset` 命令移除数组中的某个值, 但要小心, 因为这也需要一定的技巧 :   
    
    $ unset mytest[2]
    $ echo ${mytest[*]}
    one two four five
    
    $ echo ${mytest[2]}
    $
    $ echo ${mytest[3]}
    four

[返回目录](#toc)



#### 5.8 [使用命令别名](id:使用命令别名)

**命令别名** 允许你为公共命令 (以及它们的参数) 创建别名, 以尽可能减少录入工作。  
查看别名列表 :   

    $ alias -p

**命令别名** 与本地环境变量的行为相似。它们只对于定义范围内的 shell 进程有效 :  

    $ alias li='ls -il'
    $ bash 
    $ li 
    bash : li : command not found
在启动新的交互式 shell 时, bash shell 始终会读取 `$HOME/.bashrc` 启动文件。这是放置 **命令别名** 语句的绝佳位置。

[返回目录](#toc)


