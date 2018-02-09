---
layout: post
title: 初探phpDoc
categories: PHP
description: 初探phpDoc
keywords: PHP, phpDoc, phpDocumentor
---

去年新开发的一个项目, 因为是一边摸索一般开发, 所以需求变换的也很快, 同时也是懒了, 就没有做接口文档.
刚开始还没有什么问题, 随着时间的慢慢推移, 业务逻辑也越来越多, 定制的一些规则也都记不住了, 所以. 做一个接口的文档. 还是很有必要的.
现在的开源的api文档管理系统非常多. 最终, 我选择了`phpDocumentor`, 为什么呢? 只因为他最符合PHP.

## `phpDocumentor` 是什么

是一个根据源代码中的注释, 自动生成文档的工具. 没有别的作用. 但是就这个一个作用. 就已经非常棒了.

还有其他的如 `swagger`, 也可以根据注释自动生成文档. 但是. `swagger` 要求的注释, 不符合 `psr-5` 的规范. 果断放弃.

## `phpDocumentor` 怎么用

可以下载`phar`直接使用.

也可以`pear`安装

同时也可以用`composer`安装.

我这里就用`composer`全局安装.

在`cli`下, 使用命令:

```zsh
➜  php composer global require "phpdocumentor/phpdocumentor"
Changed current directory to /Users/qiuyu/.composer
Using version ^2.9 for phpdocumentor/phpdocumentor
./composer.json has been updated
Loading composer repositories with package information
Updating dependencies (including require-dev)
Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - Installation request for phpdocumentor/phpdocumentor ^2.9 -> satisfiable by phpdocumentor/phpdocumentor[v2.9.0].
    - Conclusion: remove nikic/php-parser v3.1.4
    - Conclusion: don't install nikic/php-parser v3.1.4
    - phpdocumentor/phpdocumentor v2.9.0 requires phpdocumentor/reflection ^3.0 -> satisfiable by phpdocumentor/reflection[3.0.0, 3.0.1].
    - phpdocumentor/reflection 3.0.0 requires nikic/php-parser ^1.0 -> satisfiable by nikic/php-parser[v1.0.0, v1.0.1, v1.0.2, v1.1.0, v1.2.0, v1.2.1, v1.2.2, v1.3.0, v1.4.0, v1.4.1].
    - phpdocumentor/reflection 3.0.1 requires nikic/php-parser ^1.0 -> satisfiable by nikic/php-parser[v1.0.0, v1.0.1, v1.0.2, v1.1.0, v1.2.0, v1.2.1, v1.2.2, v1.3.0, v1.4.0, v1.4.1].
    - Can only install one of: nikic/php-parser[v1.0.0, v3.1.4].
    - Can only install one of: nikic/php-parser[v1.0.1, v3.1.4].
    - Can only install one of: nikic/php-parser[v1.0.2, v3.1.4].
    - Can only install one of: nikic/php-parser[v1.1.0, v3.1.4].
    - Can only install one of: nikic/php-parser[v1.2.0, v3.1.4].
    - Can only install one of: nikic/php-parser[v1.2.1, v3.1.4].
    - Can only install one of: nikic/php-parser[v1.2.2, v3.1.4].
    - Can only install one of: nikic/php-parser[v1.3.0, v3.1.4].
    - Can only install one of: nikic/php-parser[v1.4.0, v3.1.4].
    - Can only install one of: nikic/php-parser[v1.4.1, v3.1.4].
    - Installation request for nikic/php-parser (locked at v3.1.4) -> satisfiable by nikic/php-parser[v3.1.4].


Installation failed, reverting ./composer.json to its original content.
➜  php
```

好吧. 报错了. 去查一下原因. 

[Composer installation fails with Symfony 3.2.*](https://github.com/phpDocumentor/phpDocumentor2/issues/1818)

这里面提到了. 作者不会继续维护使用`composr`的安装方式. 推荐使用`phar`. 原因是依赖不好处理.

那么我们只能使用`phar`了. 去下载一个.

[http://phpdoc.org/phpDocumentor.phar](http://phpdoc.org/phpDocumentor.phar)

算了. 还是用`pear`安装吧. 全局使用方便.

```zsh
➜  ~  pear install phpdoc/phpDocumentor
Attempting to discover channel "phpdoc"...
Attempting fallback to https instead of http on channel "phpdoc"...
unknown channel "phpdoc" in "phpdoc/phpDocumentor"
invalid package name/package file "phpdoc/phpDocumentor"
install failed
➜  ~ pear channel-discover pear.phpdoc.org
Discovering channel pear.phpdoc.org over http:// failed with message: channel-add: Cannot open "http://pear.phpdoc.org/channel.xml" (Connection to `pear.phpdoc.org:80' failed: php_network_getaddresses: getaddrinfo failed: nodename nor servname provided, or not known)
Trying to discover channel pear.phpdoc.org over https:// instead
Adding Channel "pear.phpdoc.org" succeeded
Discovery of channel "pear.phpdoc.org" succeeded
➜  ~ pear install phpdoc/phpDocumentor
downloading phpDocumentor-2.9.0.tgz ...
Starting to download phpDocumentor-2.9.0.tgz (16,175,113 bytes)
..........done: 16,175,113 bytes
install ok: channel://pear.phpdoc.org/phpDocumentor-2.9.0
➜  ~
```

```zsh
➜  ~ phpdoc --version
phpDocumentor version 2.9.0
➜  ~
```

ok, 装完了.



