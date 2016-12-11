> 平台 centos 7, 64位系统


### 1. 确认是否安装过 `zsh`

    $ cat /etc/shells

#### 1.1 如果未安装, 则确认安装 : 

    $ yum install zsh.x86_64

#### 1.2 将 `zsh` 转为默认脚本 : 

    $ chsh -s /bin/zsh

#### 1.3 重新启动电脑 : 

    $ reboot



### 2. 下载 zsh 文件 : 

    $ git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh

### 3. 拷贝 zsh 的配置文件 : 

    $ cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

### 4. 运行 `zsh` 安装脚本, 安装完成 : 

    $ sh ~/.oh-my-zsh/tools/install.sh