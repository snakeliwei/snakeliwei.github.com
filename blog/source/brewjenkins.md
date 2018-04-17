title: "Mac下安装jenkins并配置服务启动"
date: 2018-04-17 16:00:00 +0800
update: 2018-04-17 16:00:00 +0800
author: me
cover: ""
tags:
    - Mac
    - Jenkins
preview: Mac下安装jenkins并配置服务启动

---

## 使用brew安装jenkins
`brew install jenkins`

## 启动，停止，重启jenkins
```bash
brew services start jenkins
brew services stop jenkins
brew services restart jenkins
```
## 外部访问jenkins
> 使用brew安装jenkins会避免很多其他安装方式产生的用户权限问题，但是会将httpListenAddress默认设置为127.0.0.1，我们需要提供外部访问有两种方法。

1. 修改两个路径下的plist配置中的httpListenAddress后的ip地址为`0.0.0.0`并重启
```
～/Library/LaunchAgents/homebrew.mxcl.jenkins.plist
/usr/local/opt/jenkins/homebrew.mxcl.jenkins.plist
```
2. 使用nginx等反向代理一下本地服务即可