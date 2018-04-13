title: Ubuntu Nodejs install
tags:
  - Nodejs
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2015-12-08 16:50:51
author: me
cover: "-/images/zsh.png"
---
> 在ubuntu下用apt安装nodejs太坑，现在记录下另外两种安装方式以备用

![Node](/images/nodejs.png)
## 二进制包安装
1. 官网下载二进制包,并解压到相应目录
```bash
$ wget https://nodejs.org/dist/v4.2.3/node-v4.2.3-linux-x64.tar.gz
$ tar xvzf node-v4.2.3-linux-x64.tar.gz
```
2. 设置环境变量
```bash
$ vim ~/.bashrc

#set nodejs env
export NODE_HOME="the Node path"
export PATH=$NODE_HOME/bin:$PATH
export NODE_PATH=$NODE_HOME/lib/node_modules:$PATH
```
3. 设置生效
```bash
$ source ~/.bashrc
```
4. 设置npm国内镜像
```bash
$ npm config set registry http://registry.cnpmjs.org
```

## 源码安装
1. 官网下载源码包,并解压到相应目录
```bash
$ wget https://nodejs.org/dist/v4.2.3/node-v4.2.3.tar.gz
$ tar xvzf node-v4.2.3.tar.gz
```
2. 进入目录进行编译
```bash
$ ./configure --prefix=/usr/local/nodejs
$ make && make install
```
3. 设置环境变量
```bash
$ vim ~/.bashrc
#set nodejs env
export NODE_HOME="the Node path"
export PATH=$NODE_HOME/bin:$PATH
export NODE_PATH=$NODE_HOME/lib/node_modules:$PATH
```
4. 设置生效
```bash
$ source ~/.bashrc
```
5. 设置npm国内镜像
```bash
$ npm config set registry http://registry.cnpmjs.org
```
