---
layout: post
title: 版本号的规范
categories: PHP
description: 版本号的规范
keywords: version, composer
---

## refer

今天闲着没事, 翻看技术资料. 看到了一篇文章, 很有意义.

原文地址: https://www.chrisyue.com/use-composer-to-install-php-libs-elegantly.html#comment-7525

主要的内容是版本号中各位数字代表的意义.

更全的资料, 可以在官网中了解.

官网地址: https://semver.org/

## 总结

语义化版本控制的规范是由 Gravatars 创办者兼 GitHub 共同创办者 Tom Preston-Werner 所建立

这个也可以说是一种规范了. 做出这个规范的作者还是很有权威的.

所以以后的版本号, 就完全要按照这个规范来处理了.

引用官方的介绍:

版本格式：主版本号.次版本号.修订号，版本号递增规则如下：

1. 主版本号：当你做了不兼容的 API 修改，
2. 次版本号：当你做了向下兼容的功能性新增，
3. 修订号：当你做了向下兼容的问题修正。

先行版本号及版本编译信息可以加到“主版本号.次版本号.修订号”的后面，作为延伸。

对应到我们接触最多的`composer`, 我们通过版本号, 就能看出`composer`中各个版本的大致差异了.

