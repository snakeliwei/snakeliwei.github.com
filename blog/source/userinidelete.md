title: root删不掉的.user.ini文件
date: 2019-03-01 16:13:51
author: me
cover: ""
tags:
  - tips
  - linux
preview: PHP项目里有个神秘的.user.ini文件，用root都删不掉.
---

啥？ .user.ini 删不掉，是不是觉得自己真的不行了？
```
[root@localhost]# rm -rf .user.ini 
rm: cannot remove ‘.user.ini’: Operation not permitted
```

来看一下，该文件的属性
```
[root@Tech1024]# lsattr .user.ini 
----i----------- .user.ini
```
没错，文件被锁定了，不能修改，那么我们去除文件锁定属性
```
[root@Tech1024]# chattr -i .user.ini
是不是可以删除了，你是不是喜极而泣，啊，自己终于又行了。
```
> Note. lsattr和chattr真是对好基友
