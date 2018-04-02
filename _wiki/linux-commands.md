---
layout: post
title: Linux常用的命令
categories: [Linux]
description: Linux常用的命令
keywords: Linux
---

主要是记录一些简单的命令, 但是又不是很常用的.

### which
查看可执行文件的位置.

which指令会在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果

```shell
➜  bin which -a php
php: aliased to /Applications/MAMP/bin/php/php7.1.8/bin/php -c "/Library/Application Support/appsolute/MAMP PRO/conf/php7.1.8.ini"
/Applications/MAMP/bin/php/php7.1.8/bin/php
/usr/bin/php
➜  bin
```

### tee

tee命令用于将数据重定向到文件，另一方面还可以提供一份重定向数据的副本作为后续命令的stdin。简单的说就是把数据重定向到给定文件和屏幕上.

```shell
➜  test ll | tee tee.log
total 0
-rw-r--r--  1 qiuyu  staff     0B  4  2 21:46 tee.log
➜  test tail tee.log
total 0
-rw-r--r--  1 qiuyu  staff     0B  4  2 21:46 tee.log
➜  test
```

