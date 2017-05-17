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
