title: Nginx cros site
tags:
  - nginx
categories:
  - study
cc: true
hljs: true
comments: true
date: 2017-07-12 15:39:00
author: me
cover: "-/images/zsh.png"
---
> 又是cors跨域…cors是啥？就是跨域请求。下面给出nginx cors的相关配置。

* 允许哪个域名来访问资源
```
add_header 'Access-Control-Allow-Origin' "$http_origin";
```
* 请求的返回内容里包含cookies
```
add_header 'Access-Control-Allow-Credentials' 'true';
```
* 允许请求的method 
```
add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
```
nginx conf example: 
```
location / {
       if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' "$http_origin";
                add_header 'Access-Control-Allow-Methods' 'PUT, DELETE, GET, POST, OPTIONS';
                add_header 'Access-Control-Allow-Headers' 'reqid, nid, host, x-real-ip, x-forwarded-ip, event-type, event-id, accept, content-type';
                add_header 'Access-Control-Max-Age' 1728000;
                return 204;
             }
        add_header 'Access-Control-Allow-Origin' "$http_origin";
        add_header 'Access-Control-Allow-Credentials' 'true';
        add_header 'Access-Control-Allow-Methods' 'PUT, GET, POST, OPTIONS, DELETE';
        add_header 'Access-Control-Allow-Headers' 'reqid, nid, host, x-real-ip, x-forwarded-ip, event-type, event-id, accept, content-type';
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_connect_timeout 120s;
        proxy_send_timeout 120s;
        proxy_read_timeout 120s;
        proxy_buffering    on;
        proxy_buffer_size 64k;
        proxy_buffers  4 32k;
        proxy_busy_buffers_size 64k;
        proxy_pass http://server;
      }
```