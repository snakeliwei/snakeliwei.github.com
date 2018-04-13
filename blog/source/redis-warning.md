title: Redis startup warning
tags:
  - Redis
categories:
  - resolution 
cc: true
hljs: true
comments: true
date: 2017-10-23 09:36:19
author: me
cover: "-/images/zsh.png"
---
Redis 启动时会遇到以下警告，对于强迫症患者实在是不能忍，下面给出消除方法.

```
26069:M 08 Aug 17:06:58.858 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
26069:M 08 Aug 17:06:58.859 # Server started, Redis version 3.0.7
26069:M 08 Aug 17:06:58.859 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
26069:M 08 Aug 17:06:58.859 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
26069:M 08 Aug 17:06:58.978 * DB loaded from disk: 0.119 seconds
```

* 分别执行如下语句：
```
echo 511 > /proc/sys/net/core/somaxconn
echo "vm.overcommit_memory = 1" >> /etc/sysctl.conf
sysctl vm.overcommit_memory=1
echo never > /sys/kernel/mm/transparent_hugepage/enabled
```
* 保证重启后也生效需要再在/etc/rc.local的最后添加
echo never > /sys/kernel/mm/transparent_hugepage/enabled