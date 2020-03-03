### Linux_cURL程序

#### 25.2 cURL 程序

cURL 程序允许自动从命令行使用特定的 URL 传输文件。它目前支持 URL 中指定的 FTP、FTPS、HTTP、HTTPS、SCP、SFTP、TFTP、Telnet、DICT、LDAP、LDAPS 和 FILE 协议。

##### 25.2.2 cURL 命令行

**cURL 命令行参数**

长参数 | 短参数 | 描述
----|---|---
`--append` | `-a` | 在上传时追加到目标文件
`--user-agent name` | `-A` | 用户代理发送到服务器
`--anyauth` | | 使用 ANY 验证模式
`--cookie name=string` | `-b` | Cookie 字符串或读取 cookies 的文件
`--basic` |  | 使用 HTTP 基本验证
`--use-ascii` | `-B` | 使用 ASCII 传输数据
`--cookie-jar file` | `-c` | 操作后将 cookies 写入到指定的文件
`--continue-at offset` | `-C` | 在 offset 上恢复文件传输
`--data data` | `-d` | 使用 HTTP POST 方法发送数据
`--data-ascii data` | | 使用 HTTP POST 方法发送 ASCII 数据
`--data-binary data` | | 使用 HTTP POST 方法发送二进制数据
`negotiate` | | 使用 HTTP 谈判验证
`--digest` | | 使用 HTTP 摘要验证
`--disable-eprt` | | 禁止使用 DPRT 或 LPRT
`--disable-epsv` | | 禁止使用 EPSV
`--dump-header file` | `-D` | 将 HTTP 会话报头写入到指定文件
`--egd-file file` | | 为随机数据指定 EGD 套接字路径
`--tcp-nodelay` | | 使用 TCP 的 NODELAY 选项
`--referer` | `-e` | 引用 URL
`--cert cert:passwd` | `-E` | 指定客户端证书文件和密码
`cert-type type` | | 指定证书文件类型 ( DER/PEM/ENG )
`--key key` | | 指定私钥文件
`-key-type type` | | 指定私钥文件类型 ( DER/PEM/ENG )
`--pass pass` | | 指定私钥的 passphrase
`--engine eng` | | 指定要使用 crypto 引擎
`--cacert file` | | 指定 CA 证书以验证远程峰值
`--capth dir` | | 指定 CA 目录以验证远程峰值
`--ciphers list` | | 指定 SSL 密码列表以便在 SSL 连接中使用
`--compressed` | | 请求压缩的响应
`--connect-timeout sec` | | 指定连接的最大时间 ( 以秒为单位 )
`--create-dirs` | | 创建需要的本地目录层次结构
`--crlf` | | 在上传时将 LF 转换为 CRLF
`-fail` | `-f` | 遇到 HTTP 错误时以静默方式失败
`--ftp-create-dirs` | | 如果不存在, 则创建远程目录
`--ftp-pasv` | | 在传输中使用 PASV/EPSV, 而不是 PORT
`--ftp-skip-pasv-ip` | | 跳过 PASV 的 IP 地址
`--ftp-ssl` | | 启用 FTP 传输的 SSL/TLS
`--form name-content` | `-F` | 指定 HTTP 多部件 POST 数据
`--form-string name-string` | | 指定 HTTP 多部件 POST 数据
`--globoff` | `-g` | 使用 {} 和 [] 禁用 URL 序列和范围
`-get` | `-G` | 使用 HTTP GET 发送 -d 数据
`--help` | `-h` | 显示帮助文本文件
`--header header` | `-H` | 指定要传递给服务器的自定义 HTTP 头部
`--ignore-content-length` | | 忽略 HTTP 内容长度头部
`--head` | `-I` | 只显示文档信息
`--include` | `-i` | 在输出中包括协议的头部
`--junk-session-cookies` | `-j` | 忽略从文件读取的会话 cookie
`--interface int` | | 指定要使用的网络接口
`--krb4 level` | | 以指定的安全级别启用 krb4
`--insecure` | `-k` | 不经确定而允许连接到 SSL 站点
`--config` | `-K` | 指定读取的配置文件
`--list-only` | `-l` | 只列出 FTP 目录的名称
`--limit-rate rate` | | 将传输速度限制为 rate 字节/秒
`--location` | `-L` | 遵循 HTTP 位置 : 头部
`--location-trusted` | | 遵循位置 : 甚至将验证发送给其他主机名
`--max-time sec` | `-m` | 允许文件传输的最大时间 ( 以秒为单位 )
`--max-redirs num` | | 最大重定向次数
`--max-filesize bytes` | | 下载的最大文件大小 ( 以字节为单位 )
`--manual` | `-M` | 显示完整手册
`--netrc` | `-n` | 必须读取 .netrc 获得用户名和密码
`--netrc-optional` | | 使用 .netrc 或者 UR。覆盖 -n
`--ntlm` | | 使用 HTTP NTLM 验证
`--no-buffer` | `-N` | 禁用输出流的缓存
`--output file` | `-o` | 将输出写到以远程文件命令的文件
`--remote-name` | `-O` | 将输出写到以远程文件命令的文件
`--proxytunnel` | `-p` | 通过 HTTP 代理通道操作
`--proxy-anyauth` | | 使用任意代理验证方法
`--proxy-basic` | | 在代理上使用 Basic 验证
`--proxy-digest` | | 在代理上使用 Digest 验证
`--proxy-ntlm` | | 在代理上使用 NTLM 验证
`--ftp-port address` | `-P` | 为 FTP 使用 TCP 端口地址而不是 PASV
 | `-q` | 如果用作第一个参数, 则禁止读取 .curlrc 文件
