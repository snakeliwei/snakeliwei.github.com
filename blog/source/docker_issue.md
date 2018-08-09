title: "Docker config remote API via daemon.json issue"
date: 2018-08-09 11:00:00 +0800
update: 2018-08-09 11:00:00 +0800
author: me
cover: "-/images/docker.jpeg"
tags:
    - Docker
    - Tips
preview: Docker config remote API via daemon.json issue

---

> 使用daemon.json配置docker remote API时出现以下错误:

`Aug 09 10:38:49 node02 dockerd[1169]: unable to configure the Docker daemon with file /etc/docker/daemon.json: the following directives are specified both as a flag and in the configuration file: hosts: (from flag: [fd://], from file: [
unix:///var/run/docker.sock tcp://0.0.0.0:2376])`

## 解决方法：
1. Use the command sudo systemctl edit docker.service to open an override file for docker.service in a text editor.

2. Add or modify the following lines, substituting your own values.
```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd
```
3. Save the file.

4. Reload the systemctl configuration.

`$ sudo systemctl daemon-reload`

5. Restart Docker.
`$ sudo systemctl restart docker.service`
6. Check to see whether the change was honored by reviewing the output of netstat to confirm dockerd is listening on the configured port.
```
$ sudo netstat -lntp | grep dockerd
tcp        0      0 0.0.0.0:2376          0.0.0.0:*               LISTEN      3758/dockerd
```

