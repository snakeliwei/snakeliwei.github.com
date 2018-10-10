title: "linux-headers-4.18.12-041812-generic : Depends: libssl1.1 (>= 1.1.0) but it is not installable"
date: 2018-10-10 17:00:00 +0800
update: 2018-10-10 17:00:00 +0800
author: me
cover: ""
tags:
    - tips
    - linux
    - ssl
preview: ubuntu升级内核报libssl1.1依赖问题

---

## 问题复现

今天再升级ubuntu 16.04的内核时出现依赖libssl1.1.0问题

```bash
Reading package lists... Done
Building dependency tree
Reading state information... Done
You might want to run 'apt-get -f install' to correct these:
The following packages have unmet dependencies:
 linux-headers-4.18.12-041812-generic : Depends: libssl1.1 (>= 1.1.0) but it is not installable
E: Unmet dependencies. Try 'apt-get -f install' with no packages (or specify a solution).
```

## 解决方案

单独下载安装一个libssl1.1_1.1.0g-2ubuntu4.1_amd64.deb文件然后再升级就可以了

```bash
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4_amd64.deb
sudo dpkg -i libssl1.1_1.1.0g-2ubuntu4_amd64.deb
```