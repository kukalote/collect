# Linux yum 与 rpm 包管理

**yum**（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及SUSE中的Shell前端软件包管理器。基於RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。

yum的命令形式一般是如下：yum [options] [command] [package ...]
其中的[options]是可选的，选项包括-h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。[command]为所要进行的操作，[package ...]是操作的对象。

### yum 命令概要

#### 查找与显示

```bash
$ yum info package1  #显示安装包信息[package1]
$ yum list package1 #显示所有已经安装和可以安装的程序包[package1]

```

#### 安装
```bash
$ yum install package1 # 安装指定的安装包[package1]
```

#### 更新和升级
```bash
$ yum update package1 # 更新指定程序包[package1]
$ yum check-update # 检查可更新的程序
$ yum upgrade package1 # 升级指定程序包[package1]
$ yum groupupdate group1 # 升级程序组[group1]
```

#### 删除程序
```bash
$ yum remove package1 # 删除程序包[package1]
$ yum groupremove group1 # 删除程序组[group1]
$ yum deplist package1 # 查看程序[package1]依赖情况
```

#### 清除缓存
```bash
$ yum clean packages # 清除缓存目录下的软件包
$ yum clean headers # 清除缓存目录下的 headers
$ yum clean oldheaders # 清除缓存目录下旧的 headers
# yum clean all (= yum clean packages; yum clean oldheaders)
$ yum clean  # 清除缓存目录下的软件包及旧的headers
```
#### 源文件安装
```bash
$ wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo  # 远程源文件安装
$ cd /tmp/; wget http://mirrors.aliyun.com/repo/Centos-6.repo
$ yum localinstall /tmp/Centos-6.repo  # 本地源文件安装
```
#### 生成缓存
```bash
$ yum makecache
```
> Tips : 请在镜像源安装完成后，重新生成镜像的缓存

#### 其他信息
```bash
$ yum info updates # 列出所有可更新的软件包信息
$ yum info installed # 列出所有已安裝的软件包信息
$ yum provides # .列出软件包提供哪些文件
```
### rpm 命令概要

#### 初始化 rpm 数据库
```bash
$ rpm --initdb
$ rpm --rebuilddb # 初始化时间较长 
```
#### 对系统软件的查询

1.  对系统软件的查询
```bash
$ rpm -q soft1 # 查询soft1 软件是否存在
$ rpm -qa # 查询系统已安装软件
```
2.  查询一个已经安装的文件属于哪个软件包
```bash
$ rpm -qf 文件名
```
>Tips : 文件名所在的绝对路径要指出 

3.  查询已安装软件包都安装到何处
```bash
$ rpm -ql 软件名 
$ rpmquery -ql 软件名
```
4. 查询一个已安装软件包的信息
```bash
$ rpm -qi 软件名
```
5. 查看一下已安装软件的配置文件 
```bash
$ rpm -qc 软件名
```
6. 查看一个已经安装软件的文档安装位置
```bash
$ rpm -qd 软件名
```
7. 查看一下已安装软件所依赖的软件包及文件
```bash
$ rpm -qR 软件名
```
8. 查询已安装软件的总结
```bash
$ rpm -qil 软件名
```

#### 对于未安装的软件包的查看
查看的前提是您有一个.rpm 的文件，也就是说对**既有软件**file.rpm的查看等；

1. 查看一个软件包的用途、版本等信息；
```bash
$ rpm -qpi file.rpm
```
2. 查看一件软件包所包含的文件
```bash
$  rpm -qpl file.rpm
```
3. 查看软件包的文档所在的位置
```bash
$ rpm -qpd file.rpm
```
4. 查看一个软件包的配置文件
```bash
$ rpm -qpc file.rpm
```
5. 查看一个软件包的依赖关系
```bash
$ rpm -qpR file.rpm
```

#### 软件包的安装、升级、删除等；

1. 安装和升级一个rpm 包
```bash
# i- install, v- 输出长信息, h- print hash, U- upgrade
$ rpm -ivh file.rpm # 注：这个是用来安装一个新的rpm 包
$ rpm -Uvh file.rpm # 注：这是用来升级一个rpm 包
$ rpm -ivh --replacepkgs file.rpm 
$ rpm -ivh --test file.rpm
$ rpm -Uvh --oldpackage file.rpm
$ rpm -ivh --relocate /=/opt/gaim file.rpm
```
>Tips： **`--replacepkgs`** 参数是以已安装的软件再安装一次；有时没有太大的必要；
>测试安装参数 **`--test`** ，用来检查依赖关系；并不是真正的安装；
>由新版本降级为旧版本，要加 **`--oldpackage`** 参数
>为软件包指定安装目录：要加 **`--relocate`** 参数

2. 删除一个rpm 包
```bash
$ rpm -e 软件包名 
$ rpm -e 软件包名 --nodeps
```
>Tips : 如果有依赖关系，您也可以用 **`--nodeps`** 忽略依赖的检查来删除。但尽可能不要这么做，最好用软件包管理器 **systerm-config-packages** 来删除或者添加软件


















