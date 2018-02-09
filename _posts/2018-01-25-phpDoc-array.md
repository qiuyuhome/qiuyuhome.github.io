---
layout: post
title: phpDoc描述数组
categories: PHP
description: phpDoc描述数组
keywords: PHP, phpDoc, Array
---

[psr-5 github](https://github.com/phpDocumentor/fig-standards/blob/master/proposed/phpdoc.md)

[查看 wiki](https://zh.wikipedia.org/wiki/PHPDoc)

```php
/**
 * Initializes this class with the given options.
 *
 * @param array $options {
 *     A description of this array.
 *
 *     @type boolean $required Whether this element is required
 *     @type string  $label    The display name for this element
 * }
 */
public function __construct(array $options = array())
{
    <...>
}
```

```php
/**
 * 批量获取用户基本信息
 * @desc 用于获取多个用户基本信息
 * @return int    code 操作码，0表示成功
 * @return array  list 用户列表
 * @return int    list[].id 用户ID
 * @return string list[].name 用户名字
 * @return string list[].note 用户来源
 * @return string msg 提示信息
 */
public function getMultiBaseInfo()
{
    return [];
}
```


