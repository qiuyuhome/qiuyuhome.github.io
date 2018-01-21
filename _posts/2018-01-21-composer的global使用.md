---
layout: post
title: composer的global使用
categories: tool
description: composer的global使用
keywords: Linux, composer, php
---

# composer的global使用

## refer

[https://www.chrisyue.com/use-composer-to-install-php-libs-elegantly.html#comment-7525](https://www.chrisyue.com/use-composer-to-install-php-libs-elegantly.html#comment-7525)

## 操作

这里用`php_codesniffer`来举例说明.

```language-php
➜  ~ composer global require "squizlabs/php_codesniffer=*"
Changed current directory to /Users/qiuyu/.composer
./composer.json has been updated
Loading composer repositories with package information
Updating dependencies (including require-dev)
Package operations: 1 install, 0 updates, 0 removals
  - Installing squizlabs/php_codesniffer (3.2.2): Downloading (100%)
Writing lock file
Generating autoload files
➜  ~
```

