title: 给Linux开启BBR
tags:
  - linux
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2017-05-18 17:33:51
author: me
cover: "-/images/bbr.png"
---
> 最近，Google 开源了其 TCP BBR 拥塞控制算法，并提交到了 Linux 内核，最新的 4.11 版内核已经用上了该算法。根据以往的传统，Google 总是先在自家的生产环境上线运用后，才会将代码开源，此次也不例外。
根据实地测试，在部署了最新版内核并开启了 TCP BBR 的机器上，网速甚至可以提升好几个数量级。

## 手动安装：
Ubuntu系统下载对应版本和架构（amd64）的[内核镜像](http://kernel.ubuntu.com/~kernel-ppa/mainline/)（linux-image.+_amd64.deb）、内核头文件（linux-headers.+.deb），然后用 dpkg 安装并重启系统。
重启进入4.11.x 的内核之后，执行如下命令开启 BBR：
```
sudo bash -c 'echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf'
sudo bash -c 'echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf'
sudo sysctl -p
```
执行如下命令确认 BBR 成功开启，两个命令的预期输出都是 1：
```
sysctl net.ipv4.tcp_available_congestion_control | grep bbr | wc -l
lsmod | grep tcp_bbr | wc -l
```
开启 BBR 的效果还是蛮明显的，浏览器关闭缓存访问博客，完整加载耗时缩短将近一半。

## 懒人一键安装:
```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
chmod +x bbr.sh
./bbr.sh
```
