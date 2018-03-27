---
layout: post
title: laravel中的monolog配置
categories: PHP
description: laravel中的monolog配置
keywords: PHP, Laravel, Monolog 
---

之前写过一个直接使用 monolog 这个扩展包的使用方法, Laravel 框架自带 monolog.
但是在 Laravel 中也做了一些封装, 使用起来和原版的 monolog 有一些不同. 
怎么自定义配置 Laravel 中的 monolog 呢? 特此记录. 


## 配置方法

只需要把下面的代码放在 `Laravel_project/bootstrap/app.php` 文件中. 

注意, 要放在 `return $app` 的前面.

### 代码

```php
/**
 * 自定义monolog.
 *
 * 支持邮件发送. 日志输出到终端. 每日都记录一个日志. 保存最近7天的日志.
 *
 * @author QiuYu
 */
$app->configureMonologUsing(function (Monolog\Logger $log) use ($argv, $argc) {
    // 如果是通过命令行传递的参数, 且参数是spider:today-deals, 则使用此自定义的日志配置.
    if (isset($argv[1]) && $argv[1] === 'spider:today-deals' && $argc === 3) {
        // 日志文件名称名称, 真实的日志文件会在这个字符串后追加日期, 如, Amazon-today-deals_home-2018-03-23.log
        $spider_name = str_replace('spider:', 'Amazon-', $argv[1] . '_' . $argv[2]);
        $filename = storage_path('/logs/' . $spider_name . '.log');

        // 创建日志Handle. 每日记录一个文件.
        $handler = new Monolog\Handler\RotatingFileHandler($filename, 7, \Monolog\Logger::DEBUG, true, 0664);
        $log->pushHandler($handler);

        // 创建日志Handle. 发送邮件的Handle. 错误级别大于等于 error, 就会发送邮件.
        $mailer = new \Swift_Mailer((new \Swift_SmtpTransport(config('mail.host'), config('mail.port')))->setUsername(config('mail.username'))->setPassword(config('mail.password')));
        $message = new \Swift_Message();
        $message->setFrom([config('mail.username') => 'promopure'])->setTo(config('mail.sendto'));
        $message->setSubject('警告, ' . $spider_name)->setBody('快点来看看这个情况, 需要快点处理一下.');
        $emailHander = new Monolog\Handler\SwiftMailerHandler($mailer, $message);
        $log->pushHandler($emailHander);

        // 附加的处理. Uid和Pid
        $log->pushProcessor(new \Monolog\Processor\UidProcessor());
        $log->pushProcessor(new \Monolog\Processor\ProcessIdProcessor());
        // 输出日志
        $log->pushProcessor(function ($record) {
            dump('[' . date("Y-m-d H:i:s", time()) . '] ' . $record['message']);
            if ($record['context']) {
                dump($record['context']);
            }
            return $record;
        });
    }
});
```

### 使用

```php
// ...

use Log;

// ...

Log::info('message');

```



