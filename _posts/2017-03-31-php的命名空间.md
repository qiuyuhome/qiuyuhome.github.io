---
layout: post
title: PHP的命名空间
categories: PHP
description: PHP的命名空间
keywords: PHP, namespace
---

php的命名空间在我以前的项目中没有使用过, 虽然也了解过, 所以今天特意弄了一下. 用起来还是挺方便的. 之前看TP的源码, 里面就是用的namespace, 所以了解一下还是很有必要的.

## 例子

2个文件, 在同目录下. 

`demo.php`

```php
<?php 
namespace foo;

include("./test.php");
use test\qiuyu\t;

$obj = new t();
$obj->index();
?>
```

---

`test.php`

```php
<?php
namespace test\qiuyu;

class t
{
    public function index()
    {
        echo "aaaaa\n";
    }
}
```

这样就是一个最简单的例子了. 

高级功能以后补上. 

