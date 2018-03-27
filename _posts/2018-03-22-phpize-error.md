---
layout: post
title: phpize 报错
categories: PHP
description: phpize 报错
keywords: PHP, phpize
---

今天在 VPS 上安装 fileinfo 的扩展, 却提示错误.


```shell
[root@vultr]~/lnmp1.3-full/src/php-7.1.13/ext/fileinfo# make
make
/bin/sh /root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo/libtool --mode=compile cc -I/root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo/libmagic -I. -I/root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo -DPHP_ATOM_INC -I/root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo/include -I/root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo/main -I/root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo -I/usr/local/php/include/php -I/usr/local/php/include/php/main -I/usr/local/php/include/php/TSRM -I/usr/local/php/include/php/Zend -I/usr/local/php/include/php/ext -I/usr/local/php/include/php/ext/date/lib  -DHAVE_CONFIG_H  -g -O2   -c /root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo/libmagic/apprentice.c -o libmagic/apprentice.lo
 cc -I/root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo/libmagic -I. -I/root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo -DPHP_ATOM_INC -I/root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo/include -I/root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo/main -I/root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo -I/usr/local/php/include/php -I/usr/local/php/include/php/main -I/usr/local/php/include/php/TSRM -I/usr/local/php/include/php/Zend -I/usr/local/php/include/php/ext -I/usr/local/php/include/php/ext/date/lib -DHAVE_CONFIG_H -g -O2 -c /root/lnmp1.3-full/src/php-7.1.13/ext/fileinfo/libmagic/apprentice.c  -fPIC -DPIC -o libmagic/.libs/apprentice.o
cc: 编译器内部错误：已杀死(程序 cc1)
Please submit a full bug report,
with preprocessed source if appropriate.
See <http://bugzilla.redhat.com/bugzilla> for instructions.
make: *** [libmagic/apprentice.lo] 错误 1
[root@vultr]~/lnmp1.3-full/src/php-7.1.13/ext/fileinfo#
```

原来是因为内存不足.

关掉一些进程. 内存就够了.