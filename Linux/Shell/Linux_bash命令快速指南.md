- [bash 内置命令](#bash 内置命令)
- [bash 外部命令](#bash 外部命令)
- [环境变量](#环境变量)

### [bash 内置命令](id:bash 内置命令)

bash shell 将许多受欢迎的命令包含进了 shell 中, 使用这些命令会更节省时间。

命令 | 描述
----|---
`alias` | 定义指定命令的别名
`bg` | 以后台方式恢复一个作业
`bind` | 将键盘序列绑定到一个 readline 函数或宏
`break` | 从 for、while、select 或 until 循环中退出
`builtin` | 执行指定的 shell 内置命令
`cd` | 将当前目录更改为指定目录
`caller` | 返回任何活动的子例程调用的上下文
`command` | 执行指定的命令, 而不使用常规的 shell 查找
`compgen` | 生成指定单词的可能完整匹配
`complete` | 显示指定的单词将如何完成
`continue` | 恢复 for、while、select 或 until 循环的下一个递归
`declare` | 声明变量或变量类型
`dirs` | 显示当前记住的目录列表
`disown` | 从作业表中删除指定作业的进程
`echo` | 将指定的字符串显示到 STDOUT
`enable` | 启用或禁用指定的内置 shell 命令
`eval` | 将指定的参数连结到单个命令中, 然后执行该命令
`exec` | 将 shell 进程替换为指定的命令
`exit` | 以指定的退出状态强制 shell 退出
`export` | 为子 shell 进程设置特定的变量
`fc` | 从历史列表中选择命令列表
`fg` | 以前台模式恢复一项作业
`getopts` | 解析指定的位置参数
`hash` | 查找和记住指定命令的全路径名
`help` | 显示帮助文件
`history` | 显示命令历史记录
`jobs` | 列表活动的作业
`kill` | 将系统信号发送给指定的进程 ID (PID)
`let` | 计算在数学表达式中的每个参数
`local` | 在函数中创建一定范围的变量
`logout` | 退出登录的 shell
`popd` | 从目录堆栈中删除条目
`printf` | 使用格式化的字符串显示文本
`pushd` | 将目录添加到目录堆栈中
`pwd` | 显示当前工作目录的路径名
`read` | 从 STDIN 读取一行数据并分配给一个变量
`readonly` | 从 STDIN 读取一行数据并分配给一个不可更改的变量
`return` | 强制函数以某个值退出, 这个值是执行调用脚本中检索不到的值
`set` | 设置和显示环境变量值和 shell 属性
`shift` | 将参数轮换一个位置
`shopt` | 切换控制着可先 shell 行为的变量值
`suspend` | 挂起 shell 的执行, 直至收到 SIGCONT 信号
`test` | 基于指定的条件, 返回退出值 0 或 1
`times` | 显示累积的用户和系统
`trap` | 如果收到指定的系统信号, 则执行指定的命令
`type` | 显示指定的单词将如何被解析并作为命令进行使用
`ulimit` | 为系统用户设置特定资源的限制
`umask` | 为新创建的文件和目录设置默认权限
`unalias` | 删除指定的别名
`untest` | 删除指定的环境变量或 shell 属性
`wait` | 等候指定的进程完成并返回退出状态


### [bash 外部命令](id:bash 外部命令)

除了内置命令外, bash shell 利用外部命令来允许用户管理文件系统以系统以及操作文件和目录。

**bash shell 外部命令**

命令 | 描述
----|---
`bzip2` | 使用 Burrows-Wheeler 块排序算法和 Huffman 编码进行压缩
`cat` | 列出指定文件的内容
`chage` | 更改指定系统用户账户的密码过期日期
`chfn` | 更改指定用户账户的注释信息
`chgrp` | 更改指定文件或目录的默认组
`chmod` | 更改指定文件或目录的系统安全权限
`chown` | 更改指定文件或目录的默认所有者
`chpasswd` | 读取包含登录名和密码对的文件并更新密码
`chsh` | 更改指定用户账户的默认 shell
`compress` | 原始的 Unix 文件压缩实用程序
`cp` | 将指定的文件复制到另一个位置
`df` | 显示所有已挂载设备的硬盘使用情况统计信息
`du` | 显示指定文件路径的磁盘使用情况统计信息
`file` | 显示指定文件的文件类型
`finger` | 显示 Linux 系统或远程系统的用户账户信息
`grep` | 在文件中搜索指定的文本字符串
`groupadd` | 创建新系统组
`groupmod` | 修改现有系统组
`gzip` | GNU 项目的压缩 ( 使用 Lempel-Ziv 压缩 )
`head` | 显示指定文件内容的第一部分
`killall` | 基于进程名称将系统信号发送到一个正在运行的进程
`less` | 查看文件内容的高级方式
`link` | 使用别名创建到文件的链接
`ls` | 列出目录内容
`mkdir` | 在当前目录下创建指定的目录
`more` | 列出指定文件的内容, 并且显示一屏数据后暂停
`mount` | 将磁盘设备显示或挂载到虚拟文件系统
`passwd` | 更改系统用户账户密码
`ps` | 显示系统上正在运行的进程信息
`pwd` | 显示当前目录
`mv` | 重命名文件
`rm` | 删除指定的文件
`rmdir` | 删除指定的目录
`sort` | 基于指定的顺序在数据文件中组织数据
`stat` | 查看指定文件的统计信息
`tail` | 显示指定文件内容的最后一部分
`tar` | 将数据和目录归档到单个文件
`touch` | 创建新的空文件, 或 更新现有文件的时间戳
`umount` | 从虚拟文件系统中删除已经挂载的磁盘设备
`useradd` | 创建现有的系统用户账户
`userdel` | 删除现有的系统用户账户
`usermod` | 修改现有的系统用户账户
`zip` | Windows PKZIP 程序的 Unix 版本

### [环境变量](id:环境变量)

bash shell 还利用了许多环境变量。虽然环境变量不是命令, 但它们会影响 shell 命令的操作。

**bash shell 环境变量**

变量 | 描述
----|---
`$*` | 将所有命令行参数包含在单个文本值中
`$@` | 将所有命令行参数包含在分隔的文本值中
`$#` | 命令行参数的个数
`$?` | 最近使用过的前台进程的退出状态
`$-` | 当前命令行选项的标记
`$$` | 当前 shell 的进程 ID (PID)
`$!` | 最近在后台执行的 PID
`$0` | 命令行上的命令名称
`$_` | shell 的绝对路径名
`BASH` | 用于调用 shell 的全文件名
`BASH_ARGC` | 当前子例程中的参数数量
`BASH_ARGV` | 包含所有指定命令行参数的数组
`BASH_COMMAND` | 当前正在执行的命令名称
`BASH_ENV` | 在设置时, 每个 bash 脚本在运行前都试图执行由此变量定义的启动文件
`BASH_EXECUTION_STRING` | 在 -C 命令行选项中使用的命令
`BASH_LINENO` | 脚本中包含每个命令的行号的数组
`BASH_REMATCH` | 包含与指定正则表达式匹配的文本元素的数组
`BASH_SOURCE` | shell 中包含已声明函数的源文件名的数组
`BASH_SUBSHELL` | 当前 shell 中出现的子 shell 数量
`BASH_VERSION` | bash shell 的当前实例版本号
`BASH_VERSINFO` | 一个可变数组, 它包含 bash shell 当前实例的主要版本和次要版本
`COLUMNS` | 用于当前 bash shell 实例的终端的宽度
`COMP_CWORD` | 变量 COMP_WORDS 的索引,包含当前的光标位置
`COMP_LINE` | 当前命令行
`COMP_POINT` | 当前光标位置的索引 ( 与当前命令的开始处相关 )
`COM_WORDBREAKS` | 一系列字符, 在执行单词完成时用作单词的分隔符
`COMP_WORDS` | 一个变量数组, 包含当前命令行上的各个单词
`COMPREPLY` | 一个变量数组, 包含 shell 函数生成的可能的完成代码
`DIRSTACK` | 包含当前目录堆栈内容的变量数组
`EUID` | 当前用户的有效数字用户ID
`FCEDIT` | fc 命令的默认编辑器
`FIGNORE` | 以冒号分隔的前缀列表, 用于执行文件名完成时要忽略的项
`FUNCNAME` | 当前正在执行的函数的名称
`GLOBIGNORE` | 以冒号分隔的模式列表, 它定义了一系列被文件扩展忽略的文件名
`GROUPS` | 包含组列表的变量数组, 当前用户是组成员之一
`histchars` | 控制历史扩展的字符 ( 不超过 3 个字符 )
`HISTCMD` | 当前命令的历史号
`HISTCONTROL` | 控制输入到 shell 历史列表的命令
`HISTFILE` | 保存 shell 历史列表的文件名称 ( 默认是 .bash_history )
`HISTFILESIZE` | 保存到历史文件的最大行数
`HISTIGNORE` | 以冒号分隔的模式列表, 用于决定历史文件中应忽略哪些命令
`HISTSIZE` | 保存到历史文件的最大命令个数
`HOSTFILE` | 在 shell 需要完成主机名时, 此变量包含应该读取的文件名称
`HOSTNAME` | 当前的主机名
`HOSTTYPE` | 描述 bash shell 所运行在计算机的字符串
`IGNOREEOF` | shell 在退出前必须收到的连续 EOF 字符的个数 如果此值不存在, 则默认为 1
`INPUTRC` | Readline 初始化文件的名称 ( 默认为 .inputrc )
`LANG` | shell 的本地类别
`LC_ALL` | 定义本地类别, 覆盖 LANG 变量
`LC_COLLATE` | 在对字符串值排序时, 设置要使用的排序顺序
`LC_CTYPE` | 决定要在文件名扩展和模式匹配中使用的字符解释
`LC_MESSAGES` | 在解析前缀为美元符号的带双引号的字符串, 决定要使用的本地设置
`LC_NUMERIC` | 决定格式化数字时使用的本地设置
`LINENO` | 当前正在执行的脚本行号
`LINES` | 定义终端上可用的行数
`MACHTYPE` | 以 cpu-company-system 格式定义系统类型的字符串
`MAILCHECK` | shell 检查新mbwrr时间间隔 ( 以秒为单位, 默认为 60 秒 )
`OLDPWD` | shell 中使用的上一个工作目录
`OPTERR` | 如果设置为 1, 则 bash shell 显示由 getopts 命令产生的错误
`OSTYPE` | 定义运行 shell 的操作系统的字符串
`PIPESTATUS` | 一个变量数组, 包含来自前台进程的进程退出状态值的列表
`POSIXLY_CORRECT` | 如果设置此变量, 则 bash 以 POSIX 模式启动
`PPID` | bash shell 父进程的进程 ID (PID)
`PRCOMPT_COMMAND` | 如果设置此变量, 则命令将在显示主提示之前得以执行
`PS1` | 主命令行提示字符串
`PS2` | 次命令行提示字符串
`PS3` | select 命令使用的提示
`PS4` | 如果使用了 bash 的 -x 参数, 则在显示命令行之前显示指示
`PWD` | 当前的工作目录
`RANDOM` | 返回一个 0~32767 之间的随机数。将值分配给此变量作为随机数生成器的种子
`REPLY` | read 命令的默认变量
`SECONDS` | shell 启动后的秒数。为此变量分配时, 计时器将被重置为此值
`SHELLOPTS` | 以冒号分隔的已启用的 bash shell 选项的列表
`SHLVL` | 指示 shell 的级别, 每当启动新的 bash shell 时加 1
`TIMEFORMAT` | 指定 shell 以何种格式显示时间值
`TMOUT` | select  和 read 命令等待输入的秒数, 默认是 0 秒, 即无限等待
`UID` | 当前用户的数字实际用户 ID
