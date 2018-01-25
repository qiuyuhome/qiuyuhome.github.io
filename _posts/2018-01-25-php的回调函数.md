---
layout: post
title: php的回调函数
categories: PHP
description: php的回调函数
keywords: PHP, callback
---


```php
$foo = function($name){
    return $name.':'.date("Y-m-d H:i:s", time());
}
```



