title: Run an exe app as service on win os
date: 2019-04-04 16:13:51 +0800
update: 2019-04-04 17:00:00 +0800
author: me
cover: ""
tags:
  - windows
  - service
  - tips
preview: win10下挂将exe应用运行成后台服务
---

> 项目服务用java写的，跑在console下非常不安全，如遇意外挂掉没法自动拉起。需要类似superviser的守护软件管理起来，
[NSSM](http://www.nssm.cc)就是这类在win下将普通程序运行成后台程序的软件

## 服务安装：

服务安装可以使用如下命令： `nssm install <servicename>`

执行此命令后，会出现一个界面，基本上看着就知道怎么用了，大多数情况下，只需要填第一个界面的程序路径就可以了。

其它界面的是高级参数的配置，可以根据需要自行选择。

参数填完后执行"install service"按钮即可将服务安装到系统，可以使用系统的服务管理工具查看了。

当然，如果要自动化安装，可以直接带上程序路径： `nssm install <servicename> <program> [<arguments>]`

NSSM本身win7及以上的系统基本都是支持的，我测试过win7，2008,2016系统，都是没有问题的，如果安装失败，请首先检查是否装了某国产管家或国产杀毒软件。

安装完成后，服务还没有启动，需要通过下面的服务管理的命令启动服务。

服务管理：

服务管理主要有启动、停止和重启，其命令如下：

启动服务：　nssm start <servicename>
停止服务： nssm stop <servicename>
重启服务:    nssm restart <servicename>
当然，也可以使用系统自带的服务管理器操作和使用系统的命令。

修改参数：

NSSM安装的服务修改起来非常方便，命令如下：

`nssm edit <servicename>`

会自动启动操作界面，直接更改即可。

## 服务删除：

服务删除可以使用如下命令之一：

```bash
nssm remove <servicename>
nssm remove <servicename> confirm
```

功能没有大的区别，后面的命令是自动确认的，没有交互界面。

## 命令行：

服务自动化需要使用更多的命令行，具体参看官方文档： Managing services from the command line
如下是一个安装Jenkins服务的示例：

```bash
nssm install Jenkins %PROGRAMFILES%\Java\jre7\bin\java.exe
nssm set Jenkins AppParameters -jar slave.jar -jnlpUrl https://jenkins/computer/%COMPUTERNAME%/slave-agent.jnlp -secret redacted
nssm set Jenkins AppDirectory C:\Jenkins
nssm set Jenkins AppStdout C:\Jenkins\jenkins.log
nssm set Jenkins AppStderr C:\Jenkins\jenkins.log
nssm set Jenkins AppStopMethodSkip 6
nssm set Jenkins AppStopMethodConsole 1000
nssm set Jenkins AppThrottle 5000
nssm start Jenkins
```
