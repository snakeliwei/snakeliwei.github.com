title: 使用scp再linux间拷贝文件和目录
tags:
- linux
- scp
categories:
- Study
cc: true
hljs: true
comments: true
date: 2017-06-08 14:33:51
author: me
cover: "-/images/zsh.png"
---
> scp命令可以在linux之间复制文件和目录 

### 命令基本格式
`scp [option] file_source file_target`

#### 从本地复制到远程
```
scp local_file remote_username@remote_ip:remote_folder
scp local_file remote_username@remote_ip:remote_file
scp local_file remote_ip:remote_folder
scp local_file remote_ip:remote_file
```
第1,2个指定了用户名，命令执行后需要再输入密码，第1个仅指定了远程的目录，文件名字不变，第2个指定了文件名；
第3,4个没有指定用户名，命令执行后需要输入用户名和密码，第3个仅指定了远程的目录，文件名字不变，第4个指定了文件名；
```
scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music 
scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music/001.mp3 
scp /home/space/music/1.mp3 www.cumt.edu.cn:/home/root/others/music 
scp /home/space/music/1.mp3 www.cumt.edu.cn:/home/root/others/music/001.mp3 
```
* 复制目录： 
```
scp -r local_folder remote_username@remote_ip:remote_folder 
scp -r local_folder remote_ip:remote_folder 
```
第1个指定了用户名，命令执行后需要再输入密码； 
第2个没有指定用户名，命令执行后需要输入用户名和密码； 
```
scp -r /home/space/music/ root@www.cumt.edu.cn:/home/root/others/ 
scp -r /home/space/music/ www.cumt.edu.cn:/home/root/others/ 
```
#### 从远程复制到本地
从远程复制到本地, 只要将从本地复制到远程的命令的后2个参数调换顺序即可；
```
scp root@www.cumt.edu.cn:/home/root/others/music /home/space/music/1.mp3 
scp -r www.cumt.edu.cn:/home/root/others/ /home/space/music/
```
最简单的应用如下: `scp user@IP:filename remoteuser@IP:filename`

*可能有用的几个参数: *
+ -v 和大多数linux命令中的-v意思一样, 用来显示进度.可以用来查看连接, 认证或是配置错误.
+ -C 使能压缩选项.
+ -P 选择端口. 注意-p已经被rcp使用.
+ -4 强行使用IPV4地址.
+ -6 强行使用IPV6地址.


*注意两点*
1. 如果远程服务器防火墙有特殊限制，scp要走特殊端口，命令格式如下: 

`scp -p 4588 remote@www.abc.com:/usr/local/sin.sh /home/administrator`

2. 使用scp要注意所使用的用户是否具有可读取远程服务器相应文件的权限。
