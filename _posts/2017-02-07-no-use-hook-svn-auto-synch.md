---
layout: post
title: 不用钩子,svn自动同步的脚本
categories: Tools
description: 不用钩子,svn自动同步的脚本
keywords: svn
---

看公司的svn自动同步脚本, 学习一下. 用的不是svn自带的钩子.

一般自启动的脚本, 都写在 `/etc/rc.d/rc.local`
例如: zl测试服务器中

```bash
#!/bin/sh
#
# This script will be executed *after* all the other init scripts.
# You can put your own initialization stuff in here if you don't
# want to do the full Sys V style init stuff.

touch /var/lock/subsys/local
/etc/init.d/redis start
/etc/init.d/mysqld start
/usr/local/bin/memcached -d -m 256 -u root -p 11211 -l 192.168.1.186 -P /tmp/memcached.pid
/etc/init.d/php-fpm start
/etc/init.d/nginx start
svnserve -d -r /data/svn
nohup /data/www/svnupauto.sh&
/usr/local/webserver/php5.4/sbin/php-fpm -c /usr/local/webserver/php5.4/etc/php.ini -y /usr/local/webserver/php5.4/etc/php-fpm.conf
```

其中有一个自动同步svn的脚本. 和启动svn服务的命令

看看自动同步svn脚本的内容

```bash
#!/bin/sh
while [ 1 -eq 1 ]
do
        /usr/bin/svn up /data/www/zhulong/
        sleep 4
done
```

是个死循环. 且间隔4秒. 

* 那么, `nohup /data/www/svnupauto.sh&`这个命令中, `nohup`是什么作用?
nohup 命令

**用途：**不挂断地运行命令。

**语法**：nohup Command [ Arg … ] [　& ]

**描述：**nohup 命令运行由 Command 参数和任何相关的 Arg 参数指定的命令，忽略所有挂断（SIGHUP）信号。在注销后使用 nohup 命令运行后台中的程序。要运行后台中的 nohup 命令，添加 & （ 表示”and”的符号）到命令的尾部。

无论是否将 nohup 命令的输出重定向到终端，输出都将附加到当前目录的 nohup.out 文件中。如果当前目录的 nohup.out 文件不可写，输出重定向到 $HOME/nohup.out 文件中。如果没有文件能创建或打开以用于追加，那么 Command 参数指定的命令不可调用。如果标准错误是一个终端，那么把指定的命令写给标准错误的所有输出作为标准输出重定向到相同的文件描述符。







