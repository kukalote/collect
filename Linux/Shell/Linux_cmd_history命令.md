### Linux History命令

#### History的保存

那么命令记录在哪里呢？在家目录内的 `.bash_history` 里！ 不过，需要留意的是，`~/.bash_history` 记录的是前一次登陆以前所运行过的命令，而至于这一次登陆所运行的命令都被缓存在内存中，当你成功的注销系统后，该命令记忆才会记录到 `.bash_history` 当中！

#### 调用历史命令

接下来学习history历史命令的用法。

代码如下：


	　　[root@jb51 Desktop]# history [n]
	　　[root@jb51 Desktop]# history [-c]
	　　[root@jb51 Desktop]# history [-raw] histfiles
	　　
	　　选项与参数：
		
		n  ：数字，意思是要列出最近的 n 条命令行表的意思！
		-c ：将目前的 shell 中的所有 history 内容全部消除
		-a ：将目前新增的 history 命令新增入 histfiles 中，若没有加 histfiles ，则默认写入 ~/.bash_history
		-r ：将 histfiles 的内容读到目前这个 shell 的 history 记忆中；
		-w ：将目前的 history 记忆内容写入 histfiles 中！

历史使用的窍门

1、!的使用

	1> !!		重复前一个命令
	2> !字符 	重复前一个以“字符”开头的命令
	3> !num 	按照history命令输出中的序号来重复对应命令
	4> !?abc 	重复前一个包含abc的命令
	5> ! -n 	重复n个命令之前的那个命令

2、组合键

1> 使用up和down键来上下浏览之前执行的命令

2> 键入 `ctr+r` 来在命令历史中搜索命令

　　代码如下：

	[root@jb51 Desktop]#
	(reverse-i-search)''：
	(reverse-i-search)'h'： cat /etc/shadow  
	按回车键执行该命令

3> 要重新调用前一个命令中的参数

	Esc+.（点击Esc键，然后点击.键）

　　注意：
　　`History` 保存在每个用户自己的历史记录中，位于用户的家目录中。
　　用户登录后，执行命令存放在内存中，只有登录后才能看到。


