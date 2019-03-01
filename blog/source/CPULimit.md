title: Limit cpu usage on linux 
date: 2019-02-21 16:13:51 +0800
update: 2019-03-01 17:00:00 +0800
author: me
cover: ""
tags:
  - cpulimit
  - linux
preview: 在linux下限制程序的cpu使用率。
---

## 使用systemd限制cpu占用

> 在基于 Linux-3.x 内核版本的很多发行版都提供了 Systemd 来管理系统和服务。同时也将 cgroup 功能加到了 slice, scope 和 service 三个单元中, 基于这些特性我们可以很方便的通过 systemd 来限制服务或者进程对系统资源的使用, 这在单主机多服务的场景下会很有用。下面则以 MySQL 服务为例介绍如何使用 systemd 限制资源的使用, 其它服务的限制和此等同。

```bash
# systemctl set-property mysql.service MemoryLimit=5G       # 5G 内存
# systemctl set-property mysql.service CPUQuota=150%        # 150% cpu 使用率
# systemctl set-property mysql.service BlockIOWeight=1000   # IO 权重
```

## 使用cpulimit程序限制cpu占用

> 对于没用使用Systemd来管理的发行版可用cpulimit这个程序来实现cpu占用的限制。

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
