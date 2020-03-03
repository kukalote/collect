# Nginx_反向代理配置



```bash
#定义要包含在负载均衡方案中的服务器。  
#最好使用服务器的私有IP以获得更好的性能和安全性。

upstream load_balance {
	server load_balance1:8011   max_fails=2   fail_timeout=30s; 
	server load_balance2:8012   max_fails=2   fail_timeout=30s;  
	server load_balance3:8013   max_fails=2   fail_timeout=30s;
	# 长链接配置(可选)
	keep_alive 32;
}

# 优先使用最少链接的服务器                                          
#upstream load_balance {
#	least_conn;         
#	server load_balance1:8011;
#	server load_balance2:8012;  
#	server load_balance3:8013;
#}

# ip_hash分配访问的服务器                                          
#upstream load_balance {
#	ip_hash;         
#	server load_balance1:8011;
#	server load_balance2:8012;  
#	server load_balance3:8013;
#}

# 根据权重配置服务器访问
#upstream load_balance {
#	# weight访问服务器权重，默认为1 
#	server load_balance1:8011 weight=4; 
#	server load_balance2:8012 weight=2;
#	server load_balance3:8013;
#}      

# nginx 健康检查
#upstream load_balance {
#	# weight访问服务器权重，默认为1 
#	server load_balance1:8011 weight=4;
#	# 默认失败次数为1,禁用检查为0；超时为10s;
#	server load_balance2:8012 max_fails=2 fail_timeout=30s;
#	# down 暂停服务
#	server load_balance3:8013 down;
#	# backup 标记为备用服务器,当主服务器不可用后，请求会被传给这些服务器
#	server load_balance4:8014 backup;
#}      

# 该服务器接受到端口80的所有流量并将其传递给上游。
# 请注意，上游名称和proxy_pass需要匹配。
server {
 	listen 80;
 	server_name dev.load_balance.loc;

 	error_log    /var/log/nginx/load_balance.error.log;
 	access_log   /var/log/nginx/load_balance.access.log;

 	# proxy settings
 	proxy_redirect     off;
 	proxy_set_header   Host    $host;
 	proxy_set_header   X-Real-IP  $remote_addr;
 	# 长链接配置(可选)
 	proxy_http_version 1.1;
 	proxy_set_header Connection "";
 	# 代理异常处理
 	proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
 	proxy_max_temp_file_size 0;
 	proxy_connect_timeout 10s;
 	proxy_read_timeout 10s;
 	proxy_send_timeout 10s;

 	location / {
 		#return 200 success\n;
 		proxy_pass http://load_balance/;
 	}
}
server {
 	listen 8011;
    server_name load_balance1;
 	access_log /var/log/nginx/load_balance1.access.log;
 	return 200 server1\n;
}
server {
 	listen 8012;
 	server_name load_balance2;
    return 200 server2\n;
}
server {
	listen 8013;
	server_name load_balance3;
    return 200 server3\n;
}

```

