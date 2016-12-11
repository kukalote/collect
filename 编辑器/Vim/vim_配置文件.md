#### Vim 配置文件
    
    "无BOM 头
    set nobomb
    "显示行号
    set nu
    
    "根据上一行决定新行的缩进
    set autoindent
    
    "覆盖文件时保留备份文件
    "set backupcopy=auto
    
    "高亮指定列
    "set colorcolumn
    
    "高亮光标所在屏幕行
    set cursorline
    
    "键入 <Tab> 时使用空格
    set expandtab
    
    "高亮最近的匹配搜索模式
    set hlsearch
    
    "输入搜索模式时同时高亮部分的匹配
    set incsearch
    
    
    "开始编辑文件时进入插入模式
    "set insertmode
    
    "显示屏幕的行数
    "set lines
    
    "% 能匹配的字符对
    set matchpairs=(:),{:},[:],<:>
    
    "标尺，在状态行里显示光标的行号和列号
    set ruler
    
    "(自动) 缩进使用的步进单位，以空白数目计
    set shiftwidth=4
    
    "在状态行里显示 (部分) 命令
    set showcmd
    
    "用于提示回绕行开始的字符串
    "set showbreak
    
    "自动补全标签时显示完整的标签匹配模式
    set showfulltag
    
    "插入括号时短暂跳转到匹配的括号
    set showmatch
    
    "在状态行上显示当前模式的消息
    set showmode
    
    "是否显示标签页行
    set showtabline=1
    
    
    "模式中有大写字母时不忽略大小写
    set smartcase
    "C 程序智能自动缩进
    "set smartindent
    "插入 <Tab> 时使用 'shiftwidth'
    set smarttab
    "编辑时 <Tab> 使用的空格数
    set softtabstop=4
    
    "命令移动光标到行的第一个非空白
    set startofline
    "<Tab> 在文件里使用的空格数
    set tabstop=4
    " 搜索在文件尾折回文件头
    set wrapscan
    
    "长行回绕并在下一行继续
    set nowrap
    
    "设置主题
    colorscheme xun
    
    " 显示 <Tab> 和 <EOL>
    set list
    set listchars=eol:$,tab:--,trail:*,extends:<,precedes:@,nbsp:%
    
    filetype plugin on 
    
    "设置变量 $VIMFILES
    if has("win32")
        let $VIMFILE=$VIM 
    else
        let $VIMFILES=$HOME.'/.vim'
    endif
    
    "vimWiki 生成HTML
    "F4 当前页生成HTML
    "Shift + F4 全部生成 HTML
    "map <S-F4>:VimwikiAll2HTML<cr>
    "map <F4>:Vimwiki2HMTL<cr>
