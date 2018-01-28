---
layout: post
title: mac下cli中php命令加载慢的问题
categories: PHP
description: mac下cli中php命令加载慢的问题
keywords: PHP, Mac, Cli
---

mac自带的php我必须要吐槽一下. 版本低就不说什么了, 是5.6的版本. 删不掉, 权限不够. 不能升级是搞什么.
`/usr/bin`目录是root权限. 不重启使用特殊方法. 是搞不了这个目录的. 哎. 所以用`MAMP PRO`中的php. 问题来了.
执行一个`php -v`, 居然需要几秒钟. 难道是我的打开方式不对?

## mac下切换php版本.

我使用的是`MAMP PRO`, 这个app很棒. 常用的功能都有了. 点个赞.

在命令行中使用指定版本. 需要这么处理:

![composer_commit__mamp](/images/posts/composer_commit__mamp.png)

这样就可以在cli中使用这里的php版本. 切换php版本也很方便.

## cli中运行php命令特别慢

```zsh
➜  test git:(master) ✗ time /usr/bin/php -v
PHP 5.6.30 (cli) (built: Feb  7 2017 16:06:52)
Copyright (c) 1997-2016 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2016 Zend Technologies
/usr/bin/php -v  0.06s user 0.22s system 87% cpu 0.320 total
➜  test git:(master) ✗ time php -v
PHP 7.1.8 (cli) (built: Aug 17 2017 11:34:56) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.1.0, Copyright (c) 1998-2017 Zend Technologies
    with Xdebug v2.5.0, Copyright (c) 2002-2016, by Derick Rethans
/Applications/MAMP/bin/php/php7.1.8/bin/php -c  -v  0.04s user 0.04s system 1% cpu 5.161 total
```
可以看到. 用7.1版本的php, 运行一个`php -v`, 居然用了5秒多. 什么原因呢? 我是没想到. 只能`google`了.

## 参考

[Version 5.5.6 CLI very slow](https://github.com/liip/php-osx/issues/102)

[MAMP Pro 里面自带的 PHP 命令行执行特别慢的问题](https://sexywp.com/mamp-pro-php-cli-very-slow.htm)

## 解决方法

在hosts中. 加入本机的名称. 就可以了.

1. 在系统的`偏好设置`中, 找到`共享`:

    ![composer_commit__mamp_1](/images/posts/composer_commit__mamp_1.png)

2. 编辑`hosts`.
    ![composer_commit__mamp_2](/images/posts/composer_commit__mamp_2.png)

ok, 这样就可以了.

