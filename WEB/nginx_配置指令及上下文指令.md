# Nginx_配置的一些指令

## 指令上下文

nginx.conf 中的配置信息，根据其逻辑上的意义，对它们进行了分类，也就是分成了多个作用域，或者称之为配置指令上下文。不同的作用域含有一个或者多个配置项。

当前 Nginx 支持的几个指令上下文：

- **main** : Nginx 在运行时与具体业务功能（比如http服务或者email服务代理）无关的一些参数，比如工作进程数，运行的身份等。
- **http** : 与提供 http 服务相关的一些配置参数。例如：是否使用 keepalive 啊，是否使用gzip进行压缩等。
- **server** : http 服务上支持若干虚拟主机。每个虚拟主机一个对应的 server 配置项，配置项里面包含该虚拟主机相关的配置。在提供 mail 服务的代理时，也可以建立若干 server，每个 server 通过监听的地址来区分。
- **location** : http 服务中，某些特定的URL对应的一系列配置项。
- **mail** : 实现 email 相关的 SMTP/IMAP/POP3 代理时，共享的一些配置项（因为可能实现多个代理，工作在多个监听地址上）。

## 配置指令的上下文

在 `main` 上下文的配置指令有 :

- user
- worker_processes
- error_log
- events
- http
- mail

存在于 `http` 上下文中的指令如下:

- server

存在于 `mail` 上下文中的指令如下：

- server
- auth_http
- imap_capabilities

存在于 `server` 上下文中的配置指令如下：

- listen
- server_name
- access_log
- location
- protocol
- proxy
- smtp_auth
- xclient

存在于 `location` 上下文中的指令如下：

- index
- root

> *当然，这里只是一些示例。具体有哪些配置指令，以及这些配置指令可以出现在什么样的上下文中，需要参考 Nginx 的使用文档。

## 示例展示

```
	user  nobody;
    worker_processes  1;
    error_log  logs/error.log  info;

    events {
        worker_connections  1024;
    }

    http {
        server {
            listen          80;
            server_name     www.linuxidc.com;
            access_log      logs/linuxidc.access.log main;
            location / {
                index index.html;
                root  /var/www/linuxidc.com/htdocs;
            }
        }

        server {
            listen          80;
            server_name     www.Androidj.com;
            access_log      logs/androidj.access.log main;
            location / {
                index index.html;
                root  /var/www/androidj.com/htdocs;
            }
        }
    }

    mail {
        auth_http  127.0.0.1:80/auth.php;
        pop3_capabilities  "TOP"  "USER";
        imap_capabilities  "IMAP4rev1"  "UIDPLUS";

        server {
            listen     110;
            protocol   pop3;
            proxy      on;
        }
        server {
            listen      25;
            protocol    smtp;
            proxy       on;
            smtp_auth   login plain;
            xclient     off;
        }
    }
```