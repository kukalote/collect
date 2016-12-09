### SVN 命令概览

#### 查看远程分支

	$ svn ls https://svn.xxx.com/apt.xxx.com/branches -v

#### 查看不同版本间修改文件列表

	$ svn diff -r 113551:114444 --summarize
#### 查看当前 svn 的用户与密码

	$ vim ~/.subversion/auth/svn.simple/xxx

#### 查看某人提交历史记录

	$ svn log -l 20 -v| sed -n '/xunyalong/,/-----$/ p'