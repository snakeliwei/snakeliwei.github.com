title: Mount the EFI partition on win 10
date: 2019-04-02 16:13:51 +0800
update: 2019-04-02 17:00:00 +0800
author: me
cover: ""
tags:
  - windows
  - EFI
  - tips
preview: win10下挂载EFI分区
---

## 挂载EFI分区

- Windows操作系统下面,以系统管理员身份打开cmd窗口,输入命令:

```bash
c:\>diskpart
list disk           # 磁盘列表
select disk n       # 选择EFI分区所在的磁盘，n为磁盘号
list partition      # 磁盘分区列表
select partition n  # 选择EFI分区，n为EFI分区号
set id="ebd0a0a2-b9e5-4433-87c0-68b6b72699c7"        # 设置为基本数据分区
assign letter=X     # x为EFI分区盘符
exit                                # 退出diskpart
notepad                                # 打开记事本程序，点击文件->打开，即可访问EFI分区
```
