title: Git https push&pull with no password
tags:
  - git
  - tips
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2016-03-11 14:36:19
author: me
cover: "-/images/zsh.png"
---
在大家使用github的过程中，https方式每次要push 和pull时总是要输入github的账号和密码，这样不仅浪费了大量的时间且降低了工作效率。
在网上发现以下方法可以免输入帐号密码：

```bash
$ cd ~
$ touch .git-credentials
$ echo "https://{username}:{password}@github.com" > .git-credentials  //使用自己的帐号密码替换{username} {password}
$ git config --global credential.helper store
```

然后再执行git push/pull无需再输入用户名和密码.
