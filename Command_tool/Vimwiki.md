### Vimwiki 配置与使用

##### 下载

vimwiki 安装方式有 `vba` 和 `zip` 两种方式。推荐使用vba 安装,因为它能保存你的安装信息,不高兴了还可以卸载（详细说明请:h vba）。  
要使用这种安装方式，需要有 vimball 插件。Windows 下的 gvim 已经自带。如果你还没有这东西可以先到 [vim.org](http://www.vim.org/scripts/script.php?script_id=1502) 自行下载。

##### 安装 

使用vim 打开 vimwiki.vba,然后执行`:so %`, 就这么简单。

##### 配置 

建立系统变量 :  `$VIMFILES`。$VIMFILES 是您的 `vimfiles` 。这需要在 vimrc 中定义系统变量。  
Windows 下该是 `$VIM/vimfiles`, Linux 下是 ~/.vim/。

    if has("win32")
      let $VIMFILES = $VIM.'/vimfiles'
    else
      let $VIMFILES = $HOME.'/.vim'
    endif

##### 开始使用 wiki 语法

输入命令 `<Leader>ww` (`\ww`) 可以快速打开 wiki 首页 (`~/vimwiki/index.wiki`)。j