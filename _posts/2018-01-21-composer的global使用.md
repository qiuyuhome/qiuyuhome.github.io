---
layout: post
title: composer的global使用
categories: Tools
description: composer的global使用
keywords: Linux, composer, php
---

# composer的global使用

## refer

[https://www.chrisyue.com/use-composer-to-install-php-libs-elegantly.html#comment-7525](https://www.chrisyue.com/use-composer-to-install-php-libs-elegantly.html#comment-7525)

## 操作

这里用`php_codesniffer`来举例说明.





``` bash
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

```
➜  bin pwd
/Users/qiuyu/.composer/vendor/bin
➜  bin ll
total 24
lrwxr-xr-x  1 qiuyu  staff    28B 11  4 19:41 laravel -> ../laravel/installer/laravel
lrwxr-xr-x  1 qiuyu  staff    39B  1 21 13:15 phpcbf -> ../squizlabs/php_codesniffer/bin/phpcbf
lrwxr-xr-x  1 qiuyu  staff    38B  1 21 13:15 phpcs -> ../squizlabs/php_codesniffer/bin/phpcs
➜  bin
```

编辑配置文件

```
vim ~/.bash_profile
```

加入以下命令

```
# Setting PATH for composer/bin
PATH="/Users/qiuyu/.composer/vendor/bin:${PATH}"
export PATH
```

现在已经加入了环境变量, 在任意目录执行命令. 都可以了.

```
➜  ~ phpcs --version
PHP_CodeSniffer version 3.2.2 (stable) by Squiz (http://www.squiz.net)
➜  ~
```

