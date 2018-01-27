---
layout: post
title: PHP异常机制的使用场景
categories: PHP
description: PHP异常机制的使用场景
keywords: PHP, 异常, Exception
---

在写代码的过程中, 最痛苦的就是, 每次调用一个方法, 都需要判断返回值. 严重的影响了代码的可读性和布局.
在阅读开源项目的源码中, 总是看到很多地方都使用异常机制. 在我的实际项目中, 很少用到. 这个是病, 得治.

## 参考

在写这篇文章之前, 我在网上找了一篇文章. 虽然是12年作者写的. 但是还是很有权威性的. 讲解的也很好. 就是大名鼎鼎的 `鸡哥`, 啊, 不对, 是 `鸟哥`.

[ 我们什么时候应该使用异常?](http://www.laruence.com/2012/02/02/2515.html)

还有一些大侠的解释.

[PHP Exception 异常处理和 exit/die](https://segmentfault.com/q/1010000002912389)

[使用异常的注意事项](https://www.zhihu.com/question/29459586)

看了这些文章. 我感觉我找到了我想要的, 找到了方向. 当然. 理解了使用场景. 还是远远不够的. 基础还是得补补的.


## 理解

困了. 明天继续.

醒了, 继续.

我的理解:

1. 如果不使用异常机制, 那么, 每次调用一个方法, 都需要判断返回值. 这个是我现在每天都需要面对的问题. 最后代码很可能是下面这个样子

```php
...


/**
 * This is a test function
 * @param $params
 * @return mixed
 * @throws Exception
 */
public function test_one($params)
{
    if (!$params) {
        return ['code' => 101, 'data' => 'params can not null'];
    }
    return ['code' => 200, 'data' => $params];
} 

...

$res = $this->test_one($params);
if ($res['code'] !== 200) {
    // do something for error.
} else {
    // go on.
}

$res = $this->test_two();
if ($res['code'] !== 200) {
    // do something for error.
} else {
    // go on.
}

$res = $this->test_three();
if ($res['code'] !== 200) {
    // do something for error.
} else {
    // go on.
}

... 
```

可以看到, 每次调用一个方法, 都需要判断返回值. 如果使用异常机制, 则可以这么写.

```php
...

/**
 * This is a test function
 * @param $params
 * @return mixed
 * @throws Exception
 */
public function test_one($params)
{
    if (!$params) {
        throw new Exception('params can not null');
    }
    return $params;
} 

...

try {
    $resOne = $this->test_one($params);
    // do something for $resOne
    $resTwo = $this->test_two();
    // do something for $resTwo
    $resThree = $this->test_three();
    // do something for $resThree
} catch (\Exception $e) {
    // do something for error. 
    
}

// 这样, 就可以统一的捕获异常了. 避免了每次调用都需要处理返回值的状态码. 而是统一处理. 

...
````

自己写了一下试试.

```php
<?php
/**
 * 学习测试php的异常机制
 * User: qiuyu
 * Date: 2018/1/27
 * Time: 上午11:58
 */

include dirname(__FILE__).'/vendor/autoload.php';

/**
 * 这个可以是最顶层. 捕获所有的异常
 */
try {
    logic();
} catch (Exception $e) {
    // 记录到日志等操作.
    // log::write('something');
    dump($e);
}

/**
 * 逻辑层
 * @return mixed|string
 * @throws Exception
 */
function logic()
{
    try {
        $params = '';
        $resOne = foo($params);
        return $resOne;
    } catch (Exception $e) {
        // do something for error.
        switch ($e->getCode()) {
            case 100:
                dump('error: ' . $e->getMessage());
                break;
            default:
                throw $e;
                break;
        }
    }
    return 'ok';
}


/**
 * 逻辑层需要调用的一个小方法, 在这里可能会抛出异常.
 * @param $params
 * @return mixed
 * @throws Exception
 */
function foo($params)
{
    if (!$params) {
        throw new Exception('params can not null', 1001);
    }
    return $params;
}
```

todo 理解的还是不够, 所以此文还未完结.

## 最后附上php的异常base class

```php

/**
 * Throwable is the base interface for any object that can be thrown via a throw statement in PHP 7,
 * including Error and Exception.
 * @link http://php.net/manual/en/class.throwable.php
 * @since 7.0
 */
interface Throwable
{

    /**
     * Gets the message
     * @link http://php.net/manual/en/throwable.getmessage.php
     * @return string
     * @since 7.0
     */
    public function getMessage();

    /**
     * Gets the exception code
     * @link http://php.net/manual/en/throwable.getcode.php
     * @return int <p>
     * Returns the exception code as integer in
     * {@see Exception} but possibly as other type in
     * {@see Exception} descendants (for example as
     * string in {@see PDOException}).
     * </p>
     * @since 7.0
     */
    public function getCode();

    /**
     * Gets the file in which the exception occurred
     * @link http://php.net/manual/en/throwable.getfile.php
     * @return string Returns the name of the file from which the object was thrown.
     * @since 7.0
     */
    public function getFile();

    /**
     * Gets the line on which the object was instantiated
     * @link http://php.net/manual/en/throwable.getline.php
     * @return int Returns the line number where the thrown object was instantiated.
     * @since 7.0
     */
    public function getLine();

    /**
     * Gets the stack trace
     * @link http://php.net/manual/en/throwable.gettrace.php
     * @return array <p>
     * Returns the stack trace as an array in the same format as
     * {@see debug_backtrace()}.
     * </p>
     * @since 7.0
     */
    public function getTrace();

    /**
     * Gets the stack trace as a string
     * @link http://php.net/manual/en/throwable.gettraceasstring.php
     * @return string Returns the stack trace as a string.
     * @since 7.0
     */
    public function getTraceAsString();

    /**
     * Returns the previous Throwable
     * @link http://php.net/manual/en/throwable.getprevious.php
     * @return Throwable Returns the previous {@see Throwable} if available, or <b>NULL</b> otherwise.
     * @since 7.0
     */
    public function getPrevious();

    /**
     * Gets a string representation of the thrown object
     * @link http://php.net/manual/en/throwable.tostring.php
     * @return string <p>Returns the string representation of the thrown object.</p>
     * @since 7.0
     */
    public function __toString();
}
/**
 * Exception is the base class for
 * all Exceptions.
 * @link http://php.net/manual/en/class.exception.php
 */
class Exception implements Throwable {
    protected $message;
    protected $code;
    protected $file;
    protected $line;


    /**
     * Clone the exception
     * Tries to clone the Exception, which results in Fatal error.
     * @link http://php.net/manual/en/exception.clone.php
     * @return void
     * @since 5.1.0
     */
    final private function __clone() { }

    /**
     * Construct the exception. Note: The message is NOT binary safe.
     * @link http://php.net/manual/en/exception.construct.php
     * @param string $message [optional] The Exception message to throw.
     * @param int $code [optional] The Exception code.
     * @param Throwable $previous [optional] The previous throwable used for the exception chaining.
     * @since 5.1.0
     */
    public function __construct($message = "", $code = 0, Throwable $previous = null) { }

    /**
     * Gets the Exception message
     * @link http://php.net/manual/en/exception.getmessage.php
     * @return string the Exception message as a string.
     * @since 5.1.0
     */
    final public function getMessage() { }

    /**
     * Gets the Exception code
     * @link http://php.net/manual/en/exception.getcode.php
     * @return mixed|int the exception code as integer in
     * <b>Exception</b> but possibly as other type in
     * <b>Exception</b> descendants (for example as
     * string in <b>PDOException</b>).
     * @since 5.1.0
     */
    final public function getCode() { }

    /**
     * Gets the file in which the exception occurred
     * @link http://php.net/manual/en/exception.getfile.php
     * @return string the filename in which the exception was created.
     * @since 5.1.0
     */
    final public function getFile() { }

    /**
     * Gets the line in which the exception occurred
     * @link http://php.net/manual/en/exception.getline.php
     * @return int the line number where the exception was created.
     * @since 5.1.0
     */
    final public function getLine() { }

    /**
     * Gets the stack trace
     * @link http://php.net/manual/en/exception.gettrace.php
     * @return array the Exception stack trace as an array.
     * @since 5.1.0
     */
    final public function getTrace() { }

    /**
     * Returns previous Exception
     * @link http://php.net/manual/en/exception.getprevious.php
     * @return Exception the previous <b>Exception</b> if available
     * or null otherwise.
     * @since 5.3.0
     */
    final public function getPrevious() { }

    /**
     * Gets the stack trace as a string
     * @link http://php.net/manual/en/exception.gettraceasstring.php
     * @return string the Exception stack trace as a string.
     * @since 5.1.0
     */
    final public function getTraceAsString() { }

    /**
     * String representation of the exception
     * @link http://php.net/manual/en/exception.tostring.php
     * @return string the string representation of the exception.
     * @since 5.1.0
     */
    public function __toString() { }

    public function __wakeup() { }
}

```


> create_time: 2018-01-27 01:06:16
> modify_time: 2018-01-27 15:02:59