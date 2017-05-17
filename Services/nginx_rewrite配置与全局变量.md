# nginx rewrite配置和全局变量

## Nginx正则匹配符说明

###1、正则表达式匹配，其中：

- `~` 为区分大小写匹配
- `~*` 为不区分大小写匹配
- `!~` 和 `!~*` 分别为区分大小写不匹配及不区分大小写不匹配

###2、文件及目录匹配，其中：

- `-f` 和 `!-f` 用来判断是否存在文件
- `-d` 和 `!-d` 用来判断是否存在目录
- `-e` 和 `!-e` 用来判断是否存在文件或目录
- `-x` 和 `!-x` 用来判断文件是否可执行

如：

	if (!-f $request_filename) {
		proxy_pass  http://127.0.0.1;
	}
## 2. flag标记有：

- `last` 终止继续在本 location 块中处理接收到的 URI，并将此处重写的 URI 作为一个新的 URI，使用各 location 块进行处理。该标志将重写后的 URI 重新在 server 块中执行，为重写后的 URI 提供了转入到其他 location 块的机会。
- `break` 将此处重写的 URI 作为一个新的 URI，在本块中继续进行处理。该标志将重写后的地址在当前的 location 块中执行，不会将新的 URI 转向到其他 location。
- `redirect` 将重写后的 URI 返回给客户端，状态代码为 302,指明是临时重定向 URI，主要用在 replacement 变量不是以 “http://” 或者 “https://” 开头的情况下。
- `permanent` 将重写后的 URI 返回给客户端，状态代码为 301，指明是永久重定向 URI。

## 3. Nginx全局变量

一些可用的全局变量有，可以用做条件判断(待补全) :

变量名称 | 解释
---|---
`$args_PARAMETER` | 客户端请求中 PARAMETER 的值
`$args` | 请求中的参数
`$binary_remote_addr` | 远程地址的二进制表示
`$body_bytes_sent` | 已发送的消息体字节数
`$content_length` | http 请求中的 "`Content-Length`"
`$content_type` | 请求中的 "`Content-Type`"
`$cookie_COOKIE` | 客户端请求中 COOKIE 头域的值
`$document_root` | 针对当前请求的根路径设置值
`$document_uri` | 与 $uri 相同
`$host` | 请求中的 "host";如果请求中没有 host 行， 则等于设置的服务器名
`$http_HEADER` | HTTP 请求信息里的 HEADER 字段
`$http_host` | 与 `$host` 相同， 但如果请求信息中没有 Host 行,则可能不同
`$http_cookie` | 客户端的 cookie 信息
`$http_referer` | 引用地址
`$http_user_agent` | 客户端代理信息
`$http_via` | 最后一个访问服务器的 IP 地址
`$http_x_forwarded_for` | 相当于网络访问路径
`$is_args` | 如果 `$args` 有值，则等于 "`?`";否则等于空
`$limit_rate` | 对链接速率的限制
`$nginx_version` | 当前 Nginx 服务器版本
`$pid` | 当前 Nginx 服务器主进程的进程 ID
`$query_string` | 与 `$args` 相同
`$remote_addr` | 客户端 IP 地址
`$remote_port` | 客户端接口
`$remote_user` | 客户端用户名，认证用
`$request` | 客户端请求
`$request_body` | 客户端请求的报文
`$request_body_file` | 发往后端服务器的本地临时缓存文件名称
`$request_filename` | 当前请求文件路径名,由 root 或 alias 指令与 URI 请求生成
`$request_method` | 请求方法，比如 "POST","GET"
`$request_uri` | 请求的 URI,带参数
`$scheme` | 所用的协议，比如 http 或者 https,比如 `rewrite^(.+)$$scheme://mysite.name$1redirect`
`$sent_http_cache_control` |
`$sent_http_connection` |
`$sent_http_content_length` |
`$sent_http_content_type` |
`$sent_http_keep_alive` |
`$sent_http_last_modified` |
`$sent_http_location` |
`$sent_http_transfer_encoding` |
`$server_addr` | 服务器地址，如果没有用 listen 指明服务器地址，使用这个变量将发起一次系统调用来获得地址(造成资源浪费)
`$server_port` | 请求到达的服务器端口
`$server_protocol` | 请求的协议版本,"`HTTP/1.0`" 或 "`HTTP/1.1`"
`$server_name` | 服务器名称
`$uri` | 请求的 URI，可能和最初的值有不同，比如经过重定向之类的

