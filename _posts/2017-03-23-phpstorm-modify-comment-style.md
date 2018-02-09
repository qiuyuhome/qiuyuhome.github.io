---
layout: post
title: phpstorm for Mac 修改注释的风格
categories: Tools
description: phpstorm for Mac 修改注释的风格
keywords: phpstorm, Mac, tools
---


phpstorm默认的注释, 会在行首加上注释符. 非常的不习惯. 我习惯是在代码前面加上`//`再加一个空格. 修改如下达到效果. 

![-w600](/images/posts/14902336357005.jpg)



效果如下:

```php
$elements = $driver->findElements(WebDriverBy::cssSelector("#resultsCol h2"));

foreach ($elements as $v) {
   // echo $v->getText()."\n";

}
```




