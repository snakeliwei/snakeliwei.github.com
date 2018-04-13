title: Postgre readonly user
tags:
  - Postgre
  - tips
categories:
  - Study
cc: true
hljs: true
comments: true
date: 2016-07-17 14:36:19
author: me
cover: "-/images/zsh.png"
---
>生产系统上通常不让随便启用可写权限，这里给出只读用户创建方法


### 创建一个用户名为readonly密码为ropass的用户
```
CREATE USER readonly WITH ENCRYPTED PASSWORD 'ropass';
```
### 用户只读事务
```
alter user readonly set default_transaction_read_only=on;
```
### 把所有库的语言的USAGE权限给到readonly
```
GRANT USAGE ON SCHEMA public to readonly;
```
### 授予select权限(这句要进入具体数据库操作在哪个db环境执行就授予那个db的权)
```
grant select on all tables in schema public to readonly;
```
