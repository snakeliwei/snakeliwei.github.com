title: After Docker Install
tags:
  - Docker
  - linux
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2017-03-02 15:33:51
author: me
cover: "-/images/zsh.png"
---
> 在docker安装完毕后，需要进行必要的设置以便更好的为我们服务

## 安装Docker
推荐使用阿里云的国内源进行安装，原因你们知道的GFW，另外就是很快很方便[传送门](http://mirrors.aliyun.com/help/docker-engine)

## 消除"WARNING: No swap limit support"
这个默认官方给予解决方法了:[链接](https://docs.docker.com/engine/installation/linux/ubuntu/)
```
When users run Docker, they may see these messages when working with an image:

WARNING: Your kernel does not support cgroup swap limit. WARNING: Your
kernel does not support swap limit capabilities. Limitation discarded.
To prevent these messages, enable memory and swap accounting on your system. To enable these on system using GNU GRUB (GNU GRand Unified Bootloader), do the following.

Log into Ubuntu as a user with sudo privileges.

Edit the /etc/default/grub file.

Set the GRUB_CMDLINE_LINUX value as follows:

GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
Save and close the file.

Update GRUB.

$ sudo update-grub
Reboot your system.
```

## 配置加速器，使用overlay2，启用Live Restore
以ubuntu为例，其他发行版请自行查看[官方文档](https://docs.docker.com)
> 使用overlay需要内核版本在3.18以上，并且在内核中已启用[传送门](http://snakeliwei.github.io/2015/12/03/Docker-overlay/)。启用Live Restore后在更新docker daemon时不影响已启动的容器。

- Docker客户端版本大于1.10的用户
修改daemon配置文件/etc/docker/daemon.json：

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://knb1nxmo.mirror.aliyuncs.com"],
  "storage-driver": "overlay2", //使用第二版驱动
  "live-restore": true
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

- Docker客户的版本小于等于1.10的用户或者想配置启动参数，可以使用下面的命令将配置添加到docker daemon的启动参数中。

1. Ubuntu 12.04 14.04的用户

```bash
echo "DOCKER_OPTS=\"\$DOCKER_OPTS --registry-mirror=https://knb1nxmo.mirror.aliyuncs.com -s overlay --live-restore=true \"" | sudo tee -a /etc/default/docker
sudo service docker restart
```

2. Ubuntu 15.04 16.04的用户

```bash
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo tee /etc/systemd/system/docker.service.d/mirror.conf <<-'EOF'
[Service]
ExecStart=/usr/bin/docker daemon -H fd:// --registry-mirror=https://knb1nxmo.mirror.aliyuncs.com -s overlay --live-restore=true
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

