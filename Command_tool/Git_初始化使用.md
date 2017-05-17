### Git 配置及简易上下行

#### 1. 配置本地git环境

请从Git官网下载最新版的Git客户端。

安装完客户端后，需要完成以下的配置：

**配置用户名**

获知用户名后，在命令行中输入：

    $> git config --global user.name "睡梦之龙"
    $> git config --global user.email "454769906@qq.com"

**检查配置**

最后检查user.name及user.email是否配置正确：

    $>  git config -l

#### 2、添加SSH公钥

公钥是CODE识别您的用户身份的一种认证方式，通过公钥，您可以将本地git项目与CODE建立联系，然后您就可以很方便的将本地代码上传到CODE，或者将CODE代码下载到本地了。

>以下介绍生成公钥和管理公钥的方法。如果你是在windows系统下使用，需要先安装git的windows客户端msysgit , 然后运行 Git Bash, 在弹出的终端中输入下面提示的代码。

**生成公钥前准备**

>首先检查本机公钥(Windows版本为 c:\Users\$current_user\.ssh )：

    $> cd ~/.ssh

如果提示：No such file or directory 说明你是第一次使用git。如果不是第一次使用，请执行下面的操作,清理原有ssh密钥(将原密钥备份到key_backup文件夹)。

    $> mkdir key_backup
    $> cp id_rsa* key_backup    
    $> rm id_rsa*



**生成新的密钥**

    $> ssh-keygen -t rsa -C "454769906@qq.com"


> 这里会要要求确认密钥添加位置, 默认为`~/.ssh`。如果是其他 git 库的话, 建议重新设置密钥添加位置。

在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入。

您可以在你本机系统盘下，您的用户文件夹里发现一个`.ssh`文件，其中的id_rsa.pub文件里储存的即为刚刚生成的ssh密钥。

>私钥：id_rsa  
公钥：id_rsa.pub (其中最后的邮箱为添加公钥时的名称, 邮箱不做为公钥)

**添加公钥**

登录CODE 或 OSC 平台，进入用户“账户设置”，点击右侧栏的“ssh公钥管理”，点击“添加公钥”，将刚刚生成的公钥填写到“公钥”栏，并为它起一个名称，保存即可。

注意：复制公钥时不要复制多余的空格，否则可能添加不成功。

#### 3. 初始化 Git 仓库

**上传本地文件**

初始化git库, 创建一个新文件上传到 Git 库

    $> cd target_fold
    $> git init
    $> touch README.md
    $> git add README.md
    $> git commit -m "first commit"
    $> git remote add origin https://git.oschina.net/shuimeng/project_name.git
    $> git push -u origin master    //以后可以用此命令上传至服务器了
    
**上传已有项目**

    $> cd existing_git_repo
    $> git remote add origin https://git.oschina.net/shuimeng/www_eju.git
    $> git push -u origin master