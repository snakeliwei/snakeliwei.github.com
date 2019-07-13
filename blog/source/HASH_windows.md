title: Hash cmd on win
date: 2019-07-05 16:13:51 +0800
update: 2019-07-05 17:00:00 +0800
author: me
cover: ""
tags:
  - hash
  - windows
preview: win下的hash命令。
---

## Windows下查看文件hash

> 使用certutil命令查看文件的hash

```cmd
certutil -hashfile filename MD5
certutil -hashfile filename SHA1
certutil -hashfile filename SHA256
```
