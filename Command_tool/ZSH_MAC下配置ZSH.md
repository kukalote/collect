# ZSH_MAC下配置ZSH

<!-- create time: 2016-03-09 18:02:28  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->

MAC下面的终端是神器。而且苹果非常贴心的为我们准备好了ZSH。可惜ZSH不是很好用，需要配合一些插件和模板：oh-my-zsh
### 将bash切换为zsh

    chsh -s /bin/zsh

其实还可以用which来定位（特别是ubuntu的童鞋）

    chsh -s `which zsh`

直接用zsh会很蛋疼，因为zsh功能很强大但是太复杂，所以需要oh-my-zsh来将它简单化。如果要切换回去：

    chsh -s /bin/bash

### 下载oh-my-zsh

1) 直接用git从github上面下载包：

    git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh 

2) 备份已有的zshrc(一般不需要)

    cp ~/.zshrc ~/.zshrc.orig

3) 替换zshrc

    cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc