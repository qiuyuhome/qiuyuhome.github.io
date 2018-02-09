---
layout: post
title: 命令行中的输出替换字符
categories: Linux
description: 命令行中的输出替换字符
keywords: PHP, Linux, 命令行中的输出替换字符
---

只需要输出\r\033[2K字符，就能实现清除当前行并光标回到行首

```php
 <?php
 for ($i = 0; $i < 100; $i++) {
 		echo $i;
 		sleep(1);
 		echo "\r\033[2K";
 }
```



