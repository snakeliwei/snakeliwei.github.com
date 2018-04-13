title: "使用Travis-ci和纸小墨（InkPaper）部署GithubPage"
date: 2015-03-01 18:00:00 +0800
update: 2016-07-11 17:00:00 +0800
author: me
cover: "-/images/ink-Travis.png"
tags:
    - 设计
    - 写作
preview: 纸小墨（InkPaper）是一个GO语言编写的开源静态博客构建工具，可以快速搭建博客网站。它无依赖跨平台，配置简单构建快速，注重简洁易用与更优雅的排版。

---

## 纸小墨简介

纸小墨（InkPaper）是一个GO语言编写的开源静态博客构建工具，可以快速搭建博客网站。它无依赖跨平台，配置简单构建快速，注重简洁易用与更优雅的排版。

### 开始上手

- 下载并解压 [Ink](http://www.chole.io/)，运行命令 `ink preview`

  > 注意：Linux/macOS下，使用 `./ink preview`

- 使用浏览器访问 `http://localhost:8000` 预览。

### 特性介绍
- YAML格式的配置
- Markdown格式的文章
- 无依赖跨平台
- 超快的构建速度
- 不断改善的主题与排版
- 多文章作者支持
- 归档与标签自动生成
- 保存时实时预览页面
- 离线的全文关键字搜索

### 配置网站
编辑`config.yml`，使用如下格式：

``` yaml
site:
    title: 网站标题
    subtitle: 网站子标题
    limit: 每页可显示的文章数目
    theme: 网站主题目录
    comment: 评论插件变量(默认为Disqus账户名)
    root: 网站根路径 #可选
    lang: 网站语言 #支持en, zh, ru, ja，可在theme/config.yml配置
    url: 网站链接 #用于RSS生成
    link: 文章链接形式 #默认为{title}.html，支持{year},{month},{day},{title}变量

authors:
    作者ID:
        name: 作者名称
        intro: 作者简介
        avatar: 作者头像路径

build:
    output: 构建输出目录 #可选, 默认为 "public"
    port: 预览端口
    copy:
        - 构建时将会复制的目录/文件
    publish: |
        ink publish 命令将会执行的脚本
```

### 创建文章
在`source`目录中建立任意`.md`文件（可置于子文件夹），使用如下格式：

``` yaml
title: 文章标题
date: 年-月-日 时:分:秒 #创建时间，可加时区如" +0800"
update: 年-月-日 时:分:秒 #更新时间，可选，可加时区如" +0800"
author: 作者ID
cover: 题图链接 #可选
draft: false #草稿，可选
top: false #置顶文章，可选
preview: 文章预览，也可在正文中使用<!--more-->分割 #可选
tags: #可选
    - 标签1
    - 标签2
type: post #指定类型为文章(post)或页面(page)，可选
hide: false #隐藏文章，只可通过链接访问，可选

---

Markdown格式的正文
```

### 发布博客
- 在博客目录下运行`ink publish`命令自动构建博客并发布。
- 或运行`ink build`命令将生成的`public`目录下的内容手动部署。

> Tips: 在使用`ink preview`命令时，编辑保存文件后，博客会自动重新构建并刷新浏览器页面。

## 定制支持

### 修改主题

默认主题在`theme`目录下，修改源代码后在该目录下运行`npm install`与`npm run build`重新构建。

页面包含`page.html`（文章列表）及`article.html`（文章）等，所有页面均支持[GO语言HTML模板](http://golang.org/pkg/html/template/)语法，可引用变量。

### 添加页面

在`source`目录下创建的任意`.html`文件将被复制，这些文件中可引用`config.yml`中site字段下的所有变量。

### 博客迁移(Beta)

纸小墨提供简单的Jeklly/Hexo博客文章格式转换，使用命令：
``` shell
ink convert /path/_posts
```

### 源码编译

本地运行

1. 配置[GO](http://golang.org/doc/install)语言环境。
2. 运行命令`go get github.com/InkProject/ink`，编译并获取ink。
3. 运行命令`ink preview $GOPATH/src/github.com/InkProject/ink/template`，预览博客。

Docker构建（示例）

1. Clone源码 `git clone git@github.com:InkProject/ink.git`。
2. 源码目录下构建镜像`docker build -t ink .`。
3. 运行容器`docker run -p 8000:80 ink`。

## 主题

- Dark(Official Theme): [https://github.com/InkProject/ink-theme-dark](https://github.com/InkProject/ink-theme-dark)
- simple: [https://github.com/myiq/ink-simple](https://github.com/myiq/ink-simple)

## 使用Travis-ci来自动部署GithubPage

### 建立Github Page仓库

- 在Github上建立名为`yourname.github.com`的仓库

 > 用该名字建立的仓库可用用`yourname.github.io`域名直接访问生成的博客页面。

- 将纸小墨的源码push到非master分支上，我使用的是名为blog-src的分支。

- 在源码根目录新建`.travis.yml`文件，填入下列内容并push到github上：

``` yaml
language: go

install:
  - go get github.com/InkProject/ink
  - ink build

script:
  - cd ./blog/public/
  - git init
  - git config user.name "yourname"
  - git config user.email "yourname@gmail.com"
  - git add .
  - git commit -m "update blogs"
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master

branches:
  only:
    - blog-src
env:
  global:
    - GH_REF: github.com/yourname/yourname.github.com
```

### 在Travis-ci.org上进行相关设置

- 用你的 GitHub 账号登录 Travis CI。

- 登录之后，请转到您的配置页，并为你想要构建的仓库启用 Travis CI 。

- 在 Travis CI 里为对应的仓库添加 Github Access Token，用于后续使用 GitHub API. 这样 Travis CI 可以将通过 InkPaper 生成的静态博客源文件推送到 GitHub Pages 分支。