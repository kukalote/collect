### vim 窗口操作

1. 只显示当前窗口

	方式 | 操作
	----|----
	快捷键 | `Ctl-W o`
	命令 | `:only` 简写(`:on`)
2. 隐藏当前窗口

	方式 | 操作
	----|----
	快捷键 | `Ctl-W c`
	命令 | `:close` 简写(`:clo`)
	命令 | `:hidden` 简写(`:hid`)

3. 关闭当前窗口

	方式 | 操作
	----|----
	快捷键 | `Ctl-W q`
	命令 | `:quit` 简写(`:q`)

4. 显示缓存中文件列表

	方式 | 操作
	----|----
	命令 | `:ls`
	命令 | `:files`
	命令 | `:buffers`
5. 打开缓存列表中的文件

	方式 | 操作
	----|----
	快捷键 | `N Ctl-^`
	命令 | `:Nbffer` 简写(`:Nb`)
	命令 | `:buffer %N` 简写(`:b %N`)
5. 删除缓存列表中的文件

	方式 | 操作
	----|----
	命令 | `:Nbdelete` 简写(`:Nbd`)
	命令 | `:bdelete N` 简写(`:bd N`)
7. 在缓存文件中跳转

	方式 | 操作 | 功能
	----|----|----
	命令 | `:bnext` 简写(`:bn`) | 下一个缓存文件
	命令 | `:bprevious` 简写(`:bp`) | 前一个缓存文件
	命令 | `:brewind` 简写(`:br`) | 第一个缓存文件
	命令 | `:bfirst` 简写(`:bf`) | 第一个缓存文件
	命令 | `:blast` 简写(`:bl`) | 最后一个缓存文件


