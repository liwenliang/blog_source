---
title: websocket https 连接
date: 2018-04-02 14:14:26
tags: [websocket, wss, https, pomelo]
---

最近部署项目需要用到wss连接，这时候就需要自己配置nginx对应的https配置，
wss连接实际上是通过https连接的，默认支持，所以我的配置如下：

```
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

server {
        listen       443 ssl;
        server_name  XXX.XX.XXX;

        ssl on;
        ssl_certificate ./keys/XXX.XX.XXX.crt;
        ssl_certificate_key ./keys/XXX.XX.XXX.key;
        ssl_session_timeout 5m;
        ssl_protocols SSLv2 SSLv3 TLSv1;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        access_log  /letv/logs/nginx/socket.access.log main;
        error_log /letv/logs/nginx/socket.err.log;

        root            /letv/www;
        location /gate {
                proxy_pass https://127.0.0.1:3010;
                proxy_redirect      off;
                proxy_set_header X-Real-IP        $remote_addr;
                proxy_set_header Host             $host;
                proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;

                proxy_http_version  1.1;
                proxy_set_header    Upgrade $http_upgrade;
                proxy_set_header    Connection 'upgrade';
        }

        location /connector_1 {
                proxy_pass https://127.0.0.1:4010;
                proxy_redirect      off;
                proxy_set_header X-Real-IP        $remote_addr;
                proxy_set_header Host             $host;
                proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;

                proxy_http_version  1.1;
                proxy_set_header    Upgrade $http_upgrade;
                proxy_set_header    Connection 'upgrade';
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
                root   html;
        }
}
```

其中通过路由做了反向代理，这么做的坏处是开发环境不太好整，最好是运维开放这几个websocket端口

找到运维，有些开放的，使用开放的就好了

公司机器的外网非80端口被禁用了不少，找到运维才知道有哪些是不被禁用的，我们使用不被禁用的就没必要再配置那么多nginx配置了。

公司内网wiki地址：http://wiki.letv.cn/pages/viewpage.action?pageId=56973252

最后的配置方式如下即可：

```
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

server {
        listen       443 ssl;
        server_name  XXX.XX.XXX;

        ssl on;
        ssl_certificate ./keys/XXX.XX.XXX.crt;
        ssl_certificate_key ./keys/XXX.XX.XXX.key;
        ssl_session_timeout 5m;
        ssl_protocols SSLv2 SSLv3 TLSv1;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        access_log  /letv/logs/nginx/socket.access.log main;
        error_log /letv/logs/nginx/socket.err.log;

        root            /letv/www;

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
                root   html;
        }
}
```