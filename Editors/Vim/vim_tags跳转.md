### vim tags跳转

使用ctags 生成标签后可以使用 `ctrl+]` , 跳转到相应的标签，不过有时候这种有效率的方式跳的地址并不准确。

但是可以使用 `:tselect`  和 `:tjump`  形式的命令  
还有 `g]`

下面介绍其他可以使用的命令吧

	    :tag 	 关键字（跳转到与“关键字”匹配的标记处）  
	    :tselect [关键字]（显示与“关键字”匹配的标记列表，输入数字跳转到指定的标记）
	    :tjump 	[关键字]（类似于“:tselect”，但当匹配项只有一个时直接跳转至标记处而不再显示列表）
	    :tn	（跳转到下一个匹配的标记处）
	    :tp	（跳转到上一个匹配的标记处）
	    Ctrl-]（跳转到与光标下的关键字匹配的标记处；除“关键字”直接从光标位置自动获得外，功能与“:tags”相同）
	    g]	（与“Ctrl-]”功能类似，但使用的命令是“:tselect”）
	    g Ctrl-]	（与“Ctrl-]”功能类似，但使用的命令是“:tjump”）
	    Ctrl-T	（跳转回上次使用以上命令跳转前的位置）