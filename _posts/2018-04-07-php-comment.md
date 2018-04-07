---
layout: post
title: PHP 用英文写注释
categories: PHP
description: PHP 用英文写注释
keywords: PHP, Comment
---

在写 PHP 代码的时候, 总是苦恼英文注释应该如何写, 英语不好的后果就是这样, 所以, 这里记录了一些常用的英文注释的例子. 

## 参考

laravel 的源码注释

### 正文

#### 类中属性的注释, 这个属性标记了某个标记是否存在

```php
    /**
     * Indicates if the application has "booted".
     * 指示应用程序是否已启动。
     *
     * @var bool
     */
    protected $booted = false;
```