`--quote cmd` | `-Q` | 在文件传输之前, 将命令 cmd 发送到服务器
`--range range` | `-r` | 检索来处自 HTTP/1.1 或 FTP 服务器的字节范围
`--random-file file` | | 指定文件用来 SSL 读取随机数据
`--remote-time` | `-R` | 在本地输出上设置远程文件的时间
`--retry num` | | 如果发生短暂问题, 则再尝试 num 次
`--retry-delay sec` | | 在重新尝试时, 在每次尝试之前等待 sec 秒
`--retry-max-time sec` | | 只在 sec ( 秒 ) 之内尝试
`--silent` | `-s` | 静默模式。不输出任何内容
`--show-error` | `-S` | 显示错误。使用 -S 选项, 在错误发生时显示错误
`--socks host:port` | | 在指定的主机和端口上使用 SOCKS5 代理
`--stderr file` | | 指定文件以重定向 STDERR 。使用 -redirects 到 STDOUT
`--telnet-option OPT=val` | `-t` | 设置 Telnet 选项
`--trace file` | | 将错误跟踪写入指定的文件
`--trace-ascii file` | | 类似于 --trace, 但无十六进制输出
`--trace-time` | | 为跟踪/冗长输出添加时间戳
`--upload-file file` | `-T` | 指定文件以传输到远程站点
`--url URL` | | 指定要连接的 URL
`--user user:password` | `-u` | 指定远程服务器的用户 ID 和密码
`--proxy-user user:password` | `-U` | 指定代理服务器要求的用户 ID 和密码
`--verbose` | `-v` | 如果可用, 则显示更多输出信息
`--version` | `-V` | 显示版本号并退出
`--write-out format` | `-w` | 指定完成后显示的文本
`--proxy host:port` | `-x` | 指定 HTTP 代理服务器的主机名和端口
`--request cmd` | `-X` | 指定要使用的请求命令
`--speed-time` | `-y` | 激发一个 --speed-limit 忽略需要的时间 (以秒为单位)。默认为 30 秒
`--speed-limit` | `-Y` | 如果 --Speed-time 设置小于 Speed-limit, 则停止传输
`--time-cond time` | `-z` | 为传输设置时间条件
`--http1.0` | `-0` | 使用 HTTP 1.0
`--tlsv1` | `-1` | 使用 TLSv1
`--sslv2` | `-2` | 使用 SSLv2
`--sslv3` | `-3` | 使用 SSLv3
`--3p-quote` | | 类似于源用于第三方传输的 URL 的 -Q
`--3p-url` | | 激活第三方传输的源 URL
`--3p-user` | | 用于源第三方传输的用户和密码
`--ipv4` | `-4` | 将名称解析为 IPv4 地址
`--ipv6` | `-6` | 将名称解析为 IPv6 地址
`--progress-bar` | `-#` | 将传输进程显示为进度条