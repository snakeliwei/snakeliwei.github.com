title: Ansible study note 00x01
tags:
  - Ansible
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2016-07-11 16:30:19
author: me
cover: "-/images/avatar.png"
---
> 为了更好的学习自动化运维，就必须学习配置管理工具，Ansible 比较好入门所以从它开始

## 安装和配置

### 安装
```bash
sudo pip install ansible
```
创建并编辑ansible的hosts文件，内容为已经配置ssh免密码登录的远程主机的地址。
/etc/ansible/hosts是默认路径
```bash
sudo mkdir /etc/ansible
sudo vi /etc/ansible/hosts
```
内容为INI格式：
```ini
192.168.1.134
192.168.1.136
```
也可以进行分组，同一个机器可以在不同分组
```ini
[v1]
192.168.1.134

[v2]
192.168.1.136
```
如果远程的ssh端口修改了不在22标准端口，文件内容中机器地址应该明确指定端口号如：```192.168.1.134:2222```

配置ssh免密码登录远程主机：

当前用户名与远程机器上相同，ssh key在默认位置
```bash
ssh-copy-id 192.168.1.136
ssh-copy-id 192.168.1.134
```
避免重复输入密钥的短语密码：
```bash
ssh-add ~/.ssh/id_rsa
```
在本地测试是否能够ping通：

ping全部机器
```bash
ansible all -m ping
```

以ashin用户身份ping .134
```bash
ansible 192.168.1.134 -m ping -u ashin
```

以用户ashin身份使用sudo来ping v1 (.134) -K是输入root密码
```bash
ansible v1 -m ping -u ashin --sudo -K
```
### 配置
配置文件可以从多个地方加载，其优先级顺序为：
```
ANSIBLE_CONFIG (环境变量)
ansible.cfg (当前目录)
.ansible.cfg (home目录)
/etc/ansible/ansible.cfg
```
定义自己的配置文件可以参考https://raw.githubusercontent.com/ansible/ansible/devel/examples/ansible.cfg

ssh检查key是通过paramiko进行的，很慢。 不检查host key配置：
```bash
sudo vi etc/ansible/ansible.cfg or ~/.ansible.cfg
```
内容为：
```ini
[defaults]
host_key_checking = False
```
