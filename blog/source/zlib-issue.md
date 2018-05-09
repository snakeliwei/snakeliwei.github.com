title: Docker拉镜像时出version `ZLIB_1.2.5.1' not found的解决方法
tags:
  - linux
categories:
  - tips
cc: true
hljs: true
comments: true
date: 2018-04-08 16:33:51
author: me
---
> 今天在更新系统包后在拉取docker image 时莫名出现'libz.so.1: version `ZLIB_1.2.5.1' not found'的错误，解决了顺便记录下。

## docker pull 出错：
```bash
deploy@DEV:~ $ docker pull redis:4-alpine
4-alpine: Pulling from library/redis
ff3a5c916c92: Extracting [==================================================>]  2.066MB/2.066MB
aae70a2e6027: Download complete
87c655da471c: Download complete
7f8fb829cc48: Download complete
c72e0cff027d: Download complete
276d6b52cd5b: Download complete
4-alpine: Pulling from library/redis
ff3a5c916c92: Extracting [==================================================>]  2.066MB/2.066MB
aae70a2e6027: Download complete
87c655da471c: Download complete
7f8fb829cc48: Download complete
c72e0cff027d: Download complete
276d6b52cd5b: Download complete
failed to register layer: Error processing tar file(exit status 1: /usr/bin/unpigz: /usr/local/lib/libz.so.1: version `ZLIB_1.2.5.1' not found (required by /usr/bin/unpigz)
):
```
## 解决方法:
```bash
wget http://zlib.net/zlib-1.2.11.tar.gz
tar zxf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make && make install
```
