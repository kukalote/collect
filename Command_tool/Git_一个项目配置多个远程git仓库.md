# Git_一个项目配置多个远程git仓库

由于长期国内网络被墙，导致开发连接外网的github，又慢又不稳定。在此面向积极，来做了多个git仓库的配置组合。平时不用翻墙，只需要在国内的gitee上推，偶尔翻墙，又可以把这段时间积攒的代码推送到github。

这里就考虑使用一个项目，配置多个远程git仓库了。两个仓库操作注定比单个操作会麻烦一些，而且关于代码同步【gitee有同步github代码功能】也可能会遇到一些不可预测的问题。但是优点也很明显，多一份代码冗余，不用受限github被墙了。

那么现在就开始了，下面会列出一些常用操作，

## 初始化目录

**情况一：初始化目录空白，在目录中开发项目**

```sh
mkdir test
cd test
git init 
touch README.md
git add README.md
git commit -m "first commit"
# 下面的 origin 是自定义单词，可以改为如 github 之类
git remote add origin https://github.com/xxxx/test.git
git push -u origin "master"
```

**情况2：已有内容的目录**

```sh
cd noEmptyDir
git remote add origin https://github.com/xxxx/test.git
git push -u origin "master"
```

## 添加另一个远程仓库

```sh
cd gitExist
git remote add gitee git@gitee.com:xxxx/test.git
git push -u gitee "master"
```

## 查看本地项目远程仓库列表

```sh
git remote -v
```

## 删除本地库绑定的远程地址

```sh
git remote remove origin
git remote remove gitee
```

## 分支推送

```sh
git push origin master
git push gitee master
```

## 分支拉取

```sh
git pull origin master
git pull gitee master
```

## 将两个远程地址绑到一个仓库上，打包推送【推送一个未使用过的远程仓库】

```sh
# 进入一个已有版本信息的目录
cd gitExist
# 在 origin 上绑定另一个远程版本库【这个版本库没有任何提交记录，以防止与目前之前配置的版本库发生冲突】
git remote set-url --add origin git@gitee.com:xxx/merge-pros.git
git remote -v
#origin  git@gitee.com:xxx/git-exist.git (fetch)
#origin  git@gitee.com:xxx/git-exist.git (push)
#origin  git@gitee.com:xxx/merge-pros.git (push)
# 可以看出当前目录以git-exist.git来更新本地代码；而push 到两个远程仓库

git push origin master
# 向两个地址送代码
```

## 将两个远程地址绑到一个仓库上，打包推送【推送一个已使用过的远程仓库】

```sh
cd gitPart1

git remote add git-part2 git@gitee.com:xxx/git-part2.git

git remote -v
# git-part2       git@gitee.com:xxx/git-part2.git (fetch)
# git-part2       git@gitee.com:xxx/git-part2.git (push)
# origin  git@gitee.com:xxx/git-part1.git (fetch)
# origin  git@gitee.com:xxx/git-part1.git (push)

git pull git-part2 master
# fatal: refusing to merge unrelated histories
# 不可以合并不同没有相同结点的分支，如果需要合并两个不同结点的分支，那么需要在git pull添加一句代码--allow-unrelated-histories。
#git pull git-part2 master --allow-unrelated-histories

# 有必要的话，在这里合并一下，git-part1 与 git-part2 的代码
git push origin master
git push git-part2 master

# 现在两个版本库有共同相交的结点后，便有捆绑推送的基础了：下面将git-part2的远程地址也绑定到 origin,以实现一次推送，两地更新的效果
git remote remove git-part2
git remote set-url --add  origin git@gitee.com:xxx/git-part2.git
git remote -v
# origin  git@gitee.com:xxx/git-part1.git (fetch)
# origin  git@gitee.com:xxx/git-part1.git (push)
# origin  git@gitee.com:xxx/git-part2.git (push)

# 推送测试-我是成功了，不知道你结果怎么样
echo 'merge pros new'> mergeTest.txt
git ci -m 'new merge pros' .
git push origin master
```

