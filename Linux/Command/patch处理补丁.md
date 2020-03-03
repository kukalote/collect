Linux_cmd_patch处理补丁

#### 生成补丁
	$ diff -Naur passwd.old passwd.new > passwd.patch

#### 档案更新
	$ patch -pN < patch_file

#### 档案还原
	$ patch -R -pN < patch_file

#### 将刚刚制作出来的 patch file 用来更新旧版资料
	[dmtsai@study testpw]$ patch -p0 < passwd.patch
	patching file passwd.old

#### 恢复旧档案的内容
	[dmtsai@study testpw]$ patch -R -p0 < passwd.patch