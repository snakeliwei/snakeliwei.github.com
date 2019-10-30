title: Use tinypng api to compress image via python
date: 2019-10-30 9:13:51 +0800
update: 2019-10-30 9:20:00 +0800
author: me
cover: ""
tags:
  - python
  - png
  - img
preview: 写了一个适用tingpng api 批量压缩图片的脚本只用了几行, 真是快艹猛。
---

## 什么是Tinypng

Tinypng 网站提供在线图片压缩服务，是所有图片压缩工具中最好用的之一，但有限制：批量最多处理 20 张，且每张大小不允许超过 5M。
但网站同时也提供了开放了免费的 API ，API 取消了每张大小的限制，只限定每个月处理 500 张图片。

## 如何使用API

1. 在它网站上注册，获得专属的 API_KEY。
2. 然后是安装 package：
```
pip install --upgrade tinify
```
3. 接着上代码:
```
import tinify
import os

tinify.key = 'api_key'
path = "d:\\images\\photo" # 图片存放的路径

for dirpath, dirs, files in os.walk(path):
    for file in files:
        imgpath = os.path.join(dirpath, file)
        print("compressing ..."+ imgpath)
        tinify.from_file(imgpath).to_file(imgpath)
```

