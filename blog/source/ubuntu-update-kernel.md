title: Ubuntu upgrade kernel to v4
tags:
  - Linux
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2016-01-11 10:36:19
author: me
cover: "-/images/zsh.png"
---
Ubuntu 14.04 的内核过于老旧，项目需要升级内核，现在把更新方式记录下来，以便不时之需.

1. 查看当前内核版本
```
$ uname -r
3.19.0-21-generic
```

2. 从[kernel.ubuntu.com](http://kernel.ubuntu.com/~kernel-ppa/mainline/)选择相应的内核版本文件下载到本地
```
$ wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4-wily/linux-headers-4.4.0-040400_4.4.0-040400.201601101930_all.deb
$ wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4-wily/linux-image-4.4.0-040400-generic_4.4.0-040400.201601101930_amd64.deb
$ wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4-wily/linux-headers-4.4.0-040400-generic_4.4.0-040400.201601101930_amd64.deb
```
3. 在下载目录执行安装
```
$ sudo dpkg -i linux-headers-4.4.0*.deb linux-image-4.4.0*.deb 
```
4. 更新GRUB后重启系统
```
$ sudo update-grub
```
5. 查看当前系统内核，删除旧的内核
```
$ uname -r
4.4.0-040400-generic

$ sudo apt-get remove linux-image-3.19.0-21-generic linux-headers-3.19.0-21 linux-headers-3.19.0-21-generic
```
