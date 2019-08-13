# Linux_cmd_ls命令

<!-- create time: 2016-05-27 14:34:54  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

`ls`命令是linux下最常用的命令。`ls`命令就是`list`的缩写,缺省下`ls`用来打印出当前目录的清单。如果`ls`指定其他目录,那么就会显示指定目录里的文件及文件夹清单。 通过 `ls` 命令不仅可以查看linux文件夹包含的文件,而且可以查看文件权限(包括目录、文件夹、文件权限),查看目录信息等等。

### 命令参数




单字母 | 全字 | 描述
----|---|---
-a | --all | 列出目录下的所有文件，包括以 . 开头的隐含文件
-A | --almost -all | 但不列出“.”(表示当前目录)和“..”(表示当前目录的父目录)。
---| --author | 打印每个文件的作者
-b | --escape | 打印不可打印字符的八进制值
---| --block -size=size | 使用 size-byte 块计算块大小
-B | --ignore -backups | 不要列出带波浪号的条目 ( 波浪号表示备份副本 )
-c |---| 根据最后一次修改的时间排序
-C |---| 按列列出条目
---| --color=when | 何时使用颜色 ( 总是、从不、自动 )
-d | --directory | 列出目录条目而不是内容，而不是显示其下的文件，不要废弃符号链接
-F | --classify | 向条目附加文件类型指示符 (***)
---| --file-type | 仅向某些文件类型 ( 非可执行文件 ) 附加文件类型指示符
---| --format=word | 以交叉、逗号、水平、长、单列、详细、垂直等形式格式化输出
-g |---| 列出文件所有者以外的详细信息
---| --group -directories -first | 在文件之前列出所有目录
-G | --no -group | 在长列表中不显示组名称
-h | --human -readable | 打印大小,K 表示千字节,M 表示兆字节,G 表示十亿字节 (***)
---| --si | 与-h 相同,介是使用的权重为 1000 而不是 1024
-i | --inode | 显示每个文件的索引号 ( inode )
-l |---| 显示长列表格式
-L | --dereference | 显示链接文件的源文件信息 
-n | --numeric -uid -gid | 显示用户 ID 和组 ID 而不是名称 (***)
-o |---| 在长列表中不显示所有者名称
-r | --reverse | 显示文件和目录时逆向排序
-R | --recursive | 递归列出子目录的内容 (***)
-s | --size | 打印每个文件的块大小
-S | --sort=size | 按照文件大小排列输出
-t | --sort=time | 按照文件个性时间排列输出
-u |---| 显示文件上一次访问时间而不是修改时间
-U | --sort=none | 输出列表不排序
-v | --sort=version | 根据文件版本排列输出
-x |---| 按行而不是列列出条目 (***)
-X | --sort=extension | 按文件扩展名排列输出
-1 |---| 每行只列出一个文件



    -a, –all 列出目录下的所有文件，包括以 . 开头的隐含文件
    -A 同-a，但不列出“.”(表示当前目录)和“..”(表示当前目录的父目录)。
    -c  配合 -lt：根据 ctime 排序及显示 ctime (文件状态最后更改的时间)配合 -l：显示 ctime 但根据名称排序否则：根据 ctime 排序
    -C 每栏由上至下列出项目
    -d, –directory 将目录象文件一样显示，而不是显示其下的文件
    -D, –dired 产生适合 Emacs 的 dired 模式使用的结果
    -f 对输出的文件不进行排序，-aU 选项生效，-lst 选项失效
    -g 类似 -l,但不列出所有者
    -G, –no-group 不列出任何有关组的信息
    -h, –human-readable 以容易理解的格式列出文件大小 (例如 1K 234M 2G)
    -H, –dereference-command-line 使用命令列中的符号链接指示的真正目的地
    -i, –inode 印出每个文件的 inode 号
    -I, –ignore=样式 不印出任何符合 shell 万用字符<样式>的项目
    -k 即 –block-size=1K,以 k 字节的形式表示文件的大小。
    -l 除了文件名之外，还将文件的权限、所有者、文件大小等信息详细列出来。
    -L, –dereference 当显示符号链接的文件信息时，显示符号链接所指示的对象而并非符号链接本身的信息  
    -m 所有项目以逗号分隔，并填满整行行宽
    -o 类似 -l,显示文件的除组信息外的详细信息。
    -r, –reverse 依相反次序排列
    -R, –recursive 同时列出所有子目录层
    -s, –size 以块大小为单位列出所有文件的大小
    -S 根据文件大小排序
    -t 以文件修改时间排序
    -u 配合 -lt:显示访问时间而且依访问时间排序
        配合 -l:显示访问时间但根据名称排序
        否则：根据访问时间排序
    -U 不进行排序;依文件系统原有的次序列出项目
    -v 根据版本进行排序
    -w, –width=COLS 自行指定屏幕宽度而不使用目前的数值
    -x 逐行列出项目而不是逐栏列出
    -X 根据扩展名排序
    -1 每行只列出一个文件
    –help 显示此帮助信息并离开
    –version 显示版本信息并离开
    
    –color[=WHEN] 控制是否使用色彩分辨文件。WHEN 可以是'never'、'always'或'auto'其中之一
        a     black    b     red
        c     green    d     brown
        e     blue     f     magenta
        g     cyan     h     light grey
        A     bold black, usually shows up as dark grey
        B     bold red
        C     bold green
        D     bold brown, usually shows up as yellow
        E     bold blue
        F     bold magenta
        G     bold cyan
        H     bold light grey; looks like bright white
        x     default foreground or background
    –sort=WORD 以下是可选用的 WORD 和它们代表的相应选项：    
        extension -X status -c    
        none -U time -t    
        size -S atime -u    
        time -t access -u    
        version -v use -u

