## 跳转

#### 跳转局部变量声明处
`gd`

#### 视图[界面]移动
操作 | 作用
----|----
`zz` | 当前行移至视图中部
`zt` | 当前行移至视图顶部
`zb` | 当前行移至视图底部




## 修改

#### 更正
编辑模式下即可操作

快捷键 | 解释
----|----
`CTRL-W` | 向前删除单词
`CTRL-U` | 向前删除当前行

#### 补全
编辑模式下即可操作

快捷键 | 解释
----|----
`CTRL-X CTRL-F` | 文件名
`CTRL-X CTRL-L` | 整行
`CTRL-X CTRL-D` | 宏定义 (包括包含文件里的)
`CTRL-X CTRL-I` | 当前文件以及所包含的文件
`CTRL-X CTRL-K` | 字典文件内的单词
`CTRL-X CTRL-T` | 同义词词典文件内的单词
`CTRL-X CTRL-]` | 标签
`CTRL-X CTRL-V` | Vim 命令行
`CTRL-X CTRL-Y` | 编辑模式下的向下滚动
`CTRL-X CTRL-E` | 编辑模式下的向上滚动

#### 重复插入
按 `CTRL-A`,编辑器就把你上次在插入模式下输入的文本再输入一次。
按 `CTRL-2`,其实是 `CTRL-@`,编辑器就把你上次在插入模式下输入的文本再输入一次, 并调回命令模式。

#### 从另一行拷贝
编辑模式下，
`CTRL-Y` 插入光标上方的字符
`CTRL-E` 插入光标下方的字符

#### 重复替换
在执行一次替换后，在命令行状态输入 `&` 命令,可重复前一次替换

## 扩展

#### 自动纠正,单词替换
`:abbr pn penguin`

#### 查看已经安装的插件列表
`:scriptnames`

#### 插件新安装后导入帮助文档
>重新打开 vim, 运行命令 `:helptags ~/.vim/bundle/nerdtree/doc/`, 然后就可以摸查了 `:help NERD_tree.txt`.

#### 格式化代码行
操作 | 作用
----|----
`==` | 将当前行(或选中行)进行自动格式对齐
`<<` | 操作行向左移动一shiftwidth
`>>` | 操作行向右移动一shiftwidth



#### 多个文件查找
`:grep key_word dir_name`

相关操作命令

操作指令 | 作用
----|---
`:cn` | 下一个匹配
`:cp` | 上一个匹配
`:cl` | 显示匹配列表

#### 创建标签页
`:tabnew`

相关操作命令

操作指令 | 作用
----|---
`:[count]tabnew` | 在当前第count标签后创建新空白标签
`:[count]tabe[dit]` | -
`:tabc[lose][!] {count}` | 关闭某个标签页
`:tabo[nly][!]` | 关闭所有其他标签
----|----
`:tabs` | 标签列表
----|----
`:tabn[ext] {count}` | 切换至左侧某个标签
`{count}` | -
`{count}gt` | -
`:tabp[revious] {count}` | 切换至右侧某个标签
`:tabN[ext] {count}` | -
`{count}` | -
`{count}gT` | -
`:tabfir[st]` | 第一个标签页
`:tabr[ewind]` | -
`:tabl[ast]` | 最后一个标签页
----|----
`:tabm[ove] [N]` | 将标签移至N位,以0开始
`:[N]tabm[ove]` | -
`:tabm[ove] +[N]` | 将标签右移N位
`:tabm[ove] -[N]` | 将标签左移N位

