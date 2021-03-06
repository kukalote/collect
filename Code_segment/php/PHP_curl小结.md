PHP_curl小结
```php
<?php
	$ch = curl_init($url);
	$headers = array(
		"Content-type: text/xml;charset=\"utf-8\"",
		"Accept: text/html, application/xml;q=0.9, application/xhtml+xml, image/png, image/webp, image/jpeg, image/gif, image/x-xbitmap, */*;q=0.1",
		"Cache-Control: no-cache",
		"Pragma: no-cache"
	);
	curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_ENCODING, "gzip");
	curl_setopt($ch, CURLOPT_FOLLOWLOCATION,1); //是否抓取跳转后的页面
	$response = curl_exec($ch);
```

### options选项参数详解 :

`CURLOPT_HEADER`：设为1，则在返回的内容里包含**http header**；

`CURLOPT_FOLLOWLOCATION`：设为**0**，则不会自动**301**，**302**跳转；

`CURLOPT_INFILESIZE`: 当你上传一个文件到远程站点，这个选项告诉**PHP**你上传文件的大小。

`CURLOPT_VERBOSE`: 如果你想**CURL**报告每一件意外的事情，设置这个选项为一个非零值。

`CURLOPT_HEADER`: 如果你想把一个头包含在输出中，设置这个选项为一个非零值。

`CURLOPT_NOPROGRESS`: 如果你不会**PHP**为**CURL**传输显示一个进程条，设置这个选项为一个非零值。

> **注意：PHP自动设置这个选项为非零值，你应该仅仅为了调试的目的来改变这个选项。*

`CURLOPT_NOBODY`: 如果你不想在输出中包含**body**部分，设置这个选项为一个非零值。

`CURLOPT_UPLOAD`: 如果你想让**PHP**为上传做准备，设置这个选项为一个非零值。

`CURLOPT_FTPLISTONLY`: 设置这个选项为非零值，**PHP**将列出**FTP**的目录名列表。

`CURLOPT_FTPAPPEND`: 设置这个选项为一个非零值，**PHP**将应用远程文件代替覆盖它。

`CURLOPT_MUTE`: 设置这个选项为一个非零值，**PHP**对于**CURL**函数将完全沉默。

`CURLOPT_TIMEOUT`: 设置一个长整形数，作为最大延续多少秒。

`CURLOPT_LOW_SPEED_LIMIT`: 设置一个长整形数，控制传送多少字节。

`CURLOPT_RESUME_FROM`: 传递一个包含字节偏移地址的长整形参数，(你想转移到的开始表单)。

`CURLOPT_FAILONERROR`: 如果你想让**PHP**在发生错误(**HTTP**代码返回大于等于**300**)时，不显示，设置这个选项为一人非零值。默认行为是返回一个正常页，忽略代码。

`CURLOPT_POST`: 如果你想PHP去做一个正规的**HTTP POST**，设置这个选项为一个非零值。这个**POST**是普通的 `application/x-www-from-urlencoded` 类型，多数被**HTML**表单使用。

`CURLOPT_NETRC`: 设置这个选项为一个非零值，PHP将在你的 `~./netrc` 文件中查找你要建立连接的远程站点的用户名及密码。

`CURLOPT_FOLLOWLOCATION`: 设置这个选项为一个非零值(象 “**Location:** “)的头，服务器会把它当做**HTTP**头的一部分发送(注意这是递归的，**PHP**将发送形如 “**Location:** “的头)。

`CURLOPT_PUT`: 设置这个选项为一个非零值去用**HTTP**上传一个文件。要上传这个文件必须设置**CURLOPT_INFILE**和**CURLOPT_INFILESIZE**选项.

`CURLOPT_LOW_SPEED_TIME`: 设置一个长整形数，控制多少秒传送**CURLOPT_LOW_SPEED_LIMIT**规定的字节数。

`CURLOPT_SSLVERSION`: 传递一个包含**SSL**版本的长参数。默认**PHP**将被它自己努力的确定，在更多的安全中你必须手工设置。

`CURLOPT_TIMECONDITION`: 传递一个长参数，指定怎么处理**CURLOPT_TIMEVALUE**参数。你可以设置这个参数为 **TIMECOND_IFMODSINCE** 或 **TIMECOND_ISUNMODSINCE**。这仅用于**HTTP**。

`CURLOPT_TIMEVALUE`: 传递一个从 `1970-1-1` 开始到现在的秒数。这个时间将被 **CURLOPT_TIMEVALUE** 选项作为指定值使用，或被默认 **TIMECOND_IFMODSINCE** 使用。


#### 下列选项的值将被作为字符串：

`CURLOPT_USERPWD`: 传递一个形如`[username]:[password]`风格的字符串,作用 **PHP** 去连接。

`CURLOPT_POSTFIELDS`: 传递一个作为 **HTTP** “**POST**”操作的所有数据的字符串。

`CURLOPT_REFERER`: 在 **HTTP** 请求中包含一个” **referer** ”头的字符串。

`CURLOPT_USERAGENT`: 在 **HTTP** 请求中包含一个”**user-agent**”头的字符串。

`CURLOPT_COOKIE`: 传递一个包含 **HTTP cookie** 的头连接。

`CURLOPT_SSLCERT`: 传递一个包含 **PEM** 格式证书的字符串。

`CURLOPT_SSLCERTPASSWD`: 传递一个包含使用 **CURLOPT_SSLCERT** 证书必需的密码。

`CURLOPT_PROXYUSERPWD`: 传递一个形如`[username]:[password]` 格式的字符串去连接HTTP代理。

`CURLOPT_URL`: 这是你想用 PHP 取回的URL地址。你也可以在用`curl_init()`函数初始化时设置这个选项。

`CURLOPT_RANGE`: 传递一个你想指定的范围。它应该是”X-Y”格式，X或Y是被除外的。**HTTP** 传送同样支持几个间隔，用逗句来分隔`(X-Y,N-M)`。

`CURLOPT_FTPPORT`: 传递一个包含被ftp “**POST**”指令使用的IP地址。这个 **POST**指令告诉远程服务器去连接我们指定的 **IP** 地址。这个字符串可以是一个 **IP** 地址，一个主机名，一个网络界面名(在UNIX下)，或是‘-’(使用系统默认IP地址)。

**`CURLOPT_COOKIEFILE`**: 传递一个包含 **cookie** 数据的文件的名字的字符串。这个 **cookie** 文件可以是 **Netscape** 格式，或是堆存在文件中的HTTP风格的头。

**`CURLOPT_CUSTOMREQUEST`**: 当进行 **HTTP** 请求时，传递一个字符被 **GET** 或 **HEAD** 使用。为进行 **DELETE** 或其它操作是有益的，更Pass a string to be used instead of GET or HEAD when doing an HTTP request. This is useful for doing or another, more obscure, HTTP request.


>  注意: 在确认你的服务器支持命令先不要去这样做。

#### 下列的选项要求一个文件描述(通过使用fopen()函数获得)：

**`CURLOPT_FILE`**: 这个文件将是你放置传送的输出文件，默认是**STDOUT**.

**`CURLOPT_INFILE`**: 这个文件是你传送过来的输入文件。

**`CURLOPT_WRITEHEADER`**: 这个文件写有你输出的头部分。

**`CURLOPT_STDERR`**: 这个文件写有错误而不是stderr。


