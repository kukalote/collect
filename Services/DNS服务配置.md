# DNS 配置

```
$TTL 1D  #生存周期为1天
@   IN SOA          linuxprobe.com.     root.linuxprobe.com. 	(
	#授权信息开始:  #DNS区域的地址      #域名管理员的邮箱(不要用@符号)
                                                    0;serial 	#更新序列号
                                                    1D;refresh 	#更新时间
                                                    1H;retry 	#重试延时
                                                    1W;expire 	#失效时间
                                                    3H;minimum 	#无效解析记录的缓存时间
        NS          ns.linuxprobe.com.  #域名服务器记录
ns      IN A        192.168.10.10       #地址记录(ns.linuxprobe.com.)
        IN MX 10    mail.linuxprobe.com.#邮箱交换记录
mail    IN A        192.168.10.10       #地址记录(mail.linuxprobe.com.)
www     IN A        192.168.10.10       #地址记录(www.linuxprobe.com.)
bbs     IN A        192.168.10.20       #地址记录(bbs.linuxprobe.com.)
```




## 完整的区数据文件
到目前为止,我们已解释了区数据文件中的各种资源记录,现在我们将让你了解一下它们都在一起时看起来是怎么样的。再重申一下,这些资源记录的实际顺序并不重要。

下面是文件 db.movie.edu 的内容:
```
    $TTL 3h
    movie.edu. IN SOA terminator.movie.edu. al.robocop.movie.edu. (
                                    1       ; 序列号
                                    3h      ; 3 小时后刷新
                                    1h      ; 1 小时后重试
                                    1w      ; 1 周后期满
                                    1h )    ; 否定缓存 TTL 为 1 小时
    ;
    ; 名字服务器
    ;
    movie.edu.              IN NS   terminator.movie.edu.
    movie.edu.              IN NS   wormhole.movie.edu.

    ;
    ; 对应规范名字的地址
    ;
    localhost.movie.edu.    IN A    127.0.0.1
    robocop.movie.edu.      IN A    192.249.249.2
    terminator.movie.edu.   IN A    192.249.249.3
    diehard.movie.edu.      IN A    192.249.249.4
    misery.movie.edu.       IN A    192.253.253.2
    shining.movie.edu.      IN A    192.253.253.3
    carrie.movie.edu.       IN A    192.253.253.4
    wormhole.movie.edu.     IN A    192.249.249.1
    wormhole.movie.edu.     IN A    192.253.253.1

    ;
    ; 别名
    ;
    bigt.movie.edu.     IN CNAME    terminator.movie.edu.
    dh.movie.edu.       IN CNAME    diehard.movie.edu.
    wh.movie.edu.       IN CNAME    wormhole.movie.edu.

    ;
    ; 接口专用的名字
    ;
    wh249.movie.edu.    IN A        192.249.249.1
    wh253.movie.edu.    IN A        192.253.253.1
```

    下面是文件 db.192.249.249 的内容:

```
    $TTL 3h
    249.249.192.in-addr.arpa. IN SOA terminator.movie.edu. al.robocop.movie.edu.(
                                                1       ; 序列号
                                                3h      ; 3 小时后刷新
                                                1h      ; 1 小时后重试
                                                1w      ; 1 周后期满
                                                1h )    ; 否定缓存 TTL 为 1 小时
    ;
    ; 名字服务器
    ;
    249.249.192.in-addr.arpa.   IN NS   terminator.movie.edu.
    249.249.192.in-addr.arpa.   IN NS   wormhole.movie.edu.

    ;
    ; 指向规范名字的地址
    ;
    1.249.249.192.in-addr.arpa. IN PTR  wormhole.movie.edu.
    2.249.249.192.in-addr.arpa. IN PTR  robocop.movie.edu.
    3.249.249.192.in-addr.arpa. IN PTR  terminator.movie.edu.
    4.249.249.192.in-addr.arpa. IN PTR  diehard.movie.edu.
```

## 回送地址

名字服务器还需要一个额外的有关回送网络(loopback network)的 db.ADDR 文件, 回送地址(loopback address)是一个特殊的地址,主机用它来将数据流导向自己。回送网络(通常)是 127.0.0/24,而主机号(通常)是 127.0.0.1。因此这个文件名就是 db.127.0.0。毫无疑问,它看起来和其他 db.ADDR 文件一样。

下面是文件 db.127.0.0 的内容:

```
$TTL 3h
0.0.127.in-addr.arpa. IN SOA terminator.movie.edu. al.robocop.movie.edu. (
                                        1       ; 序列号
                                        3h      ; 3 小时后刷新
                                        1h      ; 1 小时后重试
                                        1w      ; 1 周后期满
                                        1h )    ; 否定缓存 TTL 为 1 小时

0.0.127.in-addr.arpa.   IN NS   terminator.movie.edu.
0.0.127.in-addr.arpa.   IN NS   wormhole.movie.edu.
1.0.0.127.in-addr.arpa. IN PTR  localhost.
```
