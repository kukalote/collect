# Git_环境变量设置

<!-- create time: 2016-03-11 08:58:22  -->

<!-- This file is created from $MARBOO_HOME/.media/starts/default.md
本文件由 $MARBOO_HOME/.media/starts/default.md 复制而来 -->



### `git config` 选项如下 : 

选项 | 含义
----|---
`--replace-all` | 
`--add` | 添加一个新选项,如果存在没有提示
`--get` | 获取指定选项的值
`--get-all` | 很像 get 选项, 只是没有不存在的错误提示
`--get-regexp` | 很像 get-all 选项, 是以正则表达式来选取键名, 大小写敏感
`--get-urlmatch name URL` | 
`--global` | 当前用户的版本库设置
`--system` | 系统级的版本库设置
`--local` | 某一版本库设置
`-f config-file 或 --file config-file` | 使用指定文件作为配置文件
`--blob blob` | 与 file 选项类似, 只是用二进制文件取代,可以使用 *master::gitmodules* 来阅读此文件中的值。
`--remove-section` | 从配置文件移除指定模块数据
`--rename-section` | 为模块重新命名
`--unset` | 移除匹配选项的行
`--unset-all` | 移除所有匹配键值的选项
`-l 或 --list` | 列出所有配置文件中的值
`--bool` | 返回 true 或 false 
`--int` | 返回数值
`--bool-or-int` | 
`--path` | 此选项会将 ~ 转换为 $HOME,如:~/user 转换为指定用户目录.但如果使用绝对地址则无作用。
`-z 或 --null` | 在键值之间添加换行符
`--name-only` | 只输出值对应的选项名(作用于 --list 和 --get-regexp)
`--get-colorboolname [stdout-is-tty]` | 
`--get-color name [default]` | 
`-e 或 --edit` | 用编辑器打开配置文件进行修改
`--[no-]includes` | 在查找值时, 注意配置文件中的 include.* 指令.默认开启。

### 重要文件

关于 git 的配置文件,加载顺序如下

配置文件地址 | 命令选项 | 作用
---|---|---
`$(prefix)/etc/gitconfig` | `--system` | 是系统上用户git仓库的通用配置文件
`~/.gitconfig` 或 `$HOME/.config/git/config` | `--global` | 只针对当前用户
`$GIT_DIR/config`(Git 仓库目录中的config文件) | `--local` | 针对该仓库



### git config 配置选项

模块 | 选项 | 值与效果
----|----|---
alias | * | 为命令起别名 "alias.last=cat-file commit HEAD"
commit | template | 此项指定为你系统上的一个文件，当你提交的时候， Git 会默认使用该文件定义的内容。
color | * | 想要具体到哪些命令输出需要被着色以及怎样着色或者 Git 的版本很老，你就要用到和具体命令有关的颜色配置选项，它们都能被置为true、false或always (color.branch, color.diff, color.interactive ,color.status)
color | ui | Git会按照你需要自动为大部分的输出加上颜色,设置为true来打开所有的默认终端着色。
core | editor | 这种方式会启动文本编辑器以便输入本次提交的说明
core | excludesfile | 添加到 .gitignore(每个目录) 和 .git/info/exclude
core | pager | core.pager指定 Git 运行诸如log、diff等所使用的分页器，你能设置成用more或者任何你喜欢的分页器（默认用的是less）
credential | helper | store(保存密钥-代码上传服务器时用)
help | autocorrect | 如果你把help.autocorrect设置成1,启动自动修正,那么在只有一个命令被模糊匹配到的情况下,Git 会自动运行该命令。
merge | tool | 合并用工具 (比如 vimdiff)
user | email | 提交时用的新email会被记录,也可以被 GIT_AUTHOR_EMAIL, GIT_COMMITTER_EMAIL 和 GIT_COMMITTER_NAME 环境变量覆盖。 
user | name | 提交时记录的名字, 可以被 GIT_AUTHOR_NAME 和 GIT_COMMITTER_NAME 环境变量覆盖。
user | signingkey | 如果你要创建经签署的含附注的标签,那么把你的GPG签署密钥设置为配置项会更好,设置密钥ID

>你能设置的颜色值如：normal、black、red、green、yellow、blue、magenta、cyan、white;  
>正如以上例子设置的粗体属性，想要设置字体属性的话，可以选择如：bold、dim、ul、blink、reverse。

**示例 :** 

```bash
$ git config --global merge.tool vimdiff
$ git config --global alias.co checkout 
$ git config --global alias.br branch 
$ git config --global alias.ci commit 
$ git config --global alias.st status
$ git config --global user.signingkey <gpg-key-id>
$ git config --global color.ui true
$ git config --global color.diff.meta “blue black bold”
```

**.gitconfig文件配置推荐**

```bash
	[user]
		name = username
		email = xxx.example.com
	[alias]
		co = checkout
		ci = commit
		st = status
		df = diff
		br = branch
	[color]
		ui = true
	[credential]
	# 密码短期保存
		helper = cache
	# 密码长期保存
	# helper = store
	[core]
    quotepath = false
    # 设置全局忽略文件
    excludesfile = ~/.gitignore
```

