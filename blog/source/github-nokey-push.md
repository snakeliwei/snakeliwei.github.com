title: Windows下github免密码push
tags:
  - github
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2015-12-02 11:07:41
author: me
cover: "-/images/zsh.png"
---
  一直以来往github push代码都是要填入用户名密码，真是浪费不少时间，现在不用输密码真是酸爽。
----------
 1. 安装Git for windowns.

 2. git clone 项目.

 3. 生成ssh-key.
```{bash}
$ ssh-keygen -t rsa -C "email@email.com"
```
 4. 在用户的.ssh目录下会生成id_rsa和id_rsa.pub两个文件.

 5. 记事本打开id_rsa.pub复制内容粘贴到github ssh-key页面.

 6. 修改推送方法为ssh,再次推送就无需密码了.
 ```{bash}
$ git remote -v
origin  https://github.com/user/Myrepo.git (fetch)
origin  https://github.com/user/Myrepo.git (push)
$ git remote set-url origin git@github.com:user/Myrepo.git
```
