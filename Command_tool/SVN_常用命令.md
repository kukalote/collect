###svn _常用命令

- [svn help](#help)
- [svn add](#add)
- [svn chechout (co)](#co)
- [svn commit (ci)](#ci)
- [svn update (up)](#up)
- [svn delete (del)](#del)
- [svn diff (di)](#di)
- [svn info](#info)
- [svn log](#log)
- [svn status (st)](#st)
- [svn merge](#merge)
- [svn import](#import)
- [svn revert](#revert)
- [svn resolve](#resolve)


#### 基础知识之版本常量

##### svn 版本信息

    -r 921:PREV
    
常量名 | 含义 | 内容获得
----|---|---
NUMBER | (字数类型)版本序号 | 版本库
DATE | 在时间开始的版本 | 版本库
HEAD | 版本库中最新的版本 | 版本库
BASE | 某个工作副本项的版本,注意这个是你上次 update 该项时的版本号,可能晚于当前最新的版本号 | 工作副本
COMMITTED | 某个工作副本项最近修改的版本，与BASE相同或更早 | 工作副本
PREV | 提交前的版本(committed-1) | 工作副本


##### svn status 展示状态

>**A item**  
>预定加入到版本库的文件、目录或符号链的item。  

>**C item**  
>当前文件item与版本库中有冲突.  

>**D item**  
>文件、目录或是符号链item预定从版本库中删除。  

>**M item**  
>文件item的内容被修改了。  

##### svn冲突解决方案

冲突选项 | 含义
----|---
 (p)ostpone | 优先更新文件,保留文件的冲突状态.
(d)iff | 用比较模式,展示与当前工作副本和冲突文件间的区别.
(e)dit | 用编辑器打开,编辑器是在环境变量中的EDITOR设定的.
(r)esolved | 编辑完文件,告诉svn 你已经解决冲突,内容以你修改后内容为准.
(m)ine-(f)ull | 丢弃服务器获取内容,以你本地内容为准.
(t)heirs-(f)ull | 弃用本地内容,以服务器内容为准.
(l)aunch | 抛出一个外部程序来解决冲突,这需要提前准备.
(h)elp | 展示你可用来解决冲突的选项.

对于每一个冲突的文件，Subversion放置三个额外的未版本化文件到你的工作拷贝：

    filename.mine
    
        你更新前的文件，没有冲突标志，只是你最新更改的内容。(如果Subversion认为这个文件不可以合并，.mine文件不会创建，因为它和工作文件相同。)
    filename.rOLDREV
    
        这是你的做更新操作以前的BASE版本文件，就是你在上次更新之后未作更改的版本。
    filename.rNEWREV
    
        这是你的Subversion客户端从服务器刚刚收到的版本，这个文件对应版本库的HEAD版本。

####[svn help](id:help)

    //显示 'svn' 帮助命令列表 `
    svn help
    
    //svn help 具体命令调用格式 :  
    svn help log

####[svn add](id:add)
添加指定文件到版本监测器
####[svn checkout `(co)`](id:co)
将线上数据检出到本地
####[svn commit `(ci)`](id:ci)    
将本地数据提交到版本库  

    //提交文件
    svn commit -m 提交备注 文件A 文件B   
####[svn update `(up)`](id:up)  
将版本库数据更新到本地

    //更新当前目录或指定文件
    svn up [ * | filename ] 
    
还原到指定版本(版本12) : 

    svn up -r 12 filename
####[svn delete `(del, remove, rm)`](id:del)
将版本库中数据删除

    svn delete -m  删除备注 svn://xxxx 
    svn del *  => svn ci -m 删除备注  
####[svn diff `(di)`](id:di)

参数 | 解释
----|---
-r [--revision] ARG | ()一些命令也可以取 `ARG1:ARG2` 范围) ARG 格式如续表
-c [--change] ARG| 对指定版本的修改( like -r ARG-1:ARG) `ARG 为数字型`
-v [--verbose] | 输出额外信息
-R [--recursive] | 递归调用
--summarize | 展示结果概要

[续表](id:extra_table_1)

参数格式 | 解释
----|---
NUMBER | 版本号
{DATE} | 版本起始时间
HEAD | 版本库中最近版本
BASE | 创建版本库复本时版本
COMMITTED | BASE 之前 或 最近提交版本
PREV | 仅在 COMMITTED 提交前的版本

    //本地修改与最近版本比较
    svn diff filename
    
    //本地个性与指定版本比较
    svn diff -r version1 filename
    
    //查看两版本间修改处
    svn diff -r version1:version2 filename

    //还可以用指定标识进行判断
    svn diff -r PREV:COMMITTED filename
    
    //对文件指定版本的修改
    svn diff  -c 784210 filename
    
    //某时间到当前版本文件修改比较(只显示文件名，不显示比较内容)
    svn diff -r {2016-02-04}:committed --summarize
    
    
####[svn info](id:info)
版本消息

 参数 | 解释
----|---
-r [--revision] ARG | ()一些命令也可以取 `ARG1:ARG2` 范围) ARG 格式如[续表](#extra_table_1)
-R [--recursive] | 递归调用

    //显示文件或目录的版本库信息
    svn info filename
####[svn log](id:log)
版本日志  

参数 | 解释
----|---
-r [--revision] ARG | ()一些命令也可以取 `ARG1:ARG2` 范围) ARG 格式如[续表](#extra_table_1)
-q [--quiet]| 只显示概况，不显示具体提交细节
-v [--verbose]| 显示具体提交细节 
-l [--limit] ARG | 最大输出日志条数


    svn log 文件A  
    
    //搜索10条数据   
    svn log -l 10     
    
    //显示提交详情(-q : 提交概括)   
    svn log -l 10 -v     
    
    //用时间作条件进行版本比较
    svn log -r {2016-02-04}:{2016-04-18} -l 100
    
**为什么svn log 不能看到刚提交的文件**    
svn提交时只是对提交的文件和目录修订了版本号，而这些文件和目录的父目录仍然保持老的版本号，而svn log缺省情况下是获取目录当前版本的历史，所以没有显示新提交的改变；要解决这个问题，svn update或者使用svn log -r LATESTreversion2.
####[svn status `(stat, st)`](id:st)
版本状态(动态)

参数 | 解释
----|---
-u [--show-updates] | 展示更新信息
-q [--quiet]| 只显示概况，不显示具体提交细节
-v [--verbose]| 显示具体提交细节 

    svn st 目录名|文件名  
    
#### [svn import](id:import)

`svn import` 是将未版本化文件导入版本库的最快方法，会根据需要创建中介目录。`svn import`不需要一个工作拷贝，你的文件会直接提交到版本库，这通常用在你希望将一组文件加入到Subversion版本库时，例如：

    $ svnadmin create /var/svn/newrepos
    $ svn import mytree file:///var/svn/newrepos/some/project -m "Initial import"
    Adding         mytree/foo.c
    Adding         mytree/bar.c
    Adding         mytree/subdir
    Adding         mytree/subdir/quux.h
    
    Committed revision 1.

在上一个例子里，将会拷贝目录mytree到版本库的some/project下：

    $ svn list file:///var/svn/newrepos/some/project
    bar.c
    foo.c
    subdir/
    
> 注意，在导入之后，原来的目录树并没有转化成工作拷贝，为了开始工作，你还是需要运行svn checkout导出一个工作拷贝。

####[svn copy](id:copy)

创建分支

    svn copy svn://xx.com/repo/trunk svn://xx.com/repo/branches/TRY-something -m 'make branches TRY-something'



####[svn merge `(stat, st)`](id:merge)




####[svn revert](id:revert)

假定我们在看`svn diff`的输出，你发现对某个文件的所有修改都是错误的，或许你根本不应该修改这个文件，或者是从开头重新修改会更加容易。

这是使用`svn revert`的好机会：

    $ svn revert README
    Reverted 'README'
    
版本恢复也可以还原一些已完成的操作, 比如, 你决定丢掉一个刚添加的文件(或是还原一个刚删除的文件):

    $ svn status foo
    ?      foo
    
    $ svn add foo
    A         foo
    
    $ svn revert foo
    Reverted 'foo'
    
    $ svn status foo
    ?      foo

>  `svn revert ITEM`的效果与删除ITEM然后执行`svn update -r BASEITEM`完全一样，但是，如果你使用`svn revert`它不必通知版本库就可以恢复文件。
    
####[svn resolve](id:resolve)

如果你选择稍后解决(`postponed`),你需要在提交前,先解决你的冲突.你需要使用`svn resolve`命令并且使用一个含几个选项的参数 (`--accept`) : 

    1. 如果你选择你你编辑前最近检索下来的版本, 可以配合 `base` 参数;
    2. 如果你要用你最近从服务器上拉下来的(弃用你编写的), 选择 `theirs-full` 参数.
    3. 如果你是有选择性的修改,那你可以手动处理, 然后选择使用 `working` 参数。
    
`svn resolve` 将移除三个临时文件,允许使用指定的选项 `--accept`,并且版本库将不再认为这个文件牌冲突状态 : 

    $ svn resolve --accept working sandwich.txt
    Resolved conflicted state of 'sandwich.txt'
    