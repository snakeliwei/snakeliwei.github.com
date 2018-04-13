title: Use nvm to manage node
tags:
  - Nodejs
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2017-07-17 15:10:51
author: me
cover: "-/images/nodejs.png"
---
> 使用NVM来管理node的版本，并使用国内镜像安装

![Node](/images/nodejs.png)
## 安装[NVM](https://github.com/creationix/nvm)
### 从GITHUB获取最新链接
* Use curl
```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```
* or use wget
```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```

## 设置环境变量使用国内node镜像
* Set nvm mirrors
```bash
$ vim ~/.bashrc
# set nodejs env
export NVM_NODEJS_ORG_MIRROR=http://npm.taobao.org/mirrors/node
```
* Take it effect
```bash
$ source ~/.bashrc
```
* Set npm mirrors
```bash
$ npm config set registry https://registry.npm.taobao.org
```
