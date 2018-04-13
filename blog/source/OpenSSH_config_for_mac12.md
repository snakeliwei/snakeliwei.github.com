title: OpenSSH config for mac 10.12.2
tags:
  - openssh
  - tips
categories:
  - Work
cc: true
hljs: true
comments: true
date: 2017-05-05 09:36:19
author: me
cover: "-/images/zsh.png"
---
之前的mac os中添加私钥到keychain即可生效，重启后也生效，现在到了12.2后apple做了更新，重启后失效了。
>在官网找到了解决办法如下，在.ssh/config中配置

```
Host test
  HostName test.eg.com
  port 55522
  user deploy
  IdentityFile ~/.ssh/id_rsa
  UseKeychain yes     # 使用keychain 
  AddKeysToAgent yes  # ssh-agent自动加载
```
