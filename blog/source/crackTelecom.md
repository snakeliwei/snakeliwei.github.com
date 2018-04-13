title: 大亚光猫破解
tags:
  - net
categories:
  - other
cc: true
hljs: true
comments: true
date: 2016-05-12 09:36:19
author: me
cover: "-/images/avatar.png"
---
>使用电信的光纤送的猫都使用的是内部拨号，想使用路由桥接拨号必须用超级管理员登录改配置

## 方法1：
1. 光猫背面有useradmin密码，用这个账号登录192.168.1.1，地址栏输入http://192.168.1.1/backupsettings.conf
2. 下载backupsettings.conf

## 方法2：
1. 把电脑IP设为 192.168.1.10 在操作电脑上安装 Cisco TFTP Server  ,开启 TFTP服务
2. 下载putty，用 PUTTY 登陆 192.168.1.1   用户：admin  密码:admin 或是 空格
3. 在SSH模式下输入 tftp -p -f backupsettings.conf 192.168.1.10  （就是下载光猫上的backupsettings.conf文件）


>backupsettings.conf里面会存有超级用户密码找到 telecomadmin开头的这行，Password中间的那个就是密码（密码一般是telecomadmin+后面的数字）
