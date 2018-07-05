title: Get the latest git tag
date: 2018-07-05 16:13:51
author: me
cover: ""
tags:
  - git
  - linux
preview: 获取最新的git tag.

---

# 最近做 ci 用到需要获取最新的 git tag 信息，记录一下

```bash
git fetch --tags origin # 拉取远程tag信息
git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)` #获取最新的tag，关键点是中间的正则匹配需要根据项目来
```
