### SVN 命令概览

#### 查看远程分支
```bash
$ svn ls https://svn.xxx.com/apt.xxx.com/branches -v
```
#### 查看不同版本间修改文件列表
```bash
$ svn diff -r 113551:114444 --summarize
```
#### 查看当前 svn 的用户与密码
```bash
$ vim ~/.subversion/auth/svn.simple/xxx
```
#### 查看某人提交历史记录
```bash
$ svn log -l 20 -v| sed -n '/xunyalong/,/-----$/ p'
```
#### 查看分支起始版本
```bash
$ svn log -v --stop-on-copy
```
#### 将主干代码更新至分支
```bash
# 分支起始版本假定为 100
$ cd branches_dir
$ svn merge -r 100:HEAD https://svn.xxx.com/apt.xxx.com/trunk
```