title: "fatal error: openssl/opensslv.h: No such file or directory"
tags:
  - Openssl
  - tips
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2016-07-07 14:36:19
author: me
cover: "-/images/zsh.png"
---
> 出现这个或者fatal error: openssl/*.h: No such file or directory。都是没有安装libssl-dev
  libssl-dev包含libraries, header files and manpages，他是openssl的一部分，而openssl对ssl进行了实现～

### 解决方案:
```
sudo apt-get install libssl-dev
```
安装libssl-dev即可
