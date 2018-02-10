---
layout: post
title: php中的匿名函数和闭包
categories: PHP
description: php中的匿名函数和闭包
keywords: PHP, Anonymous, Closure
---

`php` 中存在很多实用的技巧, 不会也不影响什么, 但是正确的使用这些技巧, 能够让你的开发更容易一些.
例如: 实际 `web` 项目中, 用到匿名的地方很少(框架内部的不算). 但是在写一些脚本或者开源项目的时候, 肯定会用到这些的.

## 参考

[PHP官方手册 - 匿名函数](http://php.net/manual/zh/functions.anonymous.php)

[PHP官方手册 - 闭包](http://php.net/manual/zh/class.closure.php)

[csdn - Closure类详解](http://blog.csdn.net/wuxing26jiayou/article/details/51067190)

这里吐槽一下, 官方手册的例子, 真让人恼火. 看一下.

## 匿名函数

```php
<?php
$message = 'hello';

// 没有 "use"
$example = function () {
    var_dump($message);
};
echo $example();

// 继承 $message
$example = function () use ($message) {
    var_dump($message);
};
echo $example();

// Inherited variable's value is from when the function
// is defined, not when called
$message = 'world';
echo $example();

// Reset message
$message = 'hello';

// Inherit by-reference
$example = function () use (&$message) {
    var_dump($message);
};
echo $example();

// The changed value in the parent scope
// is reflected inside the function call
$message = 'world';
echo $example();

// Closures can also accept regular arguments
$example = function ($arg) use ($message) {
    var_dump($arg . ' ' . $message);
};
$example("hello");
?>
```

* 槽点1: 注释部分. 要么你就别有中文手册, 要么你就全都翻译过来. 为什么有的注释是中文, 有的注释是英文.
* 槽点2: 匿名函数中输入了. 调用这个匿名函数的时候也输出. 有毛用? 这个匿名函数没有返回值的啊. 靠.

吐槽归吐槽, 这个例子的作用还是很好的. 能够直观的看出匿名函数的特点. 官网给的例子不直观. 我实际测试的时候是拆分开的. 如下.

### 测试匿名函数中变量的作用域

```php
<?php
$message = 'hello';

// 没有 "use"
$example = function () {
    var_dump($message);
};
$example();
```

输出:

![imageaaa_callback.jpg_1](/images/posts/imageaaa_callback.jpg_1.png)

> 解释: 匿名函数和普通的函数一样, 直接使用外部的变量是不允许的. 所以会警告, 下面这个是正确的使用方法.

```php
<?php
$message = 'hello';

// 继承 $message
$example = function () use ($message) {
    var_dump($message);
};
$example();
```

> 解释: 闭包可以从父作用域中继承变量。 任何此类变量都应该用 use 语言结构传递进去。 PHP 7.1 起，不能传入此类变量： superglobals、 $this 或者和参数重名。

### 测试闭包的继承外部变量值

```php
<?php
$message = 'hello';

// 继承 $message
$example = function () use ($message) {
    var_dump($message);
};
$example();

// Inherited variable's value is from when the function
// is defined, not when called
$message = 'world';
$example();
```

输出:

![imageaaa_callback3](/images/posts/imageaaa_callback3.png)

> 解释: Inherited variable's value is from when the function is defined, not when called.
> 使用外部变量的值, 要使用 use , 而且, 继承这个变量的值是在匿名函数定义时确定的, 而不是在调用的时候确定的.
> 在匿名函数定义之后, 调用这个匿名函数之前, 改变匿名函数继承的外部变量的值, 匿名函数中继承的变量值是不变的.

### 匿名函数中的外部参数, 使用地址引用

```php
<?php
$message = 'hello';

// Inherit by-reference
$example = function () use (&$message) {
    var_dump($message);
};
$example();
```

结果:

![imageaaa_callback_a_1](/images/posts/imageaaa_callback_a_1.png)

> 可以看到. 引用外部的变量参数中, 前面有 &

作用, 看代码.

```php
<?php
$message = 'hello';

// Inherit by-reference
$example = function () use (&$message) {
    var_dump($message);
};

// The changed value in the parent scope
// is reflected inside the function call
$message = 'world';
$example();
```

![imageaaa_callback_a_2](/images/posts/imageaaa_callback_a_2.png)

> 可以看到, 和上一个例子的区别在于, 使用了 & 之后, 是可以在匿名函数定义只有改变变量引用的.

### 类似于js的使用方式.

```php
<?php

$a = 'foo'; $b = 'bar';
$test = (function() use($a,$b) { return $a . $b; })();
echo $test;
```

> 输出: foobar

## 闭包

定义一个闭包函数，其实是产生了一个闭包类（Closure）的对象.

先看一下源码定义:

```php
/**
 * Class used to represent anonymous functions.
 * <p>Anonymous functions, implemented in PHP 5.3, yield objects of this type.
 * This fact used to be considered an implementation detail, but it can now be relied upon.
 * Starting with PHP 5.4, this class has methods that allow further control of the anonymous function after it has been created.
 * <p>Besides the methods listed here, this class also has an __invoke method.
 * This is for consistency with other classes that implement calling magic, as this method is not used for calling the function.
 * @link http://www.php.net/manual/en/class.closure.php
 */
final class Closure {

    /**
     * This method exists only to disallow instantiation of the Closure class.
     * Objects of this class are created in the fashion described on the anonymous functions page.
     * @link http://www.php.net/manual/en/closure.construct.php
     */
    private function __construct() { }

    /**
     * This is for consistency with other classes that implement calling magic,
     * as this method is not used for calling the function.
     * @param mixed $_ [optional]
     * @return mixed
     * @link http://www.php.net/manual/en/class.closure.php
     */
    public function __invoke(...$_) { }

    /**
     * Duplicates the closure with a new bound object and class scope
     * @link http://www.php.net/manual/en/closure.bindto.php
     * @param object $newthis The object to which the given anonymous function should be bound, or NULL for the closure to be unbound.
     * @param mixed $newscope The class scope to which associate the closure is to be associated, or 'static' to keep the current one.
     * If an object is given, the type of the object will be used instead.
     * This determines the visibility of protected and private methods of the bound object.
     * @return Closure Returns the newly created Closure object or FALSE on failure
     */
    function bindTo($newthis, $newscope = 'static') { }

    /**
     * This method is a static version of Closure::bindTo().
     * See the documentation of that method for more information.
     * @static
     * @link http://www.php.net/manual/en/closure.bind.php
     * @param Closure $closure The anonymous functions to bind.
     * @param object $newthis The object to which the given anonymous function should be bound, or NULL for the closure to be unbound.
     * @param mixed $newscope The class scope to which associate the closure is to be associated, or 'static' to keep the current one.
     * If an object is given, the type of the object will be used instead.
     * This determines the visibility of protected and private methods of the bound object.
     * @return Closure Returns the newly created Closure object or FALSE on failure
     */
    static function bind(Closure $closure, $newthis, $newscope = 'static') { }

    /**
     * Temporarily binds the closure to newthis, and calls it with any given parameters.
     * @link http://php.net/manual/en/closure.call.php
     * @param object $newThis The object to bind the closure to for the duration of the call.
     * @param mixed $parameters [optional] Zero or more parameters, which will be given as parameters to the closure.
     * @return mixed
     * @since 7.0
     */
    function call ($newThis, ...$parameters) {}

    /**
     * @param callable $callable
     * @return Closure
     * @since 7.1
     */
    public static function fromCallable (callable $callable) {}
}
```

### Closure::__construct

可以看到. `__construct` 是私有方法, 所以, 不能在外部调用. 这个方法仅用于禁止实例化一个 `Closure` 类的对象.

### Closure::bind

是一个静态方法. 复制一个闭包，绑定指定的 `$this` 对象和类作用域.

#### 参数
closure：表示需要绑定的闭包对象。

newthis：表示需要绑定到闭包对象的对象，或者NULL创建未绑定的闭包。

newscope：表示想要绑定给闭包的类作用域，可以传入类名或类的实例，默认值是 'static'， 表示不改变。

#### 返回值

该方法成功时返回一个新的 Closure 对象，失败时返回FALSE。

### 实例

```php
class Animal 
{  
    private static $cat = "cat";  
    private $dog = "dog";  
    public $pig = "pig";  
}  

/*  
 * 获取Animal类静态私有成员属性 
 */  
$cat = static function() {  
    return Animal::$cat;  
};  

/*  
 * 获取Animal实例私有成员属性 
 */  
$dog = function() {  
    return $this->dog;  
};  

/*  
 * 获取Animal实例公有成员属性 
 */  
$pig = function() {  
    return $this->pig;  
};  

// 给闭包绑定了Animal实例的作用域，但未给闭包绑定$this对象
$bindCat = Closure::bind($cat, null, new Animal());

// 给闭包绑定了Animal类的作用域，同时将Animal实例对象作为$this对象绑定给闭包
$bindDog = Closure::bind($dog, new Animal(), 'Animal');

// 将Animal实例对象作为$this对象绑定给闭包,保留闭包原有作用域
$bindPig = Closure::bind($pig, new Animal());

// 根据绑定规则，允许闭包通过作用域限定操作符获取Animal类静态私有成员属性
echo $bindCat();

// 根据绑定规则，允许闭包通过绑定的$this对象(Animal实例对象)获取Animal实例私有成员属性
echo $bindDog();

// 根据绑定规则，允许闭包通过绑定的$this对象获取Animal实例公有成员属性
echo $bindPig();

// bindTo与bind类似，是面向对象的调用方式，这里只举一个，其他类比就可以
$bindCat = $cat->bindTo(null, 'Animal');
```
