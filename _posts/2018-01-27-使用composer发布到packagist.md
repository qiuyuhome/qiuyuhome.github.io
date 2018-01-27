---
layout: post
title: 使用composer发布到packagist
categories: PHP
description: 使用composer发布到packagist
keywords: PHP, composer, packagist
---

一直在使用 `composer` 的基础功能, 平时没事的时候也会去 `packagist.org` 上面去看看优秀的 `package`, 总是想自己也发布点东西上去.
那么, 首先得学会如何发布. 目前为止, 我也不知道. 边学边记录.

## 参考

[Composer 中文网](http://www.phpcomposer.com/)

[Packagist](https://packagist.org/)

[codecasts 视频](https://www.codecasts.com/series/you-must-use-composer/episodes/9)

## 操作记录

### 编写 `composer.json` 文件

```json
{
    "name": "qiuyuhome/test-package",
    "description": "This is a demo for packagist.org, oh",
    "version": "1.0.0",
    "type": "library",
    "keywords": ["demo"],
    "homepage": "http://www.qiuyuhome.com",
    "time": "2018-01-27",
    "license": "MIT",
    "authors":[
        {
            "name": "QiuYu",
            "homepage": "http://www.qiuyuhome.com",
            "role": "Developer"
        }
    ],
    "autoload": {
        "psr-4": {"Qiuyuhome\\": "src"}
    },
    "require": {
        "php": "^5.6.0",
        "symfony/var-dumper": "~3.0.0"
    }
}
```

### 对应的目录

![composer_commit_232](/images/posts/composer_commit_232.png)

### 代码文件

`demo.php` 文件的代码:

```php
<?php
/**
 * Created by PhpStorm.
 * User: qiuyu
 * Date: 2018/1/27
 * Time: 下午6:32
 */

namespace Qiuyuhome;

class Demo
{
    public function index()
    {
        dump('This is a demo for packagist.org');
        return true;
    }
}
```

### 上传到github

### 发布到packagist

![composer_commit_32323](/images/posts/composer_commit_32323.png)

## 遇到的坑

1. 自动同步问题.
2. 版本号问题.

![composer_commit_1](/images/posts/composer_commit_1.png)


## 安装一下试试.

![composer_commit](/images/posts/composer_commit.png)

没问题. ok了

> create_time: 2018-01-27 15:55:58
