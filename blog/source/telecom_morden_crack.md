title: 获取烽火光猫超级用户
date: 2020-03-18 11:13:51 +0800
update: 2020-03-18 12:00:00 +0800
author: me
cover: ""
tags:
  - hash
  - windows
preview: 烽火光猫超级用户获取收集。
---

## 烽火HG2201T

### 获取配置信息

* 管理后台 ip:8080/cgi-bin/login.htm.cgi
* 备份信息 ip:8080/cgi-bin/baseinfoSet.cgi
* 设备信息、桥接模式 ip:8080/cgi-bin/baseinfo.cgi

> 超级管理员密码在备份信息中, 找到telecomadmin密码会是这样==>telecomadmin&49&48&55&56&56&53&51&51用下方ascii编码转换得到真正密码telecomadmin10788533

```
&48=(0) 
&49=(1) 
&50=(2)
&51=(3) 
&52=(4）
&53=(5）
&54（6）
&55=(7）
&56=(8）
&57=(9）
```

## 烽火HG221GI 
	
1. 在浏览器中(建议chrome)用useradmin登录http://192.168.1.1管理页面.
2. 然后在url中输入http://192.168.1.1/backupsettings.conf, 会将配置文件下载到本地.
3. 文本编辑器打开下载的backupsettings.conf, 找到如下选项就是telecomadmin的密码, 密码用base64存储需要解码.
```
      <X_CT-COM_TeleComAccount>
        <Password>dGVsZWNvbWFkbWluMzkyOTk4NTgA</Password>
      </X_CT-COM_TeleComAccount>
```

