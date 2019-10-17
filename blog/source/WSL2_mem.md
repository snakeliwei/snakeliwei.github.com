title: WSL 2 consumes massive amounts of RAM
date: 2019-10-17 12:13:51 +0800
update: 2019-10-17 13:00:00 +0800
author: me
cover: ""
tags:
  - WSL2
  - windows
preview: wsl2出现Vmmem内存占用过大问题解决方法。
---

确定自己win10版本高于大于18945 查看方法运行中执行winver [版本 10.0.xxxxx.*]，其中xxxxx大于18945即可
在 %UserProfile% 文件夹下创建.wslconfig文件，内容为:

```
[wsl2]
memory=4GB
swap=0
localhostForwarding=true
```

其中4GB为制作Vmmem进程使用内存的大小，然后保存即可最好保存成ANSI编码或者UTF-8格式
然后关闭linux子系统然后在cmd运行 wsl --shutdown 后再次打开linux子系统即可生效


[1]https://github.com/microsoft/WSL/issues/4166

[2]https://docs.microsoft.com/en-us/windows/wsl/release-notes#build-18945
