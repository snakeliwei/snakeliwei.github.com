title: Linux user expired
tags:
  - Linux
categories:
  - tips 
cc: true
hljs: true
comments: true
date: 2017-10-26 17:00:19
author: me
cover: "-/images/zsh.png"
---
在Linux系统操作中，通常在设置账户的时候是不会设置有效期的，主要在企业中会使用到，当Linux账户过期的时候要怎么延长有效期呢？下面就给大家介绍下Linux账户过期的解决方法，一起来学习下吧。

1. 在添加用户时
`useradd user -e 01/01/2018`

2. 修改用户属性
`usermod -e 01/01/2018 user`

3. 直接调整时间
`chage -E 01/01/2018 user`

### 查看用户过期时间
`chage -l user`