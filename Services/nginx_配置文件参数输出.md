# nginx 配置文件异常输出

在配置 `nginx` 后经常会有一些因配置异常引起的错误。那我们如何来调试这些错误呢，这里给出一些解决方法 :

## The Error Log

配置错误日志输出，如下 :

	error_log /var/log/nginx/file.log warn;

我们需要更多的信息，而不仅仅 “Unable to open primary script” ，比如像如下信息 :

	2013/06/03 12:56:12 [error] 2641#0: *20499586 FastCGI sent in stderr: "Unable to open primary script: /home/user/public_html/wp-login.php (No such file or directory)" while reading response header from upstream, client: 10.0.0.1, server: www.example.com, request: "GET //wp-login.php HTTP/1.1", upstream: "fastcgi://127.0.0.1:9000", host: "www.example.com"

## Getting Debug Output

仅凭经验，我们很难判断 `nginx` 实际的处理过程，我们通常需要知道的是命令中需要用到变量的值， 这里我们就需要使用 `return` 命令。

你可以偿试使用如下命令输出 :

	default_type text/plain;	//纯文本输出
	return 200 $request_filename;	//输出变量的值

	//输出多个变量的值
	return 200 $document_root-$fastcgi_script_name;