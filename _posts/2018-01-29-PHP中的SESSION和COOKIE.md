---
layout: post
title: PHP中的SESSION和COOKIE
categories: PHP
description: PHP中的SESSION和COOKIE
keywords: PHP, SESSION, Cookie
---

本来是在找IoC和DI的资料, 不知道怎么的, 进入了一个介绍 `session` 和 `cookie` 的文章.
浏览了一下. 觉得此文写的很好. 我在实际操作中, 使用的较少, 没有深入的理解. 所以, 锤他.

## 参考

[关于 PHP 中 Session 的几个问题](https://laravel-china.org/topics/1093/some-problems-about-session-in-php)

## PHP官方手册中的Session

> 通过为每个独立用户分配唯一的会话 ID，可以实现针对不同用户分别存储数据的功能。 会话通常被用来在多个页面请求之间保存及共享信息。 一般来说，会话 ID 通过 cookie 的方式发送到浏览器，并且在服务器端也是通过会话 ID 来取回会话中的数据。 如果请求中不包含会话 ID 信息，那么 PHP 就会创建一个新的会话，并为新创建的会话分配新的 ID。
>
> 会话的工作流程很简单。当开始一个会话时，PHP 会尝试从请求中查找会话 ID （通常通过会话 cookie）， 如果请求中不包含会话 ID 信息，PHP 就会创建一个新的会话。 会话开始之后，PHP 就会将会话中的数据设置到 $_SESSION 变量中。 当 PHP 停止的时候，它会自动读取 $_SESSION 中的内容，并将其进行序列化， 然后发送给会话保存管理器来进行保存。
>
> 默认情况下，PHP 使用内置的文件会话保存管理器（files）来完成会话的保存。 也可以通过配置项 session.save_handler 来修改所要采用的会话保存管理器。 对于文件会话保存管理器，会将会话数据保存到配置项 session.save_path 所指定的位置。
>
> 可以通过调用函数 session_start() 来手动开始一个会话。 如果配置项 session.auto_start 设置为1， 那么请求开始的时候，会话会自动开始。
>
> PHP 脚本执行完毕之后，会话会自动关闭。 同时，也可以通过调用函数 session_write_close() 来手动关闭会话。


>自定义会话管理器
>如果需要在数据库中或者以其他方式存储会话数据， 需要使用 session_set_save_handler() 函数来创建一系列用户级存储函数。 PHP 5.4.0 之后，你可以使用 SessionHandlerInterface 类 或者通过继承 SessionHandler 类来扩展内置的管理器， 从而达到自定义会话保存机制的目的。
>
>函数 session_set_save_handler() 的参数即为在会话生命周期内要调用的一组回调函数： open， read， write 以及 close。 还有一些回调函数被用来完成垃圾清理：destroy 用来删除会话， gc 用来进行周期性的垃圾收集。
>
>因此，会话保存管理器对于 PHP 而言是必需的。 默认情况下会使用内置的文件会话保存管理器。 可以通过 session_set_save_handler() 函数来设置自定义会话保存管理器。 一些 PHP 扩展也提供了内置的会话管理器，例如：sqlite， memcache 以及 memcached， 可以通过配置项 session.save_handler 来使用它们。
>
>会话开始的时候，PHP 会调用 open 管理器，然后再调用 read 回调函数来读取内容，该回调函数返回已经经过编码的字符串。 然后 PHP 会将这个字符串解码，并且产生一个数组对象，然后保存至 $_SESSION 超级全局变量。
>
>当 PHP 关闭的时候（或者调用了 session_write_close() 之后）， PHP 会对 $_SESSION 中的数据进行编码， 然后和会话 ID 一起传送给 write 回调函数。 write 回调函数调用完毕之后，PHP 内部将调用 close 回调函数。
>
>销毁会话时，PHP 会调用 destroy 回调函数。
>
>根据会话生命周期时间的设置，PHP 会不时地调用 gc 回调函数。 该函数会从持久化存储中删除超时的会话数据。 超时是指会话最后一次访问时间距离当前时间超过了 $lifetime 所指定的值。

[会话和安全](http://php.net/manual/zh/features.session.security.management.php)

这个是重点啊, 以前一直没留意过. 官网已经讲解了很多.

例如我们最常用的自动登录. 就很有必要使用一次性的登录信息. 以保证安全性.


## 需要注意的地方

> 千万不要使用 unset($_SESSION) 来复位超级变量 $_SESSION， 因为这样会导致无法继续在 $_SESSION 中注册会话变量。

> Note:
>
> 无论是通过调用函数 session_start() 手动开启会话， 还是使用配置项 session.auto_start 自动开启会话， 对于基于文件的会话数据保存（PHP 的默认行为）而言， 在会话开始的时候都会给会话数据文件加锁， 直到 PHP 脚本执行完毕或者显式调用 session_write_close() 来保存会话数据。 在此期间，其他脚本不可以访问同一个会话数据文件。
>
> 对于大量使用 Ajax 或者并发请求的网站而言，这可能是一个严重的问题。 解决这个问题最简单的做法是如果修改了会话中的变量， 那么应该尽快调用 session_write_close() 来保存会话数据并释放文件锁。 还有一种选择就是使用支持并发操作的会话保存管理器来替代文件会话保存管理器。

## 需要思考的问题

[安装 http://php.net/manual/zh/session.installation.php](http://php.net/manual/zh/session.installation.php)
> Note:
>
> 默认情况下，所有与特定会话相关的数据都被存储在由 INI 选项 session.save_path 指定的目录下的一个文件中。对每个会话会建立一个文件（不论是否有数据与该会话相关）。这是由于每打开一个会话即建立一个文件，不论是否有数据写入到该文件中。注意由于和文件系统协同工作的限制，此行为有个副作用，有可能造成用户定制的会话处理器（例如用数据库）丢失了未存储数据的会话。

**问题**

`注意由于和文件系统协同工作的限制，此行为有个副作用，有可能造成用户定制的会话处理器（例如用数据库）丢失了未存储数据的会话。`是什么意思?

---

[运行时配置 http://php.net/manual/zh/session.configuration.php](http://php.net/manual/zh/session.configuration.php)
> Warning
  如果将此设定为一个全局可读的目录，例如 /tmp（默认值），服务器上的其他用户有可能通过该目录的文件列表破解会话。

**问题**

如何破解? `/tmp`是744, 问题是, 服务器上的其他用户, 只的是什么? 能够登录到服务器的用户? 能够登录了, 就应该能够访问`/tmp`, 可读的, 还破解个P啊.

---

> session.gc_divisor integer
> session.gc_divisor 与 session.gc_probability 合起来定义了在每个会话初始化时启动 gc（garbage collection 垃圾回收）进程的概率。此概率用 gc_probability/gc_divisor 计算得来。例如 1/100 意味着在每个请求中有 1% 的概率启动 gc 进程。session.gc_divisor 默认为 100。

**问题**

感觉这个设置有严重的缺陷. 1/100的概率能够触发垃圾回收机制, 那么, 就又可能一直不触发. 还回收个毛啊?

---

> Note: 基于 URL 的会话管理比基于 cookie 的会话管理有更多安全风险。例如用户有可能通过 email 将一个包含有效的会话 ID 的 URL 发给他的朋友，或者用户总是有可能在收藏夹中存有一个包含会话 ID 的 URL 来以同样的会话 ID 去访问站点。

这个我没做过, 可以这么干? 老板会打死我的.

---

[Example #1 某单一用户的点击数](http://php.net/manual/zh/session.idpassing.php)

```php
<?php

session_start();

if (empty($_SESSION['count'])) {
   $_SESSION['count'] = 1;
} else {
   $_SESSION['count']++;
}
?>

<p>
Hello visitor, you have seen this page <?php echo $_SESSION['count']; ?> times.
</p>

<p>
To continue, <a href="nextpage.php?<?php echo htmlspecialchars(SID); ?>">click
here</a>.
</p>
```

问: 每个用户的`session_id` 肯定是不同的? 这个是怎么保证的?

答: php_session_create_id 此函数负责生成 session id. [C源码](https://raw.githubusercontent.com/php/php-src/master/ext/session/session.c)


## 代码实践

开始撸代码.

先来看一下我的session配置信息. 都是默认的. 我没有配置过.

![composer_error_php_version_2asdf_1session](/images/posts/composer_error_php_version_2asdf_1session.png)

先来一个简单的例子

```php
<?php
/**
 * Created by PhpStorm.
 * User: qiuyu
 * Date: 2018/1/29
 * Time: 下午10:54
 */

require dirname(__FILE__).'/../vendor/autoload.php';

session_start();

if (empty($_SESSION['count'])) {
    $_SESSION['count'] = 1;
} else {
    $_SESSION['count']++;
}

?>

<p>
    Hello visitor, you have seen this page <?php echo $_SESSION['count']; ?> times.
</p>

<p>
    To continue, <a href="nextpage.php?<?php echo htmlspecialchars(SID); ?>">click
        here</a>.
</p>
```

![composer_error_php_version_2asdf_1session_1](/images/posts/composer_error_php_version_2asdf_1session_1.png)

每次刷新页面, 都`count`都会加1. 证明保存了会话. 查看一下`cookie`的信息.

![composer_error_php_version_2asdf_1session_4](/images/posts/composer_error_php_version_2asdf_1session_4.png)

可以看到. 这个就是我设置的`session_name`了. 在php.info中可以看到, 就是这个名称.

再去服务器中保存`session`的路径去看看. `phpinfo`中显示, 位置是在`/Applications/MAMP/tmp/php`中.

```zsh
➜  ~ cd /Applications/MAMP/tmp/php
➜  php ll
total 24
-rwxrwxrwx  1 qiuyu  admin   3.7K  1 13 11:59 sess_799efe6414956f768109509d1278c01e
-rwxrwxrwx  1 qiuyu  admin    11B  1 29 23:06 sess_aced4c65f698f1d64b0737a9905114a7
-rw-------  1 qiuyu  admin    10B  1 29 23:03 sess_f5859e898c8cd40a3aecd55fb1b2ae8d
➜  php
```

可以看到. 这里面有`session`文件.

删掉试试.

```zsh
➜  php rm -rf ./*
zsh: sure you want to delete all the files in /Applications/MAMP/tmp/php/. [yn]? y
➜  php ll
➜  php
```

再次打开浏览器, 访问这个页面.

![composer_error_php_version_2asdf_1session_5](/images/posts/composer_error_php_version_2asdf_1session_5.png)

变回了1. 说明删除文件后, 就不能维持会话了. (废话)

再去找找其他的session函数玩玩.

1. session_cache_limiter()

    指定会话页面所使用的缓冲控制方法：

    当`session_cache_limiter('private')`时，用处是让表单`window.history.go(-1)`的时候，填写内容不丢失.

    这个会话与`header('cache-control:private,must_revalidate');`效果相同.

2. session_cache_expire()

    返回当前缓存的到期时间

3. session_id()

    获取/设置当前会话 ID, 获取的就是我用chrome插件查看到的那个值.

4. session_name()

    读取/设置会话名称, 获取的就是默认的`PHPSESSID`.

5. session_get_cookie_params()

    设置会话 cookie 参数, 包括: lifetime, path, domain, secure, httponly.

    需要注意, 这里面有坑. Cookie的生命周期不是用这个设置的. 详情看官网的文档. 第一个例子.

    [官网 代码例子](http://php.net/manual/zh/function.session-set-cookie-params.php#100657)

> create_time: 2018-01-30 00:11:25