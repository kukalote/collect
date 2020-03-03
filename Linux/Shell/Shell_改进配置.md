### 1. Bash Shell中忽略大小写的设置方法

创建 `~/.inputrc` ， 内容如下 :

    # do not show hidden files in the list
    set match-hidden-files off

    # auto complete ignoring case
    set show-all-if-ambiguous on
    set completion-ignore-case on
    "\ep": history-search-backward
    "\e[A": history-search-backward
    "\e[B": history-search-forward