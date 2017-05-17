# Vim_插件地址及配置

<!-- create time: 2016-03-14 15:09:33  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->


### 格式

### 插件名称
> 插件下载地址

    插件配置信息

### phpcheck::php语法检测

>http://www.vim.org/scripts/script.php?script_id=4984

    let g:PHP_SYNTAX_CHECK_BIN = ‘/apps/php/bin/php'
    
### Ctags::标签管理器

> http://ctags.sourceforge.net/  
> 根据平台下载相应的插件  
> 解压后使用 bin 目录下的 ctags 或 ctags.exe 执行 `ctags -R .` 来生成标签文件 `tags`  
> 可以使用 `ctrl+]` 来跳转标签、`Ctrl+o` 、`Ctrl+t` 来进行跳转、也可以使用命令 `:ta name`

    set tags=tags;    //指定生成tags文件地址,此处指当前工作目录
    set autochdir
    
### taglist::标签列表

> http://www.vim.org/scripts/script.php?script_id=273

    "设置ctags程序路径
    let Tlist_Ctags_Cmd = '/usr/bin/ctags'
    
    "设置taglist打开关闭的快捷键F8
    noremap <F8> :TlistToggle<CR>
    
    "更新ctags标签文件快捷键设置
    noremap <F6> :!ctags -R<CR>
    
    "启动vim后自动打开taglist窗口
    let Tlist_Auto_Open = 1
    
    "不同时显示多个文件的tag，仅显示一个
    let Tlist_Show_One_File = 1
    
    "taglist为最后一个窗口时，退出vim
    let Tlist_Exit_OnlyWindow = 1
    
    "taglist窗口显示在右侧，缺省为左侧
    let Tlist_Use_Right_Window =1
    
    "设置taglist窗口大小
    "let Tlist_WinHeight = 100
    let Tlist_WinWidth = 40
    
 还有一些快捷键
 
    <CR>          跳到光标下tag所定义的位置，用鼠标双击此tag功能也一样（但要在vimrc文件中打开此项功能）
    o             在一个新打开的窗口中显示光标下tag
    <Space>       显示光标下tag的原型定义
    u             更新taglist窗口中的tag
    s             更改排序方式，在按名字排序和按出现顺序排序间切换
    x             taglist窗口放大和缩小，方便查看较长的tag
    +             打开一个折叠，同zo
    -             将tag折叠起来，同zc
    *             打开所有的折叠，同zR
    =             将所有tag折叠起来，同zM
    [[            跳到前一个文件
    ]]            跳到后一个文件
    q             关闭taglist窗口
    <F1>          显示帮助
    
可操作命令

    在vim中，打开taglist窗口使用
    :Tlist (:TlistOpen, :TlistToggle)
    
    关闭窗口可使用
    :Tlist (:TlistClose, :TlistToggle)
    

    