#Nginx_自定义服务日志

`Nginx` 日志分为访问日志和错误日志。在**全局块**，**http 块**和 **server 块**中都可以对 Nginx 服务器的日志进行相关配置。

## 配置错误日志

使用的指令是 `error_log`,其语法结构是 :

	error_log file | stderr [debug | info | notice | warn | error | crit | alert | emerg];

Nginx 的错误日志支持输出到某一个固定文件 file;也可以输出到标准错误输出 stderr。

日志的错误级别是可选的, 由低到高分为 debug, info, notice, warn, error, crit, alert, emerg 等。
>需要注意的是,设置某一级别后，比这一级别高的日志也会被记录。

>此指令可以在 **全局块**, **http 块**, **server 块** 或者 **location 块** 中进行设置。
示例 :

	error_log logs/error.log error;

## 服务日志

服务日志与常规的不同，它是指记录 Nginx 服务器提供服务过程应答前端请求的日志，我们将其称为**服务日志**。

Nginx 服务器支持对服务日志的格式，大小，输出等进行配置，需要使用两个指令，分别是 `access_log` 和 `log_format` 指令。

**access_log**

`access_log` 指令的语法结构为 :

	access_log path [format [buffer=size]];

- `path` : 配置服务日志的文件存放的路径和名称。
- `format` : 可选项，自定义服务日志的格式字符串，也可以通过 "格式串的名称" 使用 `log_format` 指令定义好的格式。"格式串的名称" 在 `log_format` 指令中定义。
- `size` : 配置临时在存放的日志的内存缓存区大小。

>此指令可以在 **http 块**, **server 块** 或者 **location 块** 中进行设置。

默认的配置为 :

	access_log logs/access.log combined;
> combined 为 `log_format` 指令默认定义的日志格式字符串的名称。

如果要取消记录服务日志的功能, 则使用 :

	access_log off;

**log_format**

`log_format` 专门用于定义服务日志的格式，并且可以为格式字符串定义一个名字，以便 `access_log` 指令可以直接调用。

> 此指令只可在 **http 块** 中配置。

其语法格式为 :

	log_format name string ...;

- `name` : 格式字符串的名字，默认的名字为 combined。
- `string` : 服务日志的格式字符串。在字义过程中，可以使用 Nginx 的全局变量和写入日志时存在的变量进行配置，变量的名称使用双引号括起来， string 整体使用单绰号括起来。

下面来看一下示例 :

	log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
		'$status $body_bytes_sent "$http_referer" '
		'"$http_user_agent" "$http_x_forwarded_for"';

日志输出如下 :

	192.168.56.101 - - [29/Mar/2017:18:19:16 +0800] "GET /favicon.ico HTTP/1.1" 200 0 "-" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0"     "-"

日志格式可以包含**全局变量**(请参看nginx全局变量)和只在日志**写入时存在的变量** :

变量 | 解释 | 原文
---|---|---
`$bytes_sent` | 发往客户端的字节数 | the number of bytes sent to a client 
`$connection` | 链接序列号 | connection serial number 
`$connection_requests` | 当前一个链接上的请求量 | the current number of requests made through a connection (1.1.18) 
`$msec` | 日志写入所需时间(单位:ms) | time in seconds with a milliseconds resolution at the time of the log write 
`$pipe` | 如果是管道请求是"p",不是则为"." | “p” if request was pipelined, “.” otherwise 
`$request_length` | 请求长度(包括请求行，头和请求体) | request length (including request line, header, and request body) 
`$request_time` | 请求耗时(单位:ms) | request processing time in seconds with a milliseconds resolution; time elapsed between the first bytes were read from the client and the log write after the last bytes were sent to the client 
`$status` | 返回状态 | response status 
`$time_iso8601` | 当前时间(iso 8601格式) | local time in the ISO 8601 standard format 
`$time_local` | 当前时间 |  local time in the Common Log Format 
