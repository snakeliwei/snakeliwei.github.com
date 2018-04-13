title: 使用自签证书部署HTTPS
tags:
  - nginx
  - Cert
  - tips
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2016-07-15 15:36:19
author: me
cover: "-/images/zsh.png"
---
> 项目马上要启用HTTPS，先找个东西练手
- 先生成一个RSA的key，加密算法选择aes也行，des3也行。这里使用1024位的des3。
```bash
openssl genrsa -des3 -out ssl.key 1024
```
> 会提示必须输入一个密码，随便输入一个就行了，下面我们要解密后才给nginx使用。

```bash
openssl rsa -in ssl.key -out fm.key
```
- 然后用这个key来生成一个证书请求。

```bash
openssl req -new -key fm.key -out fm.csr
```
> 接着按照提示，输入证书的信息。

- 得到csr文件后，通过下面命令生成一个自签名的证书，x509是证书的格式，3650表示证书有效期为10年。

```bash
openssl x509 -req -days 3650 -in fm.csr -signkey fm.key -out fm.crt
```
> 有了fm.crt和fm.key，就可以在nginx上搭建https的服务了

- 下面是nginx的server段配置参考
```nginx
server {
    listen 443 ssl deferred spdy;
    server_name 120.24.x.x;
    root /data/fm/www;
    ssl on;
    ssl_certificate /data/fm/online/certs/fm.crt;
    ssl_certificate_key /data/fm/online/certs/fm.key;
    ssl_session_cache shared:SSL:50m;
    ssl_session_timeout 5m;
 	...  ...
}
```
> 最后提供一个[免费证书申请](https://buy.wosign.com/free/) 自用也可用选择Let's Encrypt 只是蛋疼的DNS解析，如果这个解决还是非常不错的。
