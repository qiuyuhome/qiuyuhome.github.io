---
layout: post
title: mac下MAMP在终端使用mysql
categories: Linux
description: mac下MAMP在终端使用mysql
keywords: MySql
---

MAMP中已经集成了phpMyAdmin，可以很方便的管理mysql数据库，但是有的情况是phpMyAdmin不能做到的。比如，导入sql文件，当sql文件非常大（大于20MB）的时候，apache通常会执行超时，导致最终导入不完全，而且费时很长。如果此时使用shell导入的话，仅仅需要10秒即可轻松导入20MB的SQL文件。

打开终端（Terminal），输入

```shell
/Applications/MAMP/Library/bin/mysql -uroot -p
```

提示输入密码，输入完成后回车。之后操作和Windows的DOS下一样

注意：MAC系统输入MySQL密码的时候是不显示的，你要确认输出正确，在敲回车


