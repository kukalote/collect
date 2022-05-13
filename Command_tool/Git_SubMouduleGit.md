# sub module git 

```bash
# 添加子模块
git submodule add http://github.com/chaconinc/DbConnector
# 添加子模块后，正常commit,push即可将子模块绑定关系更新到版本库

#克隆含子模块的项目
git clone http://github.com/chaconinc/MainProject
cd MainProject
# 克隆后子模块文件夹中是空的，
#还需要初始化本地配置文件
git submodule init
# 和 从该项目中抓取所有数据并检出父项目中列出的合适的提交。
git submodule update --remote
# --remote 保证拉取子模块最新提交

# 简洁的递归克隆含子模块的项目
git clone --recursive http://github.com/chaconinc/MainProject
```

### 删除 Submodule

```shell
cd git-main

git rm --cached git-part1
rm -rf git-part1
# 编辑或者修改.gitmodules
rm .gitmodules
#更改git的配置文件config:
vim .git/config
```

可以看到Submodule的配置信息：

```shell
[submodule "git-part1"]
  url = git@github.com:jjz/git-part1.git
```

删除submodule相关的内容,然后提交到远程服务器:

```shell
git commit -a -m 'remove git-part1 submodule'
```



### 下载指定分支的版本库

```bash
# 下载指定版本代码
git clone -b dev http://github.com/chaconinc/MainProject

cd MainProject
# 加载指定分支的子模块
git submodule add -b dev http://github.com/chaconinc/git-part1.git

# --remote 保证拉取子模块最新提交
git submodule update --remote
```

