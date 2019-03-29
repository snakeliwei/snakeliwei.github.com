title: Mysql Error: could not open single-table tablespace file
date: 2019-03-29 11:13:51 +0800
update: 2019-03-29 12:00:00 +0800
author: me
cover: ""
tags:
  - Mysql
  - linux
preview: 一次mysql重启错误的解决
---

## Symptoms

Unable to start MySQL/mysqld service. The following errors are foundin /var/log/mysqld.log:

```bash
[ERROR] InnoDB: innodb-page-size mismatch in tablespace ./phpmyadmin_rq_U9tqAuU26/pma_central_columns.ibd (table phpmyadmin_rq_U9tqAuU26/pma_central_columns)...
InnoDB: Error: could not open single-table tablespace file ./phpmyadmin_rq_U9tqAuU26/pma_central_columns.ibd
```

## Cause

Crash recovery of InnoDB cannot be finished because the table is corrupted during the recovery process.

## Resolution

- Log in to the server via SSH.
- Move the file mentioned in the error message to the other directory:

```bash
# mv /var/lib/mysql/phpmyadmin_rq_U9tqAuU26/pma_central_columns.ibd ~/
```

- Start the mysqld service (InnoDB recovery process will be executed automatically):

```bash
# service mysqld start
```
