title: "bash ‘/bin/sh: Syntax error: “(” unexpected"
date: 2018-11-28 10:00:00 +0800
update: 2018-11-28 10:00:00 +0800
author: me
cover: ""
tags:
    - tips
    - linux
    - bash
preview: bash另类报错

---

## 问题复现

今天在服务器上执行bash时报错`‘/bin/sh: Syntax error: “(” unexpected`
折腾了好久就是因为用的是linux将sh指向了dash而不是bash，so。。兼容就有问题了

查看当前指向就是:

```bash
echo $SHELL
```
一般在使用bash特性的脚本前写(推荐后面的写法):

`#!/bin/bash` or `#!/usr/bin/env bash`
> Using #!/usr/bin/env NAME makes the shell search for the first match of NAME in the $PATH environment variable. It can be useful if you aren't aware of the absolute path or don't want to search for it.

## 解决方案

但是有时一些库就比较麻烦了,所以干脆直接修改指向

```bash
dpkg-reconfigure dash
```

弹出窗口选NO，关掉就OK了
