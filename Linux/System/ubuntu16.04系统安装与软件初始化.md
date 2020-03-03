系统配置：侧栏，背影，网络，触摸板

软件安装： 
1. 输入法安装
```bash
sudo apt install fcitx-table-wubi fcitx-table-wbpy
# 重启
```

2. 浏览器
firefox 账号同步，插件安装， 软件升级
chrome 安装

3. 邮箱
邮件配置，邮件过滤规则
新闻接收点

4. 安装zsh
```bash
sudo apt-get install curl
sudo apt-get install zsh
sudo apt-get install git
http://ohmyz.sh/
chsh -s $(which zsh)
```
5. 配置终端
快捷键

6. QQ安装 https://www.jianshu.com/p/1150aa5a6cec
```bash
sudo add-apt-repository ppa:wine/wine-builds
sudo apt-get update
sudo apt-get install winehq-devel
tar xvf wineQQ8.9_19990.tar.xz -C ~/
# 重新启动
```

7. 安装vim8 https://itsfoss.com/vim-8-release-install/
```bash
sudo add-apt-repository ppa:jonathonf/vim
sudo apt update
sudo apt install vim
```
8. 下载公共文档库和脚本库
````bash
cd Documents 
git clone https://github.com/kukalote/collect.git
cd /usr/local
sudo git clone --recursive https://github.com/kukalote/workshell.git
vim ~/.zshrc
```
```vim
if [ -f /usr/local/workshell/bashrc ]; then
    . /usr/local/workshell/bashrc
fi
```
vim /usr/share/vim/vimrc
```
if filereadable("/usr/local/workshell/vimrc")
    source /usr/local/workshell/vimrc
endif
```


9. 微信安装 http://blog.csdn.net/ch593030323/article/details/53571807
    chrome 浏览器安装
    virtualbox 安装
    goldenDict 安装 : /usr/share/goldendict-wordent 字典目录
	配置 wiki : editor->dictionary-> mediawik->english wiki[http=>https]
    flash 安装    http://www.linuxidc.com/Linux/2016-05/131098.htm
	





