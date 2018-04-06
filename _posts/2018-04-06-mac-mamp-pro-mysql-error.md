---
layout: post
title: mac 下 MAMP PRO MYSQL 不能启动
categories: [Linux]
description: mac 下 MAMP PRO MYSQL 不能启动
keywords: Linux, MAMP, MySQL
---

在使用 mac 下的 MAMP PRO 启动 MySQL 时, 启动不成功. 特此记录处理方法. 

MySQL 的日志为:

```log
180406 14:01:47 mysqld_safe Logging to '/Applications/MAMP/logs/mysql_error.log'.
180406 14:01:47 mysqld_safe Starting mysqld daemon with databases from /Library/Application Support/appsolute/MAMP PRO/db/mysql56
2018-04-06 14:01:47 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2018-04-06 14:01:47 0 [Note] --secure-file-priv is set to NULL. Operations related to importing and exporting data are disabled
2018-04-06 14:01:47 0 [Note] /Applications/MAMP/Library/bin/mysqld (mysqld 5.6.35) starting as process 98432 ...
2018-04-06 14:01:47 98432 [Warning] Setting lower_case_table_names=2 because file system for /Library/Application Support/appsolute/MAMP PRO/db/mysql56/ is case insensitive
2018-04-06 14:01:47 98432 [Note] Plugin 'FEDERATED' is disabled.
2018-04-06 14:01:47 98432 [Note] InnoDB: Using atomics to ref count buffer pool pages
2018-04-06 14:01:47 98432 [Note] InnoDB: The InnoDB memory heap is disabled
2018-04-06 14:01:47 98432 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
2018-04-06 14:01:47 98432 [Note] InnoDB: Memory barrier is not used
2018-04-06 14:01:47 98432 [Note] InnoDB: Compressed tables use zlib 1.2.8
2018-04-06 14:01:47 98432 [Note] InnoDB: Using CPU crc32 instructions
2018-04-06 14:01:47 98432 [Note] InnoDB: Initializing buffer pool, size = 128.0M
2018-04-06 14:01:47 98432 [Note] InnoDB: Completed initialization of buffer pool
2018-04-06 14:01:47 98432 [Note] InnoDB: Highest supported file format is Barracuda.
2018-04-06 14:01:47 98432 [Note] InnoDB: The log sequence numbers 1670707 and 1670707 in ibdata files do not match the log sequence number 1670717 in the ib_logfiles!
2018-04-06 14:01:47 98432 [Note] InnoDB: Database was not shutdown normally!
2018-04-06 14:01:47 98432 [Note] InnoDB: Starting crash recovery.
2018-04-06 14:01:47 98432 [Note] InnoDB: Reading tablespace information from the .ibd files...
2018-04-06 14:01:47 98432 [ERROR] InnoDB: Attempted to open a previously opened tablespace. Previous tablespace mysql/slave_master_info uses space ID: 4 at filepath: ./mysql/slave_master_info.ibd. Cannot open tablespace sample/migrations which uses space ID: 4 at filepath: ./sample/migrations.ibd
2018-04-06 14:01:47 7fffc263b3c0  InnoDB: Operating system error number 2 in a file operation.
InnoDB: The error means the system cannot find the path specified.
InnoDB: If you are installing InnoDB, remember that you must create
InnoDB: directories yourself, InnoDB does not create them.
InnoDB: Error: could not open single-table tablespace file ./sample/migrations.ibd
InnoDB: We do not continue the crash recovery, because the table may become
InnoDB: corrupt if we cannot apply the log records in the InnoDB log to it.
InnoDB: To fix the problem and start mysqld:
InnoDB: 1) If there is a permission problem in the file and mysqld cannot
InnoDB: open the file, you should modify the permissions.
InnoDB: 2) If the table is not needed, or you can restore it from a backup,
InnoDB: then you can remove the .ibd file, and InnoDB will do a normal
InnoDB: crash recovery and ignore that table.
InnoDB: 3) If the file system or the disk is broken, and you cannot remove
InnoDB: the .ibd file, you can set innodb_force_recovery > 0 in my.cnf
InnoDB: and force InnoDB to continue crash recovery here.
```

查找网上的资料. 删除这个3个文件即可. 


```shell
$ rm /Library/Application\ Support/appsolute/MAMP\ PRO/db/mysql56/ib_logfile0
$ rm /Library/Application\ Support/appsolute/MAMP\ PRO/db/mysql56/ib_logfile1
$ rm /Library/Application\ Support/appsolute/MAMP\ PRO/db/mysql56/ibdata1
```
