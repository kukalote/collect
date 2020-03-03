# Vim_Ctags小结

<!-- create time: 2016-03-14 15:24:10  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->


### Ctags 命令选项
    
     ctags [options] [file(s)]
    
      -a   追加标签到已存在的标签文件。
           Append the tags to an existing tag file.
      -B   使用反向正则匹配
           Use backward searching patterns (?...?).
      -e   输出标签,用Emacs编辑器操作
           Output tag file for use with Emacs.
      -f <name>
           将标签写入指定文件
           Write tags to specified file. Value of "-" writes tags to stdout
           ["tags"; or "TAGS" when -e supplied].
      -F   用正向正则匹配
           Use forward searching patterns (/.../) (default).
      -h <list>
           指定文件扩展名列表
           Specify list of file extensions to be treated as include files.
           [".h.H.hh.hpp.hxx.h++"].
      -I <list|@file>
           A list of tokens to be specially handled is read from either the
           command line or the specified file.
      -L <file>
           A list of source file names are read from the specified file.
           If specified as "-", then standard input is read.
      -n   Equivalent to --excmd=number.
      -N   Equivalent to --excmd=pattern.
      -o   Alternative for -f.
      -R   递归操作
           Equivalent to --recurse.
      -u   不排序
           Equivalent to --sort=no.
      -V   Equivalent to --verbose.
      -x   Print a tabular cross reference file to standard output.
      --append=[yes|no]
           Should tags should be appended to existing tag file [no]?
      --etags-include=file
          Include reference to 'file' in Emacs-style tag file (requires -e).
      --exclude=pattern
          Exclude files and directories matching 'pattern'.
      --excmd=number|pattern|mix
           Uses the specified type of EX command to locate tags [pattern].
      --extra=[+|-]flags
          Include extra tag entries for selected information (flags: "fq").
      --fields=[+|-]flags
          Include selected extension fields (flags: "afmikKlnsStz") [fks].
      --file-scope=[yes|no]
           Should tags scoped only for a single file (e.g. "static" tags
           be included in the output [yes]?
      --filter=[yes|no]
           Behave as a filter, reading file names from standard input and
           writing tags to standard output [no].
      --filter-terminator=string
           Specify string to print to stdout following the tags for each file
           parsed when --filter is enabled.
      --format=level
           Force output of specified tag file format [2].
      --help
           Print this option summary.
      --if0=[yes|no]
           Should C code within #if 0 conditional branches be parsed [no]?
      --<LANG>-kinds=[+|-]kinds
           Enable/disable tag kinds for language <LANG>.
      --langdef=name
           Define a new language to be parsed with regular expressions.
      --langmap=map(s)
           Override default mapping of language to source file extension.
      --language-force=language
           Force all files to be interpreted using specified language.
      --languages=[+|-]list
           Restrict files scanned for tags to those mapped to langauges
           specified in the comma-separated 'list'. The list can contain any
           built-in or user-defined language [all].
      --license
           Print details of software license.
      --line-directives=[yes|no]
           Should #line directives be processed [no]?
      --links=[yes|no]
           Indicate whether symbolic links should be followed [yes].
      --list-kinds=[language|all]
           Output a list of all tag kinds for specified language or all.
      --list-languages
           Output list of supported languages.
      --list-maps=[language|all]
           Output list of language mappings.
      --options=file
           Specify file from which command line options should be read.
      --recurse=[yes|no]
           Recurse into directories supplied on command line [no].
      --regex-<LANG>=/line_pattern/name_pattern/[flags]
           Define regular expression for locating tags in specific language.
      --sort=[yes|no|foldcase]
           Should tags be sorted (optionally ignoring case) [yes]?.
      --tag-relative=[yes|no]
           Should paths be relative to location of tag file [no; yes when -e]?
      --totals=[yes|no]
           Print statistics about source and tag files [no].
      --verbose=[yes|no]
           Enable verbose messages describing actions on each source file.
      --version
           Print version identifier to standard output.
    
    
    
    
### Ctags命令示例

    $ ctags -R    //递归的为当前目录及子目录下的所有代码文件生成tags文件
    //为某些源码生成tags文件，使用如下命令
    $ ctags filename.c filename1.c file.h
    $ ctags *.c *.h

### vim 配置

动态配置

    在vim打开源码时，指定tags文件，才可正常使用，通常手动指定，在vim命令行输入：
    :set tags=./tags(当前路径下的tags文件)
    若要引用多个不同目录的tags文件，可以用逗号隔开

    或者，设置 ~/.vimrc，加入一行，则不用手动设置tags路径：
    set tags=~/path/tags

配置文件


    如果经常在不同工程里查阅代码，那么可以在~/.vimrc中添加：  
    set tags=tags;  
    set autochdir  


### 关于tags的一些操作

    设置好了tags文件，在定位变量/函数的定义时，最常用的快捷键是：  
    Ctrl + ]  
    跳转到变量或函数的定义处，或者用命令  
    :ta name  
    返回到跳转前的位置。
    Ctrl + o/t  



    $ vim -t myAdd  
    用vim打开文件时，添加参数-t funcName会自动打开定义该函数的文件并定位到定义首行，上面这句就是找到myAdd定义的文件打开并将光标置于定义的第一行处。

    :tags    
    会列出查找/跳转过程(经过的标签列表)  
    
    :ts
    显示当前标签所在的文件

    ctags不会生成局部变量的索引，
    
    关于更详细的ctags用法，vim中使用
    :help tags
