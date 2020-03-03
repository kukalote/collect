#### Vim 配置文件

    "双引号开头会当作注释
	"设置当前字符编码为简体中文。
	set encoding=cp936
	set termencoding=utf-8
	set fileencoding=chinese
	set fileencodings=ucs-bom,utf-8,chinese,cp936
	set langmenu=zh_CN,utf-8


	"去除gui版本中的toolbar
	set guioptions-=T
	"编辑过程中，在右下脚显示光标位置的状态行
	set vb t_vb=


	"Vim 配置文件
	"无BOM 头
	set nobomb
	"显示行号
	set nu

	"根据上一行决定新行的缩进
	set autoindent

	"覆盖文件时保留备份文件
	"set backupcopy=auto

	"不生成备份
	set nobackup
	set nowritebackup
	set noswapfile
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

	"打开关键字色
	syntax on
	syntax enable

	"1.设想这样一个情况： 当前光标前面有若干字母， 按下 i 键进入了 Insert模式， 然后输入了 3 个字母， 再按 5 下删除(Backspace)。
	"   默认情况下，VIM 仅能删除新输入的 3 个字母， 然后喇叭嘟嘟响两声。 
	"   如果setbackspace=start， 则可以在删除了新输入的 3 个字母之后， 继续向前删除原有的两个字符。
	"2.再设想一个情况： 有若干行文字， 把光标移到中间某一行的行首， 按 i 键进入 Insert 模式， 然后按一下 Backspace。
	"   默认情况下， 喇叭会嘟一声，然后没有任何动静。 如果set backspace=eol，则可以删除前一行行末的回车，也就是说将两行拼接起来。
	"3.当设置了自动缩进后， 如果前一行缩进了一定距离，按下回车后，下一行也会保持相同的缩进。默认情况下，不能在 Insert 模式下直接按 Backspace 删除行首的缩进。
	"   如果set backspace=indent， 则可以开启这一项功能。    
	set backspace=indent,eol,start 

	"模式中有大写字母时不忽略大小写
	set smartcase
	"C 程序智能自动缩进
	set smartindent
	"插入 <Tab> 时使用 'shiftwidth'
	set smarttab
	"编辑时 <Tab> 使用的空格数
	set softtabstop=4

	"键入 <Tab> 时使用空格
	set expandtab
	"<Tab> 在文件里使用的空格数
	set tabstop=4

	"命令移动光标到行的第一个非空白
	set startofline
	" 搜索在文件尾折回文件头
	set wrapscan
	"不在单词中间断行，尽量在单词间空白处断开
	set lbr
	"打开断行模块对亚洲语言支持。m表示允许在两个汉字之间断行，即使汉字之间没有出现空格
	"B表示将两行合并为一行的时候，汉字与汉字之间不要补空格。该命令支持的更多选项请看用户手册
	set fo+=mB

	"长行回绕并在下一行继续
	set nowrap

	"设置主题
	colorscheme torte
	"colorscheme xun

	"检测文件类型
	filetype on


	"打开C/C++风格的自动缩进。
	set cindent
	"设定C/C++风格自动缩进的选项。
	set cinoptions=:0g0t0(sus

	" 显示 <Tab> 和 <EOL>
	set list
	set listchars=eol:$,tab:--,trail:*,extends:<,precedes:@,nbsp:%

	filetype plugin on 



	"不使用 selectmode
	"set selectmode=
	"当右键单击窗口的时候，弹出快捷菜单  set mousemodel=popup
	"不使用Shift+方向键选择文本，如果使用它可以用
	"set keymodel=startsel,stopsel打开它
	set keymodel=
	"指定在选择文本时，光标所在位置也属于被选中的范围。
	"如果指定selection=exclusive的话，可能会有某些文本无法被选中。
	"set selection=inclusive



	"添加水平滚动条，如果指定不折行的话，添加水平滚动条就很必要了。
	"set guioptions+=b
	"设置图形界面下的字体,可用:set guifont ?查询当前字体、字号
	"set guifont=Courier\ 9
	"if (has("gui_running"))
	" 图形界面下的设置
	"    set nowrap
	"    set guioptions+=b
	"    colo torte
	"else
	" 字符界面下的设置
	"    set wrap
	"    colo ron
	"endif 



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


	" 设置NerdTree插件
	"map <F3> :NERDTreeMirror<CR>
	"map <F3> :NERDTreeToggle<CR>



	"自动更换工作目录
	set autochdir 
	"ctags 文件设置, 当前工作目录下
	set tags=tags;

	"让taglist找到ctags
	" 已将ctags.exe加入系统环境变量
	let Tlist_Ctags_Cmd="ctags.exe"
	let Tlist_Show_One_File=1
	let Tlist_Sort_Type='name'
	let Tlist_Exit_OnlyWindow=1

	""let Tlist_OnlyWindow=1
	""let Tlist_Use_Right_Window=0
	""let Tlist_Show_Menu=1
	""let Tlist_Max_Submenu_Items=10
	""let Tlist_Max_Tag_length=20
	""let Tlist_Use_SingleClick=0
	""let Tlist_Auto_Open=0
	""let Tlist_Close_On_Select=0
	""let Tlist_File_Fold_Auto_Close=1
	""let Tlist_GainFocus_On_ToggleOpen=0
	""let Tlist_Process_File_Always=1
	""let Tlist_WinHeight=14
	""let Tlist_WinWidth=25
	""let Tlist_Use_Horiz_Window=0


	"winmanager插件管理
	let g:winManagerWindowLayout='TagList|FileExplorer'
	nmap wm :WMToggle<cr>


	"minibufexpl.vim 插件
	let g:miniBufExplMapCTabSwitchBufs=1
	let g:miniBufExplMapWindowsNavVim=1
	let g:miniBufExplMapWindowNavArrows=1

	"grep.vim 在工程中快速查找(按F3可以查找光标位置上的值)
	""nnoremap <silent> <F3> :Grep<CR>
	nnoremap <silent> <F3> :Grep<CR> 

	"visualmark 高亮书签(ctl-F2:添加/删除书签; (shift-)F2:跳转到书签)

	"vim74/syntax/php.vim加入:对php函数进行配色
	""syn match phpFunctions "\<[a-zA-Z_][a-zA-Z_0-9]*\>[^()]*)("me=e-2
	""syn match phpFunctions "\<[a-zA-Z_][a-zA-Z_0-9]*\>\s*("me=e-1
	""hi phpFunctions gui=NONE guifg=#ffe900




