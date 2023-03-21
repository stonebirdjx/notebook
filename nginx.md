<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [安装nginx](#%E5%AE%89%E8%A3%85nginx)
- [初学者须知](#%E5%88%9D%E5%AD%A6%E8%80%85%E9%A1%BB%E7%9F%A5)
  - [:point_right:启停、加载配置](#point_right%E5%90%AF%E5%81%9C%E5%8A%A0%E8%BD%BD%E9%85%8D%E7%BD%AE)
  - [:point_right:配置文件的结构](#point_right%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9A%84%E7%BB%93%E6%9E%84)
  - [nginx-plus - 付费版的nginx了解即可](#nginx-plus---%E4%BB%98%E8%B4%B9%E7%89%88%E7%9A%84nginx%E4%BA%86%E8%A7%A3%E5%8D%B3%E5%8F%AF)
- [:point_right:processing方式 - 挺重要](#point_rightprocessing%E6%96%B9%E5%BC%8F---%E6%8C%BA%E9%87%8D%E8%A6%81)
- [:point_right:nginx 命令行参数](#point_rightnginx-%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0)
- [:point_right:nginx负载均衡](#point_rightnginx%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1)
  - [:point_right:least_conn - 最少连接数](#point_rightleast_conn---%E6%9C%80%E5%B0%91%E8%BF%9E%E6%8E%A5%E6%95%B0)
  - [:point_right:ip_hash - hash到某一台设备](#point_rightip_hash---hash%E5%88%B0%E6%9F%90%E4%B8%80%E5%8F%B0%E8%AE%BE%E5%A4%87)
  - [:point_right:Weight - 权重](#point_rightweight---%E6%9D%83%E9%87%8D)
  - [:point_right:健康检查](#point_right%E5%81%A5%E5%BA%B7%E6%A3%80%E6%9F%A5)
- [:point_right:TCP/UDP session](#point_righttcpudp-session)
- [配置](#%E9%85%8D%E7%BD%AE)
  - [开发者指南](#%E5%BC%80%E5%8F%91%E8%80%85%E6%8C%87%E5%8D%97)
  - [:point_right:指令指南](#point_right%E6%8C%87%E4%BB%A4%E6%8C%87%E5%8D%97)
  - [:point_right:变量指南](#point_right%E5%8F%98%E9%87%8F%E6%8C%87%E5%8D%97)
  - [:point_right:core核心指南](#point_rightcore%E6%A0%B8%E5%BF%83%E6%8C%87%E5%8D%97)
  - [pid](#pid)
  - [debug日志](#debug%E6%97%A5%E5%BF%97)
  - [syslog系统日志](#syslog%E7%B3%BB%E7%BB%9F%E6%97%A5%E5%BF%97)
  - [nginx 处理请求](#nginx-%E5%A4%84%E7%90%86%E8%AF%B7%E6%B1%82)
  - [HTTPS servers](#https-servers)
  - [nginx-javascript](#nginx-javascript)
  - [:point_right:nginx - lua](#point_rightnginx---lua)
  - [:point_right:url规则重写](#point_righturl%E8%A7%84%E5%88%99%E9%87%8D%E5%86%99)
  - [websocket](#websocket)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

nginx [engine x] 是一个 HTTP 和反向代理服务器、一个邮件代理服务器和一个通用的 TCP/UDP 代理服务器，最初由 Igor Sysoev 编写

[官方介绍](http://nginx.org/en/)

主要功能

- HTTP 服务器功能
- TCP/UDP 代理
- 流媒体服务

# 安装nginx

手动安装编译参考：http://nginx.org/en/docs/configure.html

> 使用Docker 即可，特殊模块Lua 、hls、rtmp模块使用，需要自行编译

# 初学者须知

nginx 有一个master 进程和多个worker 进程。 主进程的主要目的是读取和评估配置，以及维护工作进程。 工作进程对请求进行实际处理。 nginx 采用基于事件的模型和依赖于操作系统的机制在工作进程之间有效地分发请求。 工作进程的数量在配置文件中定义，并且可以针对给定的配置固定或自动调整为可用 CPU 内核的数量（请参阅 worker_processes）。

nginx 及其模块的工作方式在配置文件中确定。 默认情况下，配置文件名为nginx.conf，放置在/usr/local/nginx/conf、/etc/nginx或/usr/local/etc/nginx目录下。

## :point_right:启停、加载配置

nginx 启动后，可以通过使用 -s 参数调用可执行文件来控制它。

```bash
nginx -s signal # Where signal may be one of the following:

stop — 快速停止
quit — 优雅关机
reload — 重新价值配置文件
reopen — 重新打开日志文件

# 借助 kill 实用程序等 Unix 工具，也可以向 nginx 进程发送信号。
kill -s QUIT 1628
```

## :point_right:配置文件的结构

nginx 由模块组成，这些模块由配置文件中指定的指令控制。 指令分为简单指令和块指令。 简单指令由名称和参数组成，以空格分隔并以分号 (;) 结尾。(examples: events, http, server, and location).

`#` 代表注释

静态内容为例：您将实现一个示例，根据请求，文件将从不同的本地目录提供：/data/www（可能包含 HTML 文件）和 /data/images（包含图像）。

```bash
http {
    server {
        location / {
            root /data/www;
        }

        location /images/ {
            root /data;
        }
    }
}
```

代理服务器 proxy_pass

```bash
server {
    location / {
        proxy_pass http://localhost:8080/;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```

Fast CGI

```bash
server {
    location / {
        fastcgi_pass  localhost:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param QUERY_STRING    $query_string;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```

## nginx-plus - 付费版的nginx了解即可

提供了更多的功能，针对企业需要的一些服务进行了优化。

# :point_right:processing方式 - 挺重要

支持以下连接处理方式：

- `select` — standard method. The supporting module is built automatically on platforms that lack more efficient methods. The `--with-select_module` and `--without-select_module` configuration parameters can be used to forcibly enable or disable the build of this module.
- `poll` — standard method. The supporting module is built automatically on platforms that lack more efficient methods. The `--with-poll_module` and `--without-poll_module` configuration parameters can be used to forcibly enable or disable the build of this module.
- `kqueue` — efficient method used on FreeBSD 4.1+, OpenBSD 2.9+, NetBSD 2.0, and macOS.
- `epoll` — efficient method used on Linux 2.6+.
  - The `EPOLLRDHUP` (Linux 2.6.17, glibc 2.8) and `EPOLLEXCLUSIVE` (Linux 4.5, glibc 2.24) flags are supported since 1.11.3.
  - Some older distributions like SuSE 8.2 provide patches that add epoll support to 2.4 kernels
- `/dev/poll` - efficient method used on Solaris 7 11/99+, HP/UX 11.22+ (eventport), IRIX 6.5.15+, and Tru64 UNIX 5.1A+.
- `eventport` - event ports, method used on Solaris 10+ (due to known issues, it is recommended using the `/dev/poll` method instead).

# :point_right:nginx 命令行参数

- `-?` | `-h` - 帮助。
- `-c `*`file`* - 使用替代配置文件而不是默认文件。
- `-e `*`file`* - 使用替代错误日志文件而不是默认文件 (1.19.5) 来存储日志。 特殊值 stderr 选择标准错误文件。
- `-g `*`directives`* - 设置全局配置指令

> - nginx -g "pid /var/run/nginx.pid; worker_processes `sysctl -n hw.ncpu`;"

- `-p `*`prefix`* - 设置 nginx 路径前缀，即保存服务器文件的目录（默认值为 /usr/local/nginx.
- `-q` - 在配置测试期间抑制非错误消息。
- `-s `*`signal`* — 向主进程发送信号:
  - `stop` - 快速关闭。
  - `quit` - 优雅关闭。
  - `reload` - 重新加载配置.
  - `reopen` — 重新打开日志文件。
- `-t` - 测试配置文件：nginx 检查配置的语法是否正确，然后尝试打开配置中引用的文件.
- `-T` - 测试配置文件并将其输出到终端。
- `-v` - 输出当前nginx的版本号.
- `-V` - 打印 nginx 版本、编译器版本和配置参数。

# :point_right:nginx负载均衡

## :point_right:least_conn - 最少连接数

当 least_conn 指令用作服务器组配置的一部分时，nginx 中的最少连接负载平衡被激活：

```Bash
upstream myapp1 {
        least_conn;
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
}
```

## :point_right:ip_hash - hash到某一台设备

使用 ip-hash，客户端的 IP 地址被用作散列键来确定应该为客户端的请求选择服务器组中的哪个服务器。

```Bash
upstream myapp1 {
    ip_hash;
    server srv1.example.com;
    server srv2.example.com;
    server srv3.example.com;
}
```

## :point_right:Weight - 权重

当为服务器指定权重参数时，权重将作为负载平衡决策的一部分进行计算。

```Bash
upstream myapp1 {
   server srv1.example.com weight=3;
   server srv2.example.com;
   server srv3.example.com;
}
#每 5 个新请求将分布在应用程序实例中，如下所示：3 个请求将定向到 srv1，一个请求将定向到 srv2，另一个 - 定向到 srv3。
```

## :point_right:健康检查

max_fails 指令设置在 fail_timeout 期间应发生的与服务器通信的连续不成功尝试次数。 默认情况下，max_fails 设置为 1。设置为 0 时，将禁用此服务器的健康检查。 fail_timeout 参数还定义了服务器将被标记为失败的时间。

```Bash
max_fails=number fail_timeout=number
```

In addition, there are more directives and parameters that control server load balancing in nginx, e.g. [proxy_next_upstream](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream), [backup](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#server), [down](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#server), and [keepalive](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#keepalive). For more information, please check our [reference documentation](http://nginx.org/en/docs/).

# :point_right:TCP/UDP session

```Bash
# tcp
worker_processes auto;
error_log /var/log/nginx/error.log info;
events {
    worker_connections  1024;
}
stream {
    upstream backend {
        server 127.0.0.1:12345  max_fails=3 fail_timeout=30s;
    }
    server {
        listen 12345;
        proxy_connect_timeout 1s;
        proxy_timeout 3s;
        proxy_pass backend;
    }
}

# udp
worker_processes auto;
error_log /var/log/nginx/error.log info;
events {
    worker_connections  1024;
}
stream {
    upstream tcp_backend {
        server 192.168.31.1:22  max_fails=5 fail_timeout=30s;
    }
    server {
        listen 12345;
        proxy_connect_timeout 10s;
        proxy_timeout 30s;
        proxy_pass tcp_backend;
    }
    upstream udp_backend {
        server 192.168.31.51:514;
    }
    server {
        listen 1514 udp;
        proxy_pass udp_backend;
    }
}
```



# 配置

## 开发者指南

[Development guide](http://nginx.org/en/docs/dev/development_guide.html#time)

## :point_right:指令指南

[指令的字母索引](http://nginx.org/en/docs/dirindex.html)

## :point_right:变量指南

[变量的字母索引 ](http://nginx.org/en/docs/varindex.html)

## :point_right:core核心指南

[Core functionality](http://nginx.org/en/docs/ngx_core_module.html#example)

## pid

默认pid文件：/usr/local/nginx/logs/nginx.pid

```bash
ps axw -o pid,ppid,user,%cpu,vsz,wchan,command | egrep '(nginx|PID)'
```

## debug日志

```b
# 编译时
./configure --with-debug ...

# config配置
error_log /path/to/log debug;
```

## syslog系统日志

The [error_log](http://nginx.org/en/docs/ngx_core_module.html#error_log) and [access_log](http://nginx.org/en/docs/http/ngx_http_log_module.html#access_log) directives support logging to syslog.

```Bash
error_log syslog:server=192.168.1.1 debug;

access_log syslog:server=unix:/var/log/nginx.sock,nohostname;
access_log syslog:server=[2001:db8::1]:12345,facility=local7,tag=nginx,severity=info combined;
```

## nginx 处理请求

```bash
server {
    listen      80;
    server_name example.org www.example.org;
    ...
}

# 也可以使用 listen 指令中的 default_server 参数显式设置默认服务器：
server {
    listen      80 default_server;
    server_name example.net www.example.net;
    ...
}
```

多网卡设备,监听不同的ip地址

```bash
server {
    listen      192.168.1.1:80;
    server_name example.org www.example.org;
    ...
}

server {
    listen      192.168.1.1:80;
    server_name example.net www.example.net;
    ...
}

server {
    listen      192.168.1.2:80;
    server_name example.com www.example.com;
    ...
}
```

## HTTPS servers

```Bash
server {
    listen              443 ssl;
    server_name         www.example.com;
    ssl_certificate     www.example.com.crt;
    ssl_certificate_key www.example.com.key;
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers         HIGH:!aNULL:!MD5;
    ...
}

# 
worker_processes auto;

http {
    ssl_session_cache   shared:SSL:10m;
    ssl_session_timeout 10m;

    server {
        listen              443 ssl;
        server_name         www.example.com;
        keepalive_timeout   70;

        ssl_certificate     www.example.com.crt;
        ssl_certificate_key www.example.com.key;
        ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers         HIGH:!aNULL:!MD5;
        ...
```

## nginx-javascript

```javascript
// http.js
function hello(r) {
    r.return(200, "Hello world!");
}

export default { hello };
```

nginx.conf

```Bash
load_module modules/ngx_http_js_module.so;

events {}

http {
    js_import http.js;

    server {
        listen 8000;

        location / {
            js_content http.hello;
        }
    }
}
```

## :point_right:nginx - lua

Lua是一种轻量级、可嵌入式的脚本语言，这样可以非常容易的嵌入到其他语言中使用。

将 Nginx，Lua 组合到一起的是 OpenResty，它有一个 ngx_lua 模块

[值得细学](https://github.com/openresty/lua-nginx-module)

```bash

 # set search paths for pure Lua external libraries (';;' is the default path):
 lua_package_path '/foo/bar/?.lua;/blah/?.lua;;';

 # set search paths for Lua external libraries written in C (can also use ';;'):
 lua_package_cpath '/bar/baz/?.so;/blah/blah/?.so;;';

 server {
     location /lua_content {
         # MIME type determined by default_type:
         default_type 'text/plain';

         content_by_lua_block {
             ngx.say('Hello,world!')
         }
     }

     location /nginx_var {
         # MIME type determined by default_type:
         default_type 'text/plain';

         # try access /nginx_var?a=hello,world
         content_by_lua_block {
             ngx.say(ngx.var.arg_a)
         }
     }

     location = /request_body {
         client_max_body_size 50k;
         client_body_buffer_size 50k;

         content_by_lua_block {
             ngx.req.read_body()  -- explicitly read the req body
             local data = ngx.req.get_body_data()
             if data then
                 ngx.say("body data:")
                 ngx.print(data)
                 return
             end

             -- body may get buffered in a temp file:
             local file = ngx.req.get_body_file()
             if file then
                 ngx.say("body is in file ", file)
             else
                 ngx.say("no body found")
             end
         }
     }

     # transparent non-blocking I/O in Lua via subrequests
     # (well, a better way is to use cosockets)
     location = /lua {
         # MIME type determined by default_type:
         default_type 'text/plain';

         content_by_lua_block {
             local res = ngx.location.capture("/some_other_location")
             if res then
                 ngx.say("status: ", res.status)
                 ngx.say("body:")
                 ngx.print(res.body)
             end
         }
     }

     location = /foo {
         rewrite_by_lua_block {
             res = ngx.location.capture("/memc",
                 { args = { cmd = "incr", key = ngx.var.uri } }
             )
         }

         proxy_pass http://blah.blah.com;
     }

     location = /mixed {
         rewrite_by_lua_file /path/to/rewrite.lua;
         access_by_lua_file /path/to/access.lua;
         content_by_lua_file /path/to/content.lua;
     }

     # use nginx var in code path
     # CAUTION: contents in nginx var must be carefully filtered,
     # otherwise there'll be great security risk!
     location ~ ^/app/([-_a-zA-Z0-9/]+) {
         set $path $1;
         content_by_lua_file /path/to/lua/app/root/$path.lua;
     }

     location / {
        client_max_body_size 100k;
        client_body_buffer_size 100k;

        access_by_lua_block {
            -- check the client IP address is in our black list
            if ngx.var.remote_addr == "132.5.72.3" then
                ngx.exit(ngx.HTTP_FORBIDDEN)
            end

            -- check if the URI contains bad words
            if ngx.var.uri and
                   string.match(ngx.var.request_body, "evil")
            then
                return ngx.redirect("/terms_of_use.html")
            end

            -- tests passed
        }

        # proxy_pass/fastcgi_pass/etc settings
     }
 }
```

## :point_right:url规则重写

ngx_http_rewrite_module 模块用于使用 PCRE 正则表达式更改请求 URI、返回重定向和有条件地选择配置。

break、if、return、rewrite 和 set 指令按以下顺序处理：

在服务器级别指定的该模块的指令按顺序执行；

反复：

根据请求 URI 搜索位置；

在找到的位置内指定的该模块的指令按顺序执行；

如果请求 URI 被重写，循环将重复，但不超过 10 次。

```bash
if (condition) { … }
if 可以支持如下条件判断匹配符号
~                   正则匹配 (区分大小写)
~*                  正则匹配 (不区分大小写)
!~                  正则不匹配 (区分大小写)
!~*                 正则不匹配  (不区分大小写)
-f 和!-f             用来判断是否存在文件
-d 和!-d             用来判断是否存在目录
-e 和!-e             用来判断是否存在文件或目录
-x 和!-x             用来判断文件是否可执行

在匹配过程中可以引用一些Nginx的全局变量
$args               请求中的参数;
$document_root      针对当前请求的根路径设置值;
$host               请求信息中的"Host"，如果请求中没有Host行，则等于设置的服务器名;
$limit_rate         对连接速率的限制;
$request_method     请求的方法，比如"GET"、"POST"等;
$remote_addr        客户端地址;
$remote_port        客户端端口号;
$remote_user        客户端用户名，认证用;
$request_filename   当前请求的文件路径名（带网站的主目录/usr/local/nginx/html/images /a.jpg）
$request_uri        当前请求的文件路径名（不带网站的主目录/images/a.jpg）
$query_string       与$args相同;
$scheme             用的协议，比如http或者是https
$server_protocol    请求的协议版本，"HTTP/1.0"或"HTTP/1.1";
$server_addr        服务器地址，如果没有用listen指明服务器地址，使用这个变量将发起一次系统调用以取得地址(造成资源浪费);
$server_name        请求到达的服务器名;
$document_uri       与$uri一样，URI地址;
$server_port        请求到达的服务器端口号;

# 例子
server {
    listen       80;
    server_name  www.test.com;
    location /2020/a {
        root   /usr/share/nginx/html;
        index  1.html;
        rewrite ^/2020/(.*)$ /2018/$1 permanent;
    }

    location /2018/a {
        root /usr/share/nginx/html;
        index 1.html;
    }

}
```

## websocket

As noted above, hop-by-hop headers including “Upgrade” and “Connection” are not passed from a client to proxied server, therefore in order for the proxied server to know about the client’s intention to switch a protocol to WebSocket, these headers have to be passed explicitly:

```Bash
location /chat/ {
    proxy_pass http://backend;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
}

# 使用map
http {
    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }

    server {
        ...

        location /chat/ {
            proxy_pass http://backend;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
        }
    }
```
