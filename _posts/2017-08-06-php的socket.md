---
layout: post
title: php的socket
categories: PHP
description: php的socket
keywords: PHP
---

项目需要网卡绑定多个ip, 抓取页面时指定使用哪个IP. 所以用worerman做了ip的代理. 

具体的workerman有官方手册.

客户端直接使用php自带的函数就可以了. 如下. 可以和workerman做的服务通讯. 

左侧是服务端, 右侧是客户端. 

![](/images/posts/15020274998242.jpg)



**客户端代码**

```
<?php

error_reporting(E_ALL);
set_time_limit(0);

$ip = '127.0.0.1';
$port = 22222;

// 发送的内容
$send = 'aaaa';

// 创建socket
$socket = socket_create(AF_INET,SOCK_STREAM,SOL_TCP);
if (!$socket) {
    die("socket_create() 失败的原因是:".socket_strerror(socket_last_error())."\n");
} else {
    echo "试图连接 '$ip' 端口 '$port'...\n";
}

// 连接socket
$connectRes = socket_connect($socket, $ip, $port);
if (!$connectRes) {
    die("socket_connect() 失败的原因是:".socket_strerror(socket_last_error($socket))."\n");
} else {
    echo "连接OK\n";
}

//写数据到socket缓存
if (!socket_write($socket, $send, strlen($send))) {
    die("socket_write() 失败的原因是:".socket_strerror(socket_last_error($socket))."\n");
} else {
    echo "发送的内容为:$send \n";
}

//读取指定长度的数据
$receive = socket_read($socket, 2048);
echo "接收服务器回传信息成功！\n";
echo "接收的内容为:".$receive." \n";

echo "关闭SOCKET...\n";
socket_close($socket);
echo "关闭OK\n";
```



**服务端代码**

```
<?php
ini_set('default_socket_timeout', -1);  //不超时

use \Workerman\Worker;

$configs = include_once(dirname(__FILE__)."/config.php");
require_once __DIR__ . '/Workerman/Autoloader.php';
include_once('./function.php');

$port = 22222;

// Create a TCP worker.
$worker0 = new Worker("tcp://127.0.0.1:".$port);
// 6 processes
$worker0->count = 1;
// Worker name.
$worker0->name = 'php-http-proxy'.$port;

// Emitted when data received from client.
$worker0->onMessage = function($connection, $data) {
    var_dump($data);
    $connection->send('receive success');
};

// Run.
Worker::runAll();
```


