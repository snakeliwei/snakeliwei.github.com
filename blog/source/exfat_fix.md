title: Win/Mac 下修复exFAT 分区
tags:
  - tips
categories:
  - Other
cc: true
hljs: true
comments: true
date: 2017-05-17 17:36:19
---
> exfat分区由于各种原因有些不稳定，下面的方法仅供参考

## Windows 下：
不要用磁盘修复，会提示必须先格式化。
管理员身份打开命令行（Cmd），输入以下代码：
```
chkdsk {drive}: /f
```
例如：chkdsk E: /f 表示修复E 盘

## Mac 下：
过程略复杂，首先尝试用自带的磁盘工具（Disk Utility）修复，如果不成功再执行下面的步骤。
打开终端，输入：
```
sudo fsck_exfat -d diskXsX
```
这里diskXsX 表示要修复的分区，比如disk0s4
会出现一大堆文件列表，最后提示：
```
Main boot region needs to be updated. Yes/No?
```
输入Yes 即可。
最后再回到Disk Utility 去重新修复分区，这次就会成功了。