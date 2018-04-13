title: Apk/ipk download config for nginx 
tags:
  - nginx
  - tips
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2016-06-12 17:20:19
author: me
cover: "-/images/zsh.png"
---
最近app百石堂上线后，添加了不少android和iphone的应用下载，但发现了一个问题：在使用nginx提供下载.ipa或.apk文件时会出现以下问题：通过IE浏览器下载会出现替换扩展名为.zip；而使用火狐浏览器下载则会出现流的形式，就是不会下载，而是以乱码的形式浏览，显然这都不是我们想要的。
下面我们先来了解下 nginx下conf/mime.types内各类型文件头信息：

```
text/vnd.sun.j2me.app-descriptor      jad;   
application/java-archive              jar war ear;   
application/x-java-archive-diff       jardiff;   
application/vnd.android.package-archive apk;   
application/vnd.ms-cab-compressed     cab;   
application/octet-stream              bin exe dll;   
application/vnd.symbian.install       sis;   
x-epoc/x-sisx-app                     sisx;   
application/iphone                    pxl ipa;   
application/vnd.palm                  prc pdb;   
application/vnd.webos.ipk             ipk;   
application/vnd.rim.cod               cod;   
application/mrp                       mrp;   
x-nokia-widget                        wgz;   
application/octet-stream              deb;
```
>目前许多提供下载.apk和.ipa的地方，用IE浏览器下载完后文件自动变成了zip后缀，导致无法使用，只要在nginx的conf/mime.typs内加上

```
application/vnd.android.package-archive apk;   
application/iphone          pxl ipa;
```
