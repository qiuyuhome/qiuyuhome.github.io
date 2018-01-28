---
layout: post
title: php在cli下使用composer报错
categories: PHP
description: php在cli下使用composer报错
keywords: PHP, composer, error
---

在命令行下使用`php -v`, 显示的是7.1的版本.
但是使用`composer`安装要求7以上的包时, 却提示我的php版本是5.6, 不符合要求. 我就纳闷了. WHY.

## 查找原因

我要安装的`composer`包, 要求是`php`的版本大于`7.1.3`.

![composer_error_php_version](/images/posts/composer_error_php_version.png)

查看我的版本.

```zsh
➜  ~ php -v
PHP 7.1.8 (cli) (built: Aug 17 2017 11:34:56) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.1.0, Copyright (c) 1998-2017 Zend Technologies
    with Xdebug v2.5.0, Copyright (c) 2002-2016, by Derick Rethans
➜  ~
```

完全没问题, 我的php是7.1.8的版本.

但是. 在我安装的时候, 却提示我这个.

```zsh
➜  test_php_version composer require "symfony/var-dumper:4.0.3"
No composer.json in current directory, do you want to use the one at /Users/qiuyu/test? [Y,n]? n
./composer.json has been created
Loading composer repositories with package information
Updating dependencies (including require-dev)
Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - Installation request for symfony/var-dumper 4.0.3 -> satisfiable by symfony/var-dumper[v4.0.3].
    - symfony/var-dumper v4.0.3 requires php ^7.1.3 -> your PHP version (5.6.30) does not satisfy that requirement.


Installation failed, deleting ./composer.json.
➜  test_php_version
```

what happend?

我的版本是5.6? 容我想想. `mac`系统默认自带了php, 版本是5.6, 我使用的php是`MAMP PRO`的. 这个app如何指定的php环境变量呢? 我不知道. 去找找看.

```zsh
➜  ~ cat ~/.profile
alias php='/Applications/MAMP/bin/php/php7.1.8/bin/php -c "/Library/Application Support/appsolute/MAMP PRO/conf/php7.1.8.ini"'
alias pear='/Applications/MAMP/bin/php/php7.1.8/bin/pear'
alias pecl='/Applications/MAMP/bin/php/php7.1.8/bin/pecl'
➜  ~
```

找到原因了. 原来. `MAMP PRO`, 只是做了一个php的别名. 并没有设置环境变量. 但是. `composer`是在环境变量中查看的php版本, 所以就出现了这个问题.

so, 我得手动加入环境变量.

## 解决方案

查看系统自带的php环境变量的位置.

```zsh
➜  ~ whereis php
/usr/bin/php
➜  ~ /usr/bin/php -v
PHP 5.6.30 (cli) (built: Feb  7 2017 16:06:52)
Copyright (c) 1997-2016 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2016 Zend Technologies
➜  ~
```

可以看到, 系统自带的php是放在了`/usr/bin`下.

环境变量在`mac`中也是有顺序的.

```zsh
➜  ~ cat /etc/paths
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
➜  ~
```

这个顺序我可能调整过, 把`/usr/local/bin`放到了第一位. 所以, 我只需要把`php7.1.8`加入到`/usr/local/bin`中就可以了.

或者新加入一个环境变量.

```zsh
➜  ~ cat .profile
export PATH=/Applications/MAMP/bin/php/php7.1.8/bin:$PATH
alias php='/Applications/MAMP/bin/php/php7.1.8/bin/php -c "/Library/Application Support/appsolute/MAMP PRO/conf/php7.1.8.ini"'
alias pear='/Applications/MAMP/bin/php/php7.1.8/bin/pear'
alias pecl='/Applications/MAMP/bin/php/php7.1.8/bin/pecl'
➜  ~
```

加入后, 重新打开终端或者执行重新加载命令

```zsh
➜  ~ source ~/.profile
➜  ~
```

再次安装`symfony/var-dumper:4.0.3`试试.

```zsh
➜  test_php_version composer require "symfony/var-dumper:4.0.3"
No composer.json in current directory, do you want to use the one at /Users/qiuyu/test? [Y,n]? n
./composer.json has been created
Loading composer repositories with package information
Updating dependencies (including require-dev)
Package operations: 3 installs, 0 updates, 0 removals
  - Installing symfony/polyfill-php72 (v1.6.0): Downloading (100%)
  - Installing symfony/polyfill-mbstring (v1.6.0): Loading from cache
  - Installing symfony/var-dumper (v4.0.3): Downloading (100%)
Writing lock file
Generating autoload files
➜  test_php_version
```

ok, 解决了.




