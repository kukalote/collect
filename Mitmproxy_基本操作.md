### mitmproxy 基本操作

这里主要讲基本使用方法，所以就不累述其安装过程了。

`mitmproxy 库文件地址 ： /usr/local/lib/python2.7/dist-packages/mitmproxy/`

1. **进入操作界面**
```
	$ mitmprox -b localIP -p specialPort
	//这里登录用的是本机的ip和指定的端口号(只要手机端也用此端口号便可)
```

2. **连接手机端**
	进入后在手机端设定同网段的代理，手机代理ip指向安装mitmproxy的电脑，并且使用上面指定的端口号即可。
	此时手机访问网络时的数据就可以在电脑端的mitmproxy界面上看到了(mitmproxy只支持http[s]协议)。
3. **移动操作**
	其操作方法类似 vim,`h`,`j`,`k`,`l`。
	如果已点进一条请求，切换标签用 `tab`。选择上一个请求用`p`,而跳转至下一个用 `空格`。
4. **进入与退出**
	过选或是进用的是`回车`,退出多用`q`和`Esc`。
5. **调整数据格式**
	如果已点进一条请求, 可以用 `m` 键来选择需要展示内容的形式（例如:J[s]on, [h]tml等）。
6. **编辑请求**
	如果已点进一条请求, 需要调整请求数据（例如:cookie,url,form等）,可以点击 `e`,来选择编辑内容。
	当光标移至需要编辑内容处，`回车` 进行编辑，`Esc` 退出编辑模式。
	>这里还有一个命令 `V`，用于恢复修改后的请求。
	>编辑参数时可以用 `a` 添加参数， `d` 删除参数， `e` 编辑参数。

7. **重新请求**
	可以使用 `r`，对当前请求进行刷新。不管是在请求列表，还是在详情。

8. **显示请求日志**
	在请求列表页，点 `e` 可以显示日志列表，使用 `tab` 可以切换日志与请求列表。
	>如果关闭日志列表可以再次使用 `e`。

9. **拦截请求**
	在请求列表点 `i`， 可以输入拦截条件。如果请求被拦截时，可以使用 `a` (accept) 将请求发出。服务器响应后,又拦截response,编辑完成后`shfit+q`,出现冒号:再`wq`+`回车`,退出编辑,`a`确认最终的response。

10. **禁用缓存**
	可以在使用 mitmproxy 命令时加上 `--anticache`， 也可以使用选项命令 `o`，选择 `a` 禁用缓存。
	> 原理是去除了头信息里的 `if-none-match` 项和 `if-modified-since`,所以由此引发的 `304  not modified` 将不会被启用。

11. **过滤表达式**
	在 mitmproxy 和 mitmdump 里用到的过滤表达式 :
	Expression 	|Description
	----|----
	`~a` 	|Match asset in response: CSS, Javascript, Flash, images.
	`~b regex` 	|Body
	`~bq regex` 	|Request body
	`~bs regex` 	|Response body
	`~c int` 	|HTTP response code
	`~d regex` 	|Domain
	`~dst regex` 	|Match destination address
	`~e` 	|Match error
	`~h regex` 	|Header
	`~hq regex` 	|Request header
	`~hs regex` 	|Response header
	`~m regex` 	|Method
	`~q` 	|Match request with no response
	`~s` 	|Match response
	`~src regex` 	|Match source address
	`~t regex` 	|Content-type header
	`~tq regex` 	|Request Content-Type header
	`~ts regex` 	|Response Content-Type header
	`~u regex` 	|URL
	`!` 	|unary not
	`&` 	|and
	`|` 	| `or`
	`(...)` 	| grouping

	> - Regexes are Python-style
	>- Regexes can be specified as quoted strings
	>- Header matching (~h, ~hq, ~hs) is against a string of the form “name: value”.
	>- Strings with no operators are matched against the request URL.
	>- The default binary operator is &.

    例子 :
	>- URL containing “google.com”:
	  ```google\.com```
	>- Requests whose body contains the string “test”:
	  ```~q ~b test```
	>- Anything but requests with a text/html content type:
	  ```!(~q & ~t "text/html")```