## 4. 各式各样的 rewrite 示例

常用几个Nginx rewrite配置实例：

**Wordpress的重定向规则**：

	if (!-e $request_filename) {
		rewrite ^/(index|atom|rsd)\.xml$ http://feed.shunz.net last;
		rewrite ^([_0-9a-zA-Z-]+)?(/wp-.*) $2 last;
		rewrite ^([_0-9a-zA-Z-]+)?(/.*\.php)$ $2 last;
		rewrite ^ /index.php last;
	}

**Discuz!的重定向规则**：

	if (!-f $request_filename) {
		rewrite ^/archiver/((fid|tid)-[\w\-]+\.html)$   /archiver/index.php?$1 last;
		rewrite ^/forum-([0-9]+)-([0-9]+)\.html$   /forumdisplay.php?fid=$1&page=$2 last;
		rewrite ^/thread-([0-9]+)-([0-9]+)-([0-9]+)\.html$  /viewthread.php?tid=$1&extra=page%3D$3&page=$2 last;
		rewrite ^/space-(username|uid)-(.+)\.html$   /space.php?$1=$2 last;
		rewrite ^/tag-(.+)\.html$ /tag.php?name=$1 last;
	}


**多目录转成参数**

要求：abc.domian.com/sort/2 => abc.domian.com/index.php?act=sort&name=abc&id=2

规则配置：

	if ($host ~* (.*)\.domain\.com) {
		set $sub_name $1;
		rewrite ^/sort\/(\d+)\/?$ /index.php?act=sort&name=$sub_name&id=$1 last;
	}

**目录对换**

要求：/123456/xxxx -> /xxxx?id=123456

规则配置：

	rewrite ^/(\d+)/(.+)/ /$2?id=$1 last;



**再来一个针对浏览器优化的自动rewrite，这里rewrite后的目录可以是存在的；**

例如设定nginx在用户使用ie的使用重定向到`/nginx-ie`目录

规则如下：

	if ($http_user_agent ~ MSIE) {
		rewrite ^(.*)$ /nginx-ie/$1 break;
	}

**目录自动加“/”** ，这个功能一般浏览器自动完成

	if (-d $request_filename){
		rewrite ^/(.*)([^/])$ http://$host/$1$2/ permanent;
	}

以下这些可能就跟广义的rewrite重写无关了

**禁止htaccess**

	location ~/\.ht {
		deny all;
	}

**禁止多个目录**

	location ~ ^/(cron|templates)/ {
		deny all; break;
	}

**禁止以/data开头的文件**，可以禁止/data/下多级目录下.log.txt等请求

	location ~ ^/data {
		deny all;
	}

**禁止单个文件**

	location ~ /data/sql/data.sql {
		deny all;
	}

**给favicon.ico和robots.txt设置过期时间**; 这里为favicon.ico为99天,robots.txt为7天并不记录404错误日志

	location ~(favicon.ico) {
		log_not_found off;
		expires 99d;
		break;
	}

	location ~(robots.txt) {
		log_not_found off;
		expires 7d;
		break;
	}

**设定某个文件的浏览器缓存过期时间;这里为600秒**，并不记录访问日志

	location ^~ /html/scripts/loadhead_1.js {
		access_log off;
		expires 600;
		break;
	}

**Nginx还可以自定义某一类型的文件的保质期时间**，具体写法看下文的代码：

	location ~* \.(js|css|jpg|jpeg|gif|png|swf)$ {
		if (-f $request_filename) {
			expires    1h;
			break;
		}
	}
	//上段代码就将js|css|jpg|jpeg|gif|png|swf这类文件的保质期设置为一小时。


