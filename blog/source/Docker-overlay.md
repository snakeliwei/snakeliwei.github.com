title: Docker use overlay driver
tags:
  - Docker
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2015-12-03 14:36:19
author: me
cover: "-/images/avatar.png"
---
![overlay](http://docs.daocloud.io/images/c/0/3/5/1/c03512523e75f366ecf8f40442cbd8b46b227d28-3.png)
OverlayFS之前已经加入到了Ubuntu内核中，但是那并不是我们想要的。Overlay（没有FS）是一个不同的内核模块，因此你需要安装3.18（或者以上）的内核，Docker你需要安装Docker 1.4或者更高版本。
1. 使用Docker info查看Docker运行信息

```
$ docker info
Containers: 0
Images: 6
Server Version: 1.9.1
Storage Driver: aufs
 Backing Filesystem: extfs
Execution Driver: native-0.2
Logging Driver: json-file
Kernel Version: 3.19.0-26-generic
Operating System: Ubuntu 14.04.3 LTS
CPUs: 4
Total Memory: 7.69 GiB
Name: DEV
ID: CKAS:JEPR:SA3D:WA54:YQ6U:36RS:R5BO:7CXV:NZDF:PPY4:ORES:UY3H
WARNING: No swap limit support
```
2. 确认内核>3.18并且模块overlay有没有被启用,如果"$ lsmod | grep overlay"没有返回,可以使用"$ modprobe overlay"加载内核模块 
```
$ uname -r
3.19.0-21-generic

$ lsmod | grep overlay
overlay
```
3. For ubuntu ：在/etc/default/docker中给DOCKER_OPTS设置-s overlay
```
# Use DOCKER_OPTS to modify the daemon startup options.
DOCKER_OPTS = "-s overlay"
```

4. For Centos7 : 编辑/etc/sysconfig/docker和/usr/lib/systemd/system/docker.service
```
# /etc/sysconfig/docker
# Modify these options if you want to change the way the docker daemon runs
OPTIONS="--storage-driver overlay"

# /usr/lib/systemd/system/docker.service
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network.target docker.socket
Requires=docker.socket

[Service]
Type=notify
EnvironmentFile=-/etc/sysconfig/docker
ExecStart=/usr/bin/docker daemon -H fd:// $OPTIONS
MountFlags=slave
LimitNOFILE=1048576
LimitNPROC=1048576
LimitCORE=infinity

[Install]
WantedBy=multi-user.target
```

5. 重启docker服务
```
$ sudo service docker restart
```
6. 使用Docker info查看Docker运行信息 Storage Driver 已经使用overlay
```
$ docker info
Containers: 0
Images: 6
Server Version: 1.9.1
Storage Driver: overlay
 Backing Filesystem: extfs
Execution Driver: native-0.2
Logging Driver: json-file
Kernel Version: 3.19.0-26-generic
Operating System: Ubuntu 14.04.3 LTS
CPUs: 4
Total Memory: 7.69 GiB
Name: DEV
ID: CKAS:JEPR:SA3D:WA54:YQ6U:36RS:R5BO:7CXV:NZDF:PPY4:ORES:UY3H
WARNING: No swap limit support
```
