Git 常用命令及配置

## 配置部分
1. 用户账号及密码保存
```
	git config --global credential.helper cache
	或
	git config --global credential.helper store
```

2. 配置用户账号密码
```
	git config --global user.email '454769906@qq.com'
	git config --global user.name 'kukalote'
```
3. 配置文件
```
	[user]
		name = kukalote
		email = 454769906@qq.com
	[alias]
		co = checkout
		ci = commit
		st = status
		df = diff
		br = branch
	[color]
		ui = true
	[credential]
		helper = cache
```