**防盗链的设置**：

防盗链：如果你的网站是个下载网站，下载步骤应该是先经过你的主页找到下载地址，才能下载，为了防止某些网友直接访问下载地址完全不通过主页下载，我们就可以使用防盗链的方式，具体代码如下：

	location ~* \.(gif|jpg|swf)$ {
		//valid_referers none blocked start.igrow.cn sta.igrow.cn;
		if ($invalid_referer) {
			rewrite ^/ http://$host/logo.png;
		}
	}


**文件反盗链并设置过期时间**--<盗链多次请求也会打开你的站点的图片啊，所以设置下缓存时间，不会每次盗链都请求并下载这张图片>

	location ~* ^.+\.(jpg|jpeg|gif|png|swf|rar|zip|css|js)$ {
		//valid_referers none blocked *.jjonline.cn *.jjonline.com.cn *.lanwei.org *.jjonline.org localhost  42.121.107.189;

		if ($invalid_referer) {
			rewrite ^/ http://img.jjonline.cn/forbid.gif;
			return 417;
			break;
		}

		access_log off;
		break;
	}

说明：

> 这里的return 417 为自定义的http状态码，默认为403，方便通过nginx的log文件找出正确的盗链的请求地址
> “rewrite ^/ http://img.jjonline.cn/forbid.gif;”显示一张防盗链图片
> “access_log off;”不记录访问日志，减轻压力
> “expires 3d”所有文件3天的浏览器缓存


**只充许固定ip访问网站，并加上密码；这个对有权限认证的应用比较在行**

	location \ {
		allow 22.27.164.25; #允许的ipd
		deny all;
		auth_basic “KEY”; #认证的一些设置
		auth_basic_user_file htpasswd;
	}

> 说明：location的应用也有各种变化，这里的写法就针对了根目录了。

**文件和目录不存在时的重定向**

	if (!-e $request_filename) {
		#proxy_pass http://127.0.0.1; #这里是跳转到代理ip，这个代理ip上有一个监听的web服务器
		rewrite ^/ http://www.jjonline.cn/none.html;  #跳转到这个网页去
		#return 404; #直接返回404码，然后会寻找root指定的404.html文件
	}

**域名跳转**

	server {
		listen 80;

		server_name jump.jjonline.cn ;#需要跳转的多级域名
		index index.html index.htm index.php; #入口索引文件的名字
		root /var/www/public_html/; #这个站点的根目录
		rewrite ^/ http://www.jjonline.cn/;

		#rewrite到这个地址，功能表现：在浏览器上输入jump.jjonline.cn并回车，不会有任何提示直接变成www.jjonline.cn
		access_log off;
	}

**多域名转向**

	server {
		listen 80;

		server_name www.jjonline.cn www.jjonline.org;
		index index.html index.htm index.php;
		root /var/www/public_html/;
		if ($host ~ “jjonline\.org”) {
			rewrite ^(.*) http://www.jjonline.cn$1 permanent;
		}
	}

**三级域名跳转**

	if ($http_host ~* “^(.*)\.i\.jjonline\.cn$”) {
		rewrite ^(.*) http://demo.jjonline.cn$1;
		break;
	}

**域名镜向**

	server {
		listen 80;
		server_name mirror.jjonline.cn;
		index index.html index.htm index.php;
		root /var/www/public_html;
		rewrite ^/(.*) http://www.jjonline.cn/$1 last;
		access_log off;
	}

**某个子目录作镜向,这里的示例是demo子目录**

	location ^~ /demo {
		rewrite ^.+ http://demo.jjonline.cn/ last;
		break;
	}

**以下在附带本博客的rewrite写法，emlog系统的rewrite**

	location ~ {
		if (!-e $request_filename) {
			rewrite ^/(.+)$ /index.php last;
		}
	}