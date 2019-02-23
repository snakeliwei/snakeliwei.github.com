title: Use iperf to test Lan speeds...
date: 2019-02-23 12:13:51
author: me
cover: ""
tags:
  - iperf
  - linux
preview: 用iperf测试内网连接速度.
---

### 安装方式：
- Build from source

```bash
[root@localhost]# curl -SOL https://github.com/esnet/iperf/archive/3.6.tar.gz
[root@localhost]# tar -xvf 3.6.tar.gz
[root@localhost]# cd 3.6 && ./configure
[root@localhost]# make
[root@localhost]# make install
```
- Install from package manager (etc. apt,yum)

```bash
sudo apt install iperf3
sudo yum install iperf3
```

### 使用方法：

- 参数说明
-s 以server模式启动，eg：iperf -s
-c host以client模式启动，host是server端地址，eg：iperf -c 222.35.11.23

- 通用参数
-f [kmKM] 分别表示以Kbits, Mbits, KBytes, MBytes显示报告，默认以Mbits为单位,eg：`iperf -c 222.35.11.23 -f K`
-i sec 以秒为单位显示报告间隔，eg：`iperf -c 222.35.11.23 -i 2`
-l 缓冲区大小，默认是8KB,eg：iperf -c 222.35.11.23 -l 16
-m 显示tcp最大mtu值
-o 将报告和错误信息输出到文件 eg：`iperf -c 222.35.11.23 -o ciperflog.txt`
-p 指定服务器端使用的端口或客户端所连接的端口eg：`iperf -s -p 9999;iperf -c 222.35.11.23 -p 9999`
-u 使用udp协议
-w 指定TCP窗口大小，默认是8KB
-B 绑定一个主机地址或接口（当主机有多个地址或接口时使用该参数）
-C 兼容旧版本（当server端和client端版本不一样时使用）
-M 设定TCP数据包的最大mtu值
-N 设定TCP不延时
-V 传输ipv6数据包
 
### 实例：
- 服务端
```
[root@localhost bin]# iperf3 -s
-----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
```
- 客户端:
```
[root@localhost iperf-3.0.5]# iperf3 -c 192.168.0.1 --reverse --parallel 4
```
(192.168.0.1 服务器的IP地址，可设置为推流网卡IP)
(--reverse表示服务端发送, 客户端接收;如果不加，就是客户端发送，服务端接收 --parallel 4表示4路并发流)

> issue: iperf3: error - unable to connect to server: No route to host  --- 这个错误要检查防火墙是否放行默认5201端口.

> issue: iperf3: error while loading shared libraries: libiperf.so.0: cannot open shared object file: No such file or directory
问题原因：Linux系统中找不到libiperf.so.0 库文件，导致执行iperf3 –s时提示缺少相关lib库
解决方法：通过find /usr/local/lib/ |grep iperf查找其他服务器上是否存在该lib库文件，查询到后拷贝libiperf.so.0库文件到此台服务器/usr/local/lib/目录下
