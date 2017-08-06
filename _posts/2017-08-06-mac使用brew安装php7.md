---
layout: post
title: mac使用brew安装php7
categories: mac使用brew安装php7
description: mac使用brew安装php7
keywords: PHP
---

mac自带的php, 版本太低, 安装扩展也比较麻烦. 开发机, 没必要重新编译安装扩展吧, 直接使用`brew`多好. 所以, 准备重新装`php7`.

**遇到的问题**. 
软链到`/usr/bin`下提示没有权限. 

**下面是我的安装步骤.** 

查找php7, 没有找到. 

```
brew search php7
```

google了一下, 使用以下命令. 我也不知道是什么意思. 

```
brew tap homebrew/dupes

brew tap homebrew/versions

brew tap homebrew/homebrew-php
```

然后在查找php7, 就能够找到了. 

直接安装

```
brew install php70
```

**重点来了**

在命令行中, 直接使用`php`, 还是原来的5.6版本. 

```
ln -sf /usr/local/Cellar/php70/7.0.21_13/bin/php /usr/bin/php
```

提示我没有权限. 

```
➜  bin ln -sf /usr/local/Cellar/php70/7.0.21_13/bin/php /usr/bin/php
ln: /usr/bin/php: Operation not permitted
➜  bin
```

你妹的. 输出环境变量看看

```
➜  bin echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
➜  bin
```

可以看到, `/usr/local/bin`的优先级比`/usr/bin`的高. 那我再试试软链到`/usr/local/bin`下. 

```
➜  bin ln -sf /usr/local/Cellar/php70/7.0.21_13/bin/php /usr/local/bin/php
➜  bin
```

ok, 没有问题. 

关闭当前终端, 重新打开. 

```
➜  ~ php -v
PHP 7.0.21 (cli) (built: Jul 29 2017 09:17:51) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2017 Zend Technologies
➜  ~
```

ok, 没有问题了. 
再安装扩展, 只需要简单的`brew install extend_name`就可以了.


