title: Limit cpu usage on linux 
date: 2019-02-21 16:13:51
author: me
cover: ""
tags:
  - cpulimit
  - linux
preview: 使用cpulimit在linux下限制cpu使用率.
---

### 安装方式：
- Build from source

```bash
git clone https://github.com/opsengine/cpulimit.git
cd cpulimit
make
cp src/cpulimit /usr/bin
```

- Install from package manager (etc. apt,yum)

```bash
sudo apt install cpulimit
sudo yum install cpulimit
```

### 使用方法：
输入cpulimit，查看使用方式
#### 选项:
* -l 0~400 限定CPU占据率0%~400%
* -v 显现控制的统计信息
* -z 当被控制的程式退出的时候，CPULimit程式也跟着退出
* -i 同时限定被控程式的子进程
* -p 被控程式的PID
* -e 被控程式的程式称号
 
举例，譬如要限定php-fpm进程的CPU使用率最高为50%，并显现控制信息
```
cpulimit -l 50 -i -v -e php-fpm
```
依据需要挑选能否进入 -z 参数

如果需要后台执行，只需要在command前后进入`nohup……&`，回车便可
```
nohup cpulimit -l 50 -e php-fpm -i -v &
```

> Note. 这个只能作为临时方法使用，还是要找出占用高的根本原因，从根本入手解决cpu占用过高的问题